# NespressoMetropolisCustomerReviewAnalysis
**Kunal Jeshang - Coffee Specialist** _(Project Timeline: January 2023 to May 2023)_
> Report/ReadMe in Progress

## Introduction
This project was created to assist the Nespresso Metrotown Boutique understand its service quality in terms of customer experience and optimal execution of business operations. As management of the Boutique predominantly refers to Google Reviews to periodically assess service quality, the data utilized for this project were Google Reviews received between 2019 to 2022. Thus, reviews received in 2023 were not considered for analysis. In turn, this project is a cumulative annual analysis of our service quality until 2022 year-end.

Below is a list of the five stages the project followed through.
1. Data Scraping/Extraction
2. Data Cleaning
3. Exploratory Data Analysis
4. Sentiment Analysis - Exploration
5. Sentiment Analysis - Predictive Modeling

This project was completed using the following devices, tools, and technologies.
* Devices & Platforms
    * ASUS VivoBook - Windows 11 (8GB RAM); used for data scraping/extraction
    * Macbook Air 2014 - Mac OS X BigSur (4GB RAM); used for all stages _after_ data scraping/extraction
* Scripting
    * Programming Language: Python
        * Official Version 3.10.1 using Asus VivoBook; used for data scraping/extraction
        * Anaconda Navigator version (3.9.11) using Macbook Air 2014; used for all stages _after_ data scraping/extraction
    * Important Packages: Selenium (with ChromeDriver.exe), Datetime/Time/Dateutil, Pandas, Numpy, Re, Matplotlib, Seaborn, WordCloud, TQDM, Itertools, NLTK, String, TextBlob, NRCLex, Scikit Learn
* Development Environment
    * Jupyter Notebook 
    * Software 
        * Microsoft Visual Studio Code; used for data scraping/extraction
        * Anaconda Navigator; used for all stages _after_ data scraping/extraction

## Data Scraping/Extraction

This is the first stage of the project but it is also the hardest. Despite customer Google Reviews being freely accessible online via Google Maps, I am unable to retrieve the data in a traditional sense (i.e., download a CSV/JSON file). Being simply a Coffee Specialist at the Boutique, I would not have access to save this data locally on an endpoint because I am neither in a management-level position nor part of the Nespresso Canada Social Media team. Therefore, the only option was to programmatically extract the data using the Selenium package and the ChromeDriver executable file. 

In other words, I coded a scraper bot to perform the following.
1. The scraper bot is switched on and it will open the Google Chrome web browser.
2. Go to Google Maps webpage of Nespresso Metrotown branch via a pre-specified URL link.
3. Retrieve **_Overall Rating_** and **_Total Number of Reviews_** values via a pre-specified xpath, and save the values in a pandas dataframe.
4. Retrieve the count in the **_Number of Reviews_** per Star Rating via a pre-specified xpath, and save the counts in a pandas dataframe.
5. Access the full reviews list within the Google Maps webpage of Nespresso Metrotown branch via a pre-specified URL link. 
6. Sort the reviews by recency by toggling the _Sort_ button and then selecting the _Newest_ option via pre-specified xpaths.
7. As not all reviews can be seen within the browser window, the scraper bot would continuously scroll to the bottom of the page/review list to load all reviews until the very first that was received in 2019. This is performed with the pre-specified xpath of the review list scroll-bar.
8. After all reviews from oldest to most recent are loaded by the webpage, using a partial xpath refering to the reviewer name, the scraper bot will retrieve the reivewer names and save them as elements in a list to be used in the forthcoming steps.
9. As some reviews recieved for Nespresso Metrotown branch can be quite long, the full written review is not shown to conserve browser screen space. There are _More_ buttons under each review to show the full written review in case it is too long. Thus, the scraper bot must click all of the _More_ buttons in the review list with the button HTML Tag name to unveil the full written reviews for retrieval.
10. Using previously retrieved reviewer name list and a pre-specified parent xpath, the scraper bot will loop through the entire review list from most recent to oldest, and retrieve the values of **_Reviewer Name_**, **_Total Reviews Given_** by the reviewer, **_Time of Review_**, **_Review_** (i.e., the actual review received), and **_Stars Given_**. Then the aforementioned values are saved in a pandas dataframe.
11. All pandas dataframes are altogether saved in an Excel workbook, as well as individual CSV files, and then placed into a "data" folder.
12. The scraper bot is then shutdown.

## Data Cleaning

In this stage of the project, the raw Google Reviews data is imported and is cleaned for any data scraping errors & inconsistencies. There is transformation of the dataset to include new columns that are more meaningful for the later stages of the project. It is imperative to take a peek of the data by checking the first few rows and summary of column names, non-NULL count, and data types, prior to performing data cleaning & transformation. After the appropriate data cleaning & transformation steps are completed, the cleaned data is then loaded to a CSV file to be used in the next stage of the project. That being said, below is a list of the most important actions taken in this stage of the project leading up to the aforementioned data loading step.
* Retreive **_Retriever Title_** from **_Total Reviews Given_** column values, and create a new column from the retrieved values.
* Clean the the **_Total Reviews Given_** column values such that the word "reviews" is removed, and only the numeric count of reviews remains.
* Parse the column values for **_Total Reviews Given_** to integer, remove any instance of punctuation, and print rows whereby the column values could not be parsable to integer. Make manual adjustments by row when necessary.
* Clean the **_Time of Review_** column values such that there is no extra whitespace.
* Due to a web scraping error, for some rows the value for **_Total Reviews Given_** is moved to **_Time of Review_** column, and the value for **_Time of Review_** is moved to **_Stars Given_**. Find the rows where this error has occurred and perform the appropriate shuffling so that the appropriate values are in the correct columns.
* Retrieve the **_Year of Review_** from the _Webscraping Datetime_ value and **_Time of Review_** column value, and create a new column from the retrieved values.
* Clean the **_Review_** column such such that each row does not contain "response from owner" or NULL values.
* Clean the **_Stars Given_** column by removing the word "stars" and any extra whitespace. Then convert the column data type to numeric.
* Change the order of columns to be more logical, and change column names when necessary.

Below is a tabular breakdown of the cleaned Google Review data in terms of columns and data types.
|Column|Data Type|
|--|--|
|Reviewer Name|object|
|Reviewer Title|object|
|Total Reviews Given|int64|
|Review|object|
|Stars Given|int64|
|Year of Review|int64|
|Time of Review|object|
|Webscraping Datetime|object|

## Exploratory Data Analysis

This stage is somewhat self explanatory, but it is important in order to get "a feel" of the now cleaned Google Reviews dataset by performing Exploratory Data Analysis (EDA). Prior to this, it is important to import the data and filter it such that the **_Year of Review_** is between 2019 to 2022. In other words, the data should be filtered to reflect 2019 to 2022 as the current year, 2023, is still in progress.

To see a list of the top reviewers overall as well as top reviewers based on specific condition, please refer to the latter half of the [EDA](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/3_EDA.ipynb) Jupyter Notebook. This is to protect both the Google Accounts and identities of the customers that provided reviews to not be parsed by any internet search engines.

### Fig 1. Distribution by Stars
![Fig 1. Distribution by Stars Given](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/1.png?raw=true)

Overall, our service quality is good but not consistent. There is a higher distribution of 5-star reviews received throughout the years. The Boutique has also received a relatively lower proportionate amount of reviews with 1-star and 2-star rating.

### Fig 2. Reviews by Stars Given over the years
![Fig 2. ](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/2.png?raw=true)

There has been a significant dip in the number of reviews received in 2021, and then there was a sharp increase of the number of reviews in 2022. This is due to to the Covid-19 pandemic where there was limited in-person services provided at Nespresso. Therefore, customers would not be able to experience our in-person customer service to provide Google Reviews. Below are some general observations:
* Significant increase in 1-star reviews in 2022 comapared to 2021.
* Stagnant 2-star reviews throughout the years.
* Historically, there has been a decrease in 4-star reviews but there was a slight uptick in 2022.
* There has been a significant increase and then a sharp dip in 5-star reviews post-2020, and then a sharp increase post-2021.

### Fig 3. Number of Reviews over the years
![Fig 3. Number of Reviews over the years](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/3.png?raw=true)

Interestingly, the boutique received maximum reviews in 2020, yet minimal reviews in 2021 despite the Covid pandemic still at large. Post-pandemic levels of reviews are equivalent to pre-pandemic levels of reviews.

### Fig 4. Proportion of Reviewers with "Local Guide" Status
![Fig 4. Proportion of Reviewers with "Local Guide" Status](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/4.png?raw=true)

There is a higher proportion of reviews received from customers that are of "Local Guide" status, yet there are a lot of reviews received from customers that are not of "Local Guide" status that suggests a higher amount of subjective reviews.

### Fig 5. Stars Given by Reviewer Title
![Fig 5. Stars Given by Reviewer Title](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/5.png?raw=true)

The interesting observation in this chart is that almost 50% more reviewers with "Local Guide" status provided 5-star rating compared to reviewers without "Local Guide" status. Furthermore, reviewers with "Local Guide" status also provided more 4-star and 3-star rating compared to reviewers without "Local Guide" status. This infers that Nespresso Metrotown's service quality is excellent, and it is truly acknowledged as well as legitimate. A confirmation of this notion could be that there are more 2-star and 1-star rating provided by reviewers without "Local Guide" status. That being said, there is still a sizeable proportion of 1-star reviews form reviewers with "Local Guide" status. Thus, certain negative perceptions about Nespresso Metrotown's service quality may be legitimate.

### Fig 6. Percentage Proportion of Review Given vs No Review Given
![Fig 6. Percentage Proportion of Review Given vs No Review Given](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/6.png?raw=true)

The pie chart above indicates that only nearly 70% of the reviewers wrote a review in addition to providing a star rating. Thus, little more than 30% of the reviewers only provided a star rating, but did not write a review. Regardless whether the written reviews are positive, negative, or in-between, the service provided to the customers is memorable enough for them to provide a written review.

### Fig 7. Length of Reviews by Number of Words Used - Bar Chart
![Fig 7. Length of Reviews by Number of Words Used - Bar Chart](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/7.png?raw=true)

Despite nearly 70% of reviewers providing a written review, when comparing the word count of written reviews, there is a variation in terms of the number of words used per reviewer based on the bar chart. The number of reviews without a written component outweighs the number of reviews with a written component that is more than or equal to two words.

### Fig 8. Length of Reviews by Number of Words Used - Histogram
![Fig 8. Length of Reviews by Number of Words Used - Histogram](https://github.com/kjeshang/NespressoMetropolisCustomerReviewAnalysis/blob/main/Images/2_EDA/8.png?raw=true)

In the histogram, it can be seen that the majority of the number of words used for written reviews are less than 150. There is also an anomaly whereby there has been a handful of reviewers that wrote around 300 words for their review. This is indicative of an anomaly occurrence whereby the reviewer may have experienced a very positive or negative experience in terms of service quality at Nespresso Metrotown branch. Despite the anomaly, the histogram indicates a right-skewed distribution with the majority of the reviews having a  word count less than 150.

## Sentiment Analysis - Exploration

This stage of the project, after importing the necessary packages, the cleaned Google Reviews is imported and filtered to only include reviews received from 2019 to 2022. Prior to performing any natural language processing (NLP), the Google Reviews data must be pre-processed; specifically, the **_Review_** column values for each row. NLP pre-processing is necessary to perform appropriate sentiment analysis. Below are the important steps involved in the NLP pre-processing.
1. Perform word tokenization such that each word in the **_Review_** column value is separated by a comma in a list. Now, each word are referred to as a token.
2. Perform lemmatization such that each token that is a word in its extended form is reduced to its base form (i.e., Caring --> Care).
3. Remove any tokens that are punctuation or english stop words.
4. Perform Part-of-Speech tagging on each token to only include adjectives, verbs, nouns, and adverbs.
5. For each token, reduce any occurrence of additional whitespace.
6. Combine all tokens, in the comma-separated list, together into a unified sentence. 
7. Repeat steps 1 through 5 per row for each **_Review_** column value.
8. Create a new column called **_Review Cleaned_** and save the pre-processed column values to it.






