+++
date = "2016-03-20 17:07:40+00:00"
draft = false
tags = ["django", "python"]
title = "Permissions in Django Admin"
url = "/post/141377389440/permissions-in-django-admin"
+++
<a href="https://docs.djangoproject.com/en/1.9/ref/contrib/admin/" target="_blank">Admin dashboard</a> is one of the Django’s useful feature. Admin dashboard allows super users to `` create, read, update, delete `` database objects. The super users have full control over the data. `` Staff `` user can login into admin dashboard but can’t access data. In few cases, `` staff `` users needs restricted access . `` Super user `` can access all data from various in built and third party apps. Here is a screenshot of `` Super user `` admin interface after login.

<figure class="tmblr-full" data-orig-height="999" data-orig-width="684"><img data-orig-height="999" data-orig-width="684" src="https://66.media.tumblr.com/f4d9901583f8f69e58ef014fe12a23bb/tumblr_inline_o4ckudpdvy1qc390z_540.png"/></figure>

Staff users don’t have access to data.

<figure class="tmblr-full" data-orig-height="943" data-orig-width="1198"><img data-orig-height="943" data-orig-width="1198" src="https://66.media.tumblr.com/4897ce3f1f0b12bc3816b7bc8c26d83d/tumblr_inline_o4cktwItDQ1qc390z_540.png"/></figure>

__Allow staff user to access models__

<a href="https://docs.djangoproject.com/en/1.9/topics/auth/default/#topic-authorization" target="_blank">Django permissions</a> determines access to models and allowed actions in admin interface. Every model has three permissions. They are `` <app_label>.add_<model> ``, `` <app_label>.change_<model> ``, `` <app_label>.delete_<model> `` allows user to `` create, edit `` and `` delete `` objects.

`` API `` and `` Admin interface `` allows assigning permissions to the user.

<figure class="tmblr-full" data-orig-height="461" data-orig-width="1208"><img data-orig-height="461" data-orig-width="1208" src="https://66.media.tumblr.com/f63557b7fa4ea61c618df62c032ea74a/tumblr_inline_o4ckta9BDv1qc390z_540.png"/></figure>

<figure class="tmblr-full" data-orig-height="415" data-orig-width="1154"><img data-orig-height="415" data-orig-width="1154" src="https://66.media.tumblr.com/52a314dff3cc8c0a483e91fa8ac22176/tumblr_inline_o4ckssZhUe1qc390z_540.png"/></figure>

Staff user can perform various tasks on allowed models after assigning permissions.

<figure class="tmblr-full" data-orig-height="347" data-orig-width="1041"><img data-orig-height="347" data-orig-width="1041" src="https://66.media.tumblr.com/27755be91479ed665141e7c982805b1b/tumblr_inline_o4cks6Zfz31qc390z_540.png"/></figure>

__Filtering objects in model__

Conference management system hosts many conferences in a single instance. Each conference has different set of moderators. System allows only conference specific moderators to access the data. To achieve the functionality, Django provides an option to override `` queryset ``. Admin requires custom implementation of `` get_queryset `` method. Here is how a sample code looks like.

    class ConferenceAdmin(AuditAdmin):
        list_display = ('name', 'slug', 'start_date', 'end_date', 'status') + AuditAdmin.list_display
        prepopulated_fields = {'slug': ('name',), }

        def get_queryset(self, request):
            qs = super(ConferenceAdmin, self).get_queryset(request)
            if request.user.is_superuser:
                return qs
            return qs.filter(moderators=request.user)

    class ConferenceProposalReviewerAdmin(AuditAdmin, SimpleHistoryAdmin):
        list_display = ('conference', 'reviewer', 'active') + AuditAdmin.list_display
        list_filter = ('conference',)

        def get_queryset(self, request):
            qs = super(ConferenceProposalReviewerAdmin, self).get_queryset(
            request)
            if request.user.is_superuser:
                return qs
            moderators = service.list_conference_moderator(user=request.user)
            return qs.filter(conference__in=[m.conference for m in moderators])

Filtered moderator objects for staff user.

<figure class="tmblr-full" data-orig-height="921" data-orig-width="1599"><img data-orig-height="921" data-orig-width="1599" src="https://66.media.tumblr.com/16b1c22f92ce8e2fbdb5de3d40e1510f/tumblr_inline_o4ckrjEwGi1qc390z_540.png"/></figure>

Unfiltered moderator objects for superusers.

<figure class="tmblr-full" data-orig-height="915" data-orig-width="1604"><img data-orig-height="915" data-orig-width="1604" src="https://66.media.tumblr.com/e190d51fc6c2cc0d5da325a9a7b4c7fe/tumblr_inline_o4ckr2rMre1qc390z_540.png"/></figure>

Note the difference in total number of objects (23, 30) in the view.
