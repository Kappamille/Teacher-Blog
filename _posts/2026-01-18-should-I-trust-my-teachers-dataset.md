---
title: "Should I trust my teacher's dataset ?"
date: 2026-01-18T15:01:30-04:00
categories:
  - blog
tags:
  - Data Analysis
  - Teaching
  - Data Context
---

When teaching Data Visualisation I always start with an introduction about the data context. Students are often very eager to get to the coding aspect of their data visualisation, they want to produce pretty graphs and find the most ground breaking insights from the data set. This often leads them to spend very little time analyzing the source of their data. Without understanding the context of your data your visualisations and insight can quickly become useless, misguided and biased. Let's look at an example together.

#### But first, what do I mean by data context ?

Data context are informations surrounding the collection of the data. I often use the following questions as a checklist so students can ensure they have the proper information before starting their data exploration.

- What data was collected ? Do we have a proper description of what each column means ?
- When was the data collected ?
- Who collected the data ? Who financed the collection of the data ?
- For which purpose was the data collected ?
- How was the data collected ?
- What is the licence of the data set ?
- How much data was collected ?

In class I assign the following dataset, found on Kaggle, about [air quality and water pollution](https://www.kaggle.com/datasets/cityapiio/world-cities-air-quality-and-water-polution). I choose this dataset purposefully as the context of the data is possible to find but requires some amount of searching and because the dataset is small with only 5 columns .

In their assignments, most students often stick to the information given on Kaggle. They would write a very short introduction and directly start with their data exploration. They tell me that "air quality varies from 0 (bad quality) to 100 (top good quality) and water pollution varies from 0 (no pollution) to 100 (extreme pollution)".
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/about-dataset.png"
   alt="Kaggle description of dataset"
   caption="screenshot from Kaggle"
   align="center"
   width="100%" %}

They would continue by saying that "the data was collected as part of the City API project" and that "CityAPI-IO" is the author of the dataset.
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/author.png"
   alt="Kaggle Author CityAPI-IO"
   caption="screenshot from Kaggle"
   align="center"
   width="35%" %}

That the licence of the data is "CC0: Public Domain".
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/license.png"
   alt="Kaggle license screenshot"
   caption="screenshot from Kaggle"
   align="center"
   width="30%" %}

That the data is from is 2020.
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/title.png"
   alt="Kaggle screenshot of the page title"
   caption="screenshot from Kaggle"
   align="center"
   width="100%" %}

And, finally, that there are 3796 data points
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/city-column.png"
   alt="Kaggle number of datapoints"
   caption="screenshot from Kaggle"
   align="center"
   width="30%" %}

However if we take a few more seconds to think about these informations our internal bell should ring.
**What does it actually mean to have an air quality of 100 or a water pollution of 0?**
**Who or what is CityAPI-IO ?**
**Is it realistic that this dataset would have a public domain license ?** **How can I be sure that the data is from 2020?**

Let's start by verifying the link in the description of the dataset, it links to [https://city-api.io/](https://city-api.io/) which ... is not an attributed website. We continue searching and see if a google search for city api io provides more information. Unfortunately we only find the websites that point back to the Kaggle account city-api.io.

Let's change our strategy, the description mentions that the data was initially taken from Numbeo. After a google search we find that the website [https://www.numbeo.com/](https://www.numbeo.com/) does exists. In the menu under "pollution" we find that they have information related to air quality and water pollution. Numbeo seems to be a platform where users can self report their perception about their quality of life. We find that they have an Air Pollution index and a Water Quality index for several cities in the world that are between 0 and 100. This seems to match the pattern of our data on Kaggle and thus seems to be our data source.

With further research on their website we find that the numercial data originally stems from a [questionnaire](https://www.numbeo.com/pollution/form.jsp?country=France&city=Paris&returnUrl=https%3A%2F%2Fwww.numbeo.com%2Fpollution%2Fin%2FParis). The two questions related to air quality and water pollution are the following:
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/numbeo-questions.png"
   alt="numbeo questions"
   caption="screenshot from Numbeo"
   align="center"
   width="100%" %}

This raises the question as to how the categorical answers from the questions were converted to a numerical index. When clicking on the information icon for the pollution index we find the information we're searching for.
We find that _"Individual [Survey] responses are assigned a numerical value between -2 (indicating a strongly negative perception) and +2 (indicating a strongly positive perception).
Survey results are then scaled from 0 to 100 for easier interpretation and comparison. Our current index, updated continuously, is compiled from data within the past 5 years. We carefully select cities for inclusion in the index based on a minimum number of contributors to ensure statistical significance."_ [https://www.numbeo.com/pollution/indices_explained.jsp](https://www.numbeo.com/pollution/indices_explained.jsp)

This means that a an answer _"Very Satisfied"_ would be translated to 100, _"Neutral"_ to 50 and _"Very Dissatisfied"_ to 0.
So values of 100 and 0 should not be considered outliers.

Overall, this method of conversion is very questionable as the data is inherently categorical and looses it's meaning when aggregated. Indeed what does an air quality value of 58.3 mean ? Does it mean that the quality is _neutral_ as it's the category it's closest to ? How many people answered the questionnaire ? If several people answered the questionnaaire how many people found the air quality satifying, neural or dissatisfing ?
To add to this their _"minim[al] number of contributors to ensure statistical significance"_ is unclear. Indeed if we have a value of 50 for a specific city, we don't know if this value stems from one questionnaire or from the average of multiple questionnaire answers.
If we do a quick data exploration of our data we clearly see that the majority of the answers fall on exact numbers (0,25,50,75,100) for both the air quality and the water pollution. We also see that, overall, around half the datapoints falls on exact numbers for both air quality and water pollution. This could hint to the fact that the _minimal number of contibutors is probably only 1 person_

<div style="width:100%; max-width:1200px; margin:auto; aspect-ratio: 3 / 2;">
  <iframe
    src="{{ '/assets/images/blogposts/should-I-trust-my-teachers-dataset/air_water_quality.html' | relative_url }}"
    style="width:100%; height:100%; border:0;">
  </iframe>
</div>v

Let's continue with our question regarding the license of the data. In the [terms of use](https://www.numbeo.com/common/terms_of_use.jsp) of the website we find that the use of Numbeo data for personal or academic is free but attribution is necessary. So the licence is _not_ public domain as metionned on Kaggle (as public domain does not need attribution).

Another important note in the terms of use is _"Please be advised that nothing found here has necessarily been reviewed by people with the expertise required to provide you with complete, accurate or reliable information. Use our content at your own risk"_. This is crucial as the air quality and water pollution indexes have absolutelty no links to scientific measures of air quality (such as particulate matter PM10 and PM2.5, nitrogen dioxide NO2, sulfur dioxide SO2, ozone O3 and carbon monoxide CO) or water quality (such as microbial, chemical and radicological presences).

In the title of the Kaggle dataset, the year 2020 is pointed out. However in the name of the dataset the day 18/10/2021 is presented. Since the date of the 18/10/2021 seems to be more precise and Kaggle indicates that this dataset was published 4 years ago, plus that the author on Kaggle isn't the official numbeo account, I would tend to assume that the data was scraped from the numbeo website on 18/10/2021. Futhermore the Numbeo wesbite mentions that _"[indexes are] compiled from data within the past 5 years"_ Which would mean that our data has been collected between 18/10/2016 and 18/10/2021.

{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/dates.png"
   alt="conflicting dates screenshot"
   caption=""
   align="center"
   width="100%" %}

Finally, checking that there are 3796 data points is pretty straightfoward as Kaggle already has some integrated tool to check for the uniqueness of values. But while searching for information on the Numbeo website we found a map thay showcasing the values for all cities. We can already gain a glance at the distribution of data and see that countires and regions across the world are not represented equally.
{% include image.html
   src="/assets/images/blogposts/should-I-trust-my-teachers-dataset/map.png"
   alt="numbeo map of data points"
   caption="Screenshot from numbeo"
   align="center"
   width="100%" %}

With these information we can try and draft a more thorough understanding of our dataset:

- **What data was collected ?** Users self reported on how satisfied they are with the quality of the air and how concerned they are with the water pollution of their city. Categorical answers were transleted to a numerical value:
  - "Very Satisfied" or "Very Concerned" to 100
  - "Somewhat Satisfied" or "Somewhat Concerned" to 75
  - "Neutral" to 50
  - "Somewhat Dissatisfied" or "Somewhat Unconcerned" to 25
  - "Very Dissatisfied" or "Not Concerned" at all to 0
  - We assume that the category "Not Sure" was excluded from the dataset.
- **Do we have a proper description of what each column means ?**
  - The `AirQuality` and `WaterQuality` columns are the average of the user reporting. The number of participants for each city is unknown.
  - The `City`, `Region` and `Country` columns correspond to the geographical details of the city for which the questions were answered.
- **When was the data collected ?** The data was likely scraped on the 18/10/2021 from the Numbeo website and is comprised of data from the past 5 years.
- **Who collected the data ?** Numbeo provided the infrastructure for data collection and users filled in their forms based on voluntary participation. The accuracy of the content was not reviewed.
- **For which purpose was the data collected ?** The data is collected to _"explore, share, and compare information"_ but also to provide users with the option to pay for a license with more extensive data.
- **How was the data collected ?** The data was collected through an online form, though self reporting from users in two single choice questions using a likert scale.
- **What is the licence of the data set ?** The license is free to use for personal and acadmic use, granted attribution. Users can also purchase a commercial license.
- **How much data was collected ?** 3796 unique cities are present in the dataset. The dataset is moslty comprised of cities from Europe, North America and India. Data is very sparse for Africa, the Middle East and South America.

So, **should I trust the dataset given by the teacher ?** No. every dataset needs to be check for rigorousness before use, even if given by a figure of authority, in our case the teacher. It's essential to highlight that rigorous data analysis starts before writing any line of code. I would roughly advise to spend 10% to 20% of the time to check the context of the data carefully.

And finally, **should I use the air quality and water pollution dataset from Kaggle ?** With what we found out about the dataset, most likely not. The data is not very informative because of how it's been transformed from a qualitative answer to a quantitative answer and the data is probably on the Kaggle platform without Numbeo's awareness. If the students had spend a little more time checking the data context of their dataset, they should have told me that the dataset is not good enough to perform an analysis on it !

Code for the data visualisation can be found [here](https://github.com/Kappamille/Teacher-Blog/assets/images/blogposts/should-I-trust-my-teachers-dataset/should-I-use-my-teachers-dataset-dataviz.ipynb)
