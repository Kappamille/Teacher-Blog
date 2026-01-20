---
title: "Understanding the context of a dataset"
date: 2026-01-18T15:01:30-04:00
categories:
  - blog
tags:
  - Data Analysis
  - Teaching
  - Data context
---

When teaching Data Visualisation I always start with an introduction about the data context. Students are often very eager to get to the coding aspect of their data visualisation, they want to produce pretty graphs and find the most ground breaking insights from the data set. This often leads them to overlook and spend very little time analyzing the source of their data. Without understanding the context of your data your visualisations and insight can quickly become useless, misguided and biased. Let's look at an example together.

#### But first, what do I mean by data context ?

Data context are informations surrounding the collection of the data. I often use the following questions as a checklist so students can ensure they have the proper information before startting their data exploration.

- What data was collected ? Do we have a proper description of what each column means ?
- When was the data collected ?
- Who collected the data ? Who financed the collection of the data ?
- For which purpose was the data collected ?
- How was the data collected ?
- What is the licence of the data set ?
- How much data was collected ?

In class I assign the following dataset, found on Kaggle, about [air quality and water pollution](https://www.kaggle.com/datasets/cityapiio/world-cities-air-quality-and-water-polution). I choose this dataset purposefully as the context of the data is possible to find but requires some amount of searching (and beacause the dataset only has 5 columns which makes the subsequent data analysis less daunting).

In their assignments, most students often stick to the information given on Kaggle. They tell me that "air quality varies from 0 (bad quality) to 100 (top good quality) and water pollution varies from 0 (no pollution) to 100 (extreme pollution)".
{% include image.html
   src="/assets/images/blogposts/about-dataset.png"
   alt="Kaggle Author CityAPI-IO"
   caption="screenshot from Kaggle"
   align="center"
   width="100%" %}

They would continue by saying that "the data was collected as part of the City API project" and that "CityAPI-IO" is the author of the dataset.
{% include image.html
   src="/assets/images/blogposts/author.png"
   alt="Kaggle Author CityAPI-IO"
   caption="screenshot from Kaggle"
   align="center"
   width="35%" %}

That the licence of the data is "CC0: Public Domain".
{% include image.html
   src="/assets/images/blogposts/license.png"
   alt="Kaggle license screenshot"
   caption="screenshot from Kaggle"
   align="center"
   width="30%" %}

That the data is from is 2020.
{% include image.html
   src="/assets/images/blogposts/title.png"
   alt="Kaggle license screenshot"
   caption="screenshot from Kaggle"
   align="center"
   width="100%" %}

And, finally, that there are 3796 data points
{% include image.html
   src="/assets/images/blogposts/city-column.png"
   alt="Kaggle license screenshot"
   caption="screenshot from Kaggle"
   align="center"
   width="30%" %}

However if we take a few more seconds to think about these informations our internal bell should ring.
**What does it actually mean to have an air quality of 100 or a water pollution of 0?**
**Who or what is CityAPI-IO ?**
**Is it realistic that this dataset would have a public domain license ?** **How can I be sure that the data is from 2020?**

Let start by verifying the link in the description of the dataset, it links to [https://city-api.io/](https://city-api.io/) which ... is not an attributed website. We continue searching and see if a google search for city api io provides more information. Unfirtunately ae only find the websites that point back to the Kaggle account city-api.io.

Let's change our strategy, the description mentions that the data was initially taken from Numbeo. After a google search we find that the website [https://www.numbeo.com/](https://www.numbeo.com/) does exists. In the menu under "pollution" we find that they have information related to air quality and water pollution. Numbeo seems to be a platform where users can self report their perception about their quality of life. We find that they have an Air Pollution index and a Water Quality index for several cities in the world that are between 0 and 100. This seems to match the pattern of our data on Kaggle and thus seems to be our data source.

With further research on their website we find that the numercial data originally stems from a questionnaire. The two questions related to air quality and water pollution are the following:
{% include image.html
   src="/assets/images/blogposts/numbeo-questions.png"
   alt="numbeo questions"
   caption="https://www.numbeo.com/pollution/form.jsp?country=France&city=Paris&returnUrl=https%3A%2F%2Fwww.numbeo.com%2Fpollution%2Fin%2FParis"
   align="center"
   width="100%" %}

This raises the question as to how the categorical answers from the questions were converted to a numerical index. When clicking on the information icon for the pollution index we find the information we're searching for.
We find that _"Individual [Survey] responses are assigned a numerical value between -2 (indicating a strongly negative perception) and +2 (indicating a strongly positive perception).
Survey results are then scaled from 0 to 100 for easier interpretation and comparison. Our current index, updated continuously, is compiled from data within the past 5 years. We carefully select cities for inclusion in the index based on a minimum number of contributors to ensure statistical significance."_ [https://www.numbeo.com/pollution/indices_explained.jsp](https://www.numbeo.com/pollution/indices_explained.jsp)

This means that a an answer _"Very Satisfied"_ would be translated to 100, _"Neutral"_ to 50 and _"Very Dissatisfied"_ to 0.
Although this method of conversion in questionable in itself, this is a crucial information, as values of 100 and 0 should not be considered outliers.
However their _"minim[al] number of contributors to ensure statistical significance"_ is unclear. During our data exploration we'll find that the minimal number is probably only 1 person as we have many values that exactly equal to 0, 25, 50, 75 and 100.

Let's continue with our question regarding the license of the data. In the [terms of use](https://www.numbeo.com/common/terms_of_use.jsp) of the website we find that the use of Numbeo data for personal or academic is free but attribution is necessary. So the licence is not public domain as metionned on Kaggle (as public domain does not need attribution).

Another important note in the terms of use is _"Please be advised that nothing found here has necessarily been reviewed by people with the expertise required to provide you with complete, accurate or reliable information. Use our content at your own risk"_. This is crucial as the air quality and water pollution indexes have absolutelty no links to scientific measures of air quality (such as particulate matter PM10 and PM2.5, nitrogen dioxide NO2, sulfur dioxide SO2, ozone O3 and carbon monoxide CO) or water quality (such as microbial, chemical and radicological presences).

In the title of the kaggle dataset, the year 2020 is pointed out. However in the name of the dataset the day 18/10/2021 is presented. Since the date of the 18/10/2021 seems to be more precise and kaggle indicates that this dataset was published 4 years ago plus that the author on kaggle isn't the official numbeo account, I would tend to assume that the data was scraped from the numbeo website on 18/10/2021. Futhermore the Numbeo wesbite mentions that _"[indexes are] compiled from data within the past 5 years"_ Which would mean that our data has been collected between 18/10/2016 and 18/10/2021.

{% include image.html
   src="/assets/images/blogposts/dates.png"
   alt="numbeo questions"
   caption=""
   align="center"
   width="100%" %}

Finally, checking that there are 3796 data points is pretty straightfoward as kaggle already has some integrated tool to check for the uniqueness of values. But while searching for information on the Numbeo website we find that they already have a map thay showcases the cities for which values were entered. We can already gain a glace at the distribution of data and see that countires and regions across the world are not represented equally.
{% include image.html
   src="/assets/images/blogposts/map.png"
   alt="numbeo questions"
   caption=""
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
- **For which purpose was the data collected ?** The data is collected for information purposes to _"explore, share, and compare information"_ but also to provide users with the option to pay for a license with more extensive data.
- **How was the data collected ?** The data was collected through an online form, though self reporting from users in two single choice questions using a likert scale.
- **What is the licence of the data set ?** The license is free to use for personal and acadmic use, granted attribution. Users can also purchase a commercial license.
- **How much data was collected ?** 3796 unique cities are present in the dataset. The dataset is moslty comprised of cities from Europe, North America and India. Data is very sparse for Africa, the Middle East and South America.

The initial misguided data context from a stduent might lead them to produce a graph like the following.
< TO DO add bar chart of the average air quality in france and average water pollution in france >

Whereas with a precise understanding of our data context we might produce a more accurate graph
< TO DO add box plot of the distribution of the perception of air quality and water quality in france >
< the data context allows us to choose a more precise chart type, highlight in the title that we talk about the perception of water and not a scientific measure, show outliers, and show what we don't know in the description of the data>

To conclude, I think it's essential to highlight that rigorous data analysis starts before writing any line of code. I would roughly advise to spend 10% to 20% of your time to check the context of your data rigorously, even if the source of the data is trusted.
