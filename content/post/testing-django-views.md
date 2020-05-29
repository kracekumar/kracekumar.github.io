+++
date = "2016-02-01 19:17:49+00:00"
draft = false
tags = ["django", "testing"]
title = "Testing Django Views"
url = "/post/138492827565/testing-django-views"
+++
Majority of web frameworks promote <a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller" target="_blank">MVC/MTV</a> software pattern. The way web applications are designed today aren’t same as 5-6 years back. Back then it was server side templates, HTML, API’s weren’t widespread and mobile apps were becoming popular. Rise of mobile and Single Page Application shifted majority of web development towards API centric development. Testing API is super simple with data in and data out, but testing a django view in classic web application is difficult since HTML is returned. <a href="https://en.wikipedia.org/wiki/Representational_state_transfer" target="_blank">REST</a> semantics and `` status code `` helped to distinguish response without inspecting body.

`` View `` is the co ordinator in the web application. Writing integration tests makes sense for views. While writing integration tests most of the assertions are around `` permission, status code, returned data ``. Django provides set of nice helpers to test the views like <a href="https://docs.djangoproject.com/en/1.9/topics/testing/tools/#django.test.SimpleTestCase.assertTemplateUsed" target="_blank">assertTemplateUsed</a>, <a href="https://docs.djangoproject.com/en/1.9/topics/testing/tools/#django.test.SimpleTestCase.assertRedirects" target="_blank">assertRedirects</a>. The response returned contains the template `` context ``. All three are very handy to test majority of the view functionality and super helpful.

#### Sample test cases

<script src="https://gist.github.com/kracekumar/59f8d68bc39947148956.js"></script>

More of these tests can be found in <a href="https://github.com/pythonindia/junction/tree/master/tests/integrations" target="_blank">junction</a>.
