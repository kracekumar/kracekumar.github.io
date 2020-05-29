
+++
date = "2020-04-20 19:56:04+00:00"
draft = false
tags = ["covid-19", "data analysis"]
title = "Incomplete data is useless - COVID-19 India data"
url = "/post/615946721097285632/incomplete-data-is-useless-covid-19-india-data"
+++
The data is a representation of reality. When a value is missing in the piece of data, it makes it less useful and reliable. Every day, articles, a news report about COVID-19 discuss the new cases, recovered cases, and deceased cases. This information gives you a sense of hope or reality or confusion.

Regarding COVID-19, everyone believes or accepts specific details as fact like mortality rate is `` 2 to 3 percent ``, over the age of fifty, the chance of death is `` 30 to 50 percent ``. These are established based on previously affected places, and some details come out of the simulation. The mortality rate, deceased age distribution, patient age distribution, mode of spread differs from region to country. With accurate and complete data, one can understand the situation, make the decision, and update the facts.

<a href="https://api.covid19india.org/" target="_blank">Covid19India</a> provides an API to details of all the COVID-19 cases. The dataset for the entire post comes from an API response on 18th April 2020. The APIs endpoints for the analysis are <code><a href="https://api.covid19india.org/raw_data.json" target="_blank">https://api.covid19india.org/raw_data.json</a></code>, and <code><a href="https://api.covid19india.org/deaths_recoveries.json" target="_blank">https://api.covid19india.org/deaths_recoveries.json</a></code>.

When one moves away cumulative data like total cases to specific data like age, gender, the details are largely missing and makes it difficult to comprehend what’s going in the state. For all patients in India, only `` 11% `` of age brackets and `` 19.2% `` of gender details are available.

<figure class="tmblr-full" data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_patients_field_details.png" data-orig-width="700"><img data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_patients_field_details.png" data-orig-width="700" src="https://66.media.tumblr.com/c55c0b6c9281913ca08445b2f41f2448/5f9aca6138e8031e-36/s540x810/1867eb78cd3697d8f4a20e46913bb73582c9ae3d.png"/></figure>

Analyzing missing data (empty value = “) state-wise reveals how each state reveals data. The below image is a comparison of missing data for the state of Karnataka and Maharashtra.

<figure class="tmblr-full" data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_patients_field_details_karnataka.png" data-orig-width="700"><img data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_patients_field_details_karnataka.png" data-orig-width="700" src="https://66.media.tumblr.com/e42d829d66c17e0de67265fd39a8564b/5f9aca6138e8031e-5e/s540x810/abd55514ff56a96d54b7d95ad3283023b1049845.png"/></figure>

<figure class="tmblr-full" data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_patients_field_details_maharashtra.png" data-orig-width="700"><img data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_patients_field_details_maharashtra.png" data-orig-width="700" src="https://66.media.tumblr.com/865422ab789890d6c97b6284f9a8f214/5f9aca6138e8031e-f7/s540x810/94901b5ce08528b9c8066ecd4e974332e5344926.png"/></figure>

Looking further into each case, it’s clear, <a href="https://twitter.com/ANI/status/1251399310270279683" target="_blank">Karnataka officials</a> release date at the individual level such as age bracket, gender, other details in a tabular format(not machine-readable) compared to the <a href="https://twitter.com/rajeshtope11/status/1251194830962724865?s=19" target="_blank">State of Maharashtra</a>. <a href="https://arogya.maharashtra.gov.in/pdf/epressnote31.pdf" target="_blank">Maharashtra</a> releases only cumulative data like the total number of new cases, the total number of recovered patients. Each state follows its format(not so useful). Next to Karnataka, Andhra Pradesh data contains more than `` 50% `` of age bracket and gender. The rest of the states except Kerala, to a certain extent, all have close to `` 90% `` of _missing data_ for gender and age bracket.

With missing data, we can’t identify which age group is dying in Maharashtra, which age group is most affected, does deceased age group vary across all states, is there a state where a considerable amount of young people are dying?

### Karnataka Case

<figure class="tmblr-full" data-orig-height="450" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/histogram_of_all_cases_in_karnataka.png" data-orig-width="700"><img data-orig-height="450" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/histogram_of_all_cases_in_karnataka.png" data-orig-width="700" src="https://66.media.tumblr.com/df32ef17c874fefd3d37aae55c6fd863/5f9aca6138e8031e-d0/s540x810/dcb3ac70b4b1f724b1ac358e869ef905abf82b3c.png"/></figure>

If we divide the age bracket in the range of ten like `` 40 - 50 ``, all the age groups are more or less equally affected. There is no significant variation. You can also use arbitrary age groups like `` 0 to 45, 45 to 60, 60 to 75, 75+ `` as mentioned in the <a href="https://twitter.com/EconomicTimes/status/1251484936889970689" target="_blank">tweet</a>.

<figure class="tmblr-full" data-orig-height="500" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/age_group_of_all_cases_in_karnataka.png" data-orig-width="700"><img data-orig-height="500" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/age_group_of_all_cases_in_karnataka.png" data-orig-width="700" src="https://66.media.tumblr.com/26a02d34e5dfd5cca055d629fa6fec52/5f9aca6138e8031e-f5/s540x810/9b733a16955aa67d5c674d19e4361324a652700b.png"/></figure>

The `` raw_data.json `` API provides the status of the patients. The patient’s status change numbers don’t be match compared to the primary web application(covid19india.org). My guess is because of API update frequency.

<figure class="tmblr-full" data-orig-height="450" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/histogram_of_recovered_patients_karnataka.png" data-orig-width="700"><img data-orig-height="450" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/histogram_of_recovered_patients_karnataka.png" data-orig-width="700" src="https://66.media.tumblr.com/41e4525c4cb32a23e29a477e0202d56c/5f9aca6138e8031e-68/s540x810/e80b7f619ccc06778d9b232def28504e9f99f6a3.png"/></figure>

The patients in the age group 0-10 take more time to recover compared to the rest of the age group. The age group 10-20 less time to recover compared to the rest of the group. The two issues here. First, the dataset is small, only 60 cases. Second, the accuracy of dates makes a considerable difference; i.e `` dateannounced `` and `` statuschangedata `` in the API is crucial. If the data is available for the top three affected states like Maharashtra, Delhi, Gujarat, it would reveal which age group and gender are recovering fast and deceasing, in how many days the acute patients die.

### Deceased Case

From the `` deaths_recoveries.json `` API, the age bracket is available for `` 39.6% `` of deceased cases. In Karnataka for

<figure class="tmblr-full" data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_vs_available_of_deceased.png" data-orig-width="700"><img data-orig-height="400" data-orig-src="http://anthology.kracekumar.com/images/18th_april_covid19_analysis/missing_vs_available_of_deceased.png" data-orig-width="700" src="https://66.media.tumblr.com/010d390803f6e1be9d48f3361afffdca/5f9aca6138e8031e-5c/s540x810/e2f1a107c81e45c4fbe53c53109da0e2ad22c814.png"/></figure>

In the available data, the most number of deaths are in the age-group, `` 40-50 (44 deaths), and 20-30 (25 deaths) ``. To get a better picture, one needs to compare with the population distributions as well. Without complete details, it’s challenging to say the age group `` 20-30 `` mortality rate is `` 0.1% ``.

The mere number of deceased patients doesn’t represent anything close to reality. Out of `` 13 deaths `` in Karnataka, age bracket and gender are available for 10 cases. All deceased cases are in the age bracket of `` 65 to 80 ``, two females and eight males.

The small data suggests all corona related deaths happen with seven days of identification. Is this true for all states?

### Conclusion

I’m aware; volunteers maintain the API. Their effort requires a special mention, and by analyzing the two sets of APIs, it’s clear how hard it is to mark patients’ status change. When the Government doesn’t release the unique patient id, it’s confusing, and local volunteer intuition and group knowledge takes over.

Every data points help us to know more about the pandemic. All the state governments need to release the complete data in a useable format. Remember, age and gender are necessary information, with more details like the existing respiratory condition, hospital allocated, the model can help in prioritizing the resources later. There is no replacement for testing and testing early. Without clean data, it’s impossible to track the epidemic and associated rampage. We haven’t seen the cases for re-infection yet. Several other factors contribute to one’s survival, like a place of residence, access to insurance, socioeconomic status. We haven’t moved further from numbers, the details like how crucial is a patient when identified, how each patient is recovering are never released in public.

By furnishing the incomplete data, the Government denies our choice of making an informed decision, and there is no independent verification for the claims on the pattern of the spread. In the prevailing conditions, the only way to understand the scenario is both qualitative and quantity (data) stories.

Code: <a href="https://github.com/kracekumar/covid19_india/blob/master/Data%20Analysis%2018th%20April.ipynb" target="_blank">Age Analysis</a>, <a href="https://github.com/kracekumar/covid19_india/blob/master/Death%20Recoveries.ipynb" target="_blank">Death Recoveries</a>
