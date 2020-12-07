# Stock Forecasting Using Social Media Sentiment: Midterm Report

## Dataset Description:
### Features and Examples:
Our data consists of three main parts. We have data from twitter, data from reddit, and stock return data from Finnhub. All the data spans from the beginning of July 2020 through the end of September 2020. We also chose to concentrate on five large tech companies. We chose Facebook (FB), Apple (AAPL), Amazon (AMZN), Netflix (NFLX), and Google (GOOG). These four companies are commonly referred to by the acronym “FAANG”. 

For the twitter data we looked at how many times each company was mentioned every day from July through September. Each row of data corresponds to a specific date and company and includes the number of posts on that day referring to one of the five companies. 

The reddit data was mainly scraped from two relevant “subreddits”, which are communities dedicated to a specific topic. We took data from the “stocks” subreddit and the “investing” subreddit. Our original dataset includes every post from each subreddit from the beginning of July to the end of September. This ended up being over 25,000 reddit posts. In addition to the title of each post it also includes how many “upvotes” or likes that post received as well as a timestamp. We then transformed this data by counting the number of posts and upvotes each company received per day. Every row of the transformed reddit dataset corresponds to a date and company and has the total number of posts about the company and the total number of upvotes from those posts. 

Lastly, we downloaded historical stock data for each of these companies and calculated daily returns. The historical stock data includes date, opening price, closing price, and volume. We then merged this data with our twitter and reddit dataset. 

The features in our dataset we are currently considering are for each day and company: number of tweets about a company, number of reddit posts about a company, and number of upvotes those reddit posts received. The examples in our dataset are the daily returns for each company. 

### Data Scraping:
**Reddit:** In order to scrape the reddit data we used a combination of the Reddit API and a custom function that is part of the PushShift API in order to download posts from any time period. Since these APIs only allow downloading 100 posts at a time we ran a loop that downloaded posts in one hour time periods. We also added some error handling to the function so deleted posts would not cause the download to break. Overall, we scraped over 25,000 posts and it took about 10 hours to download all the posts.

**Twitter:** The Twitter data was scraped through the Twitter Search API that allows you to search past tweets on Twitter through a query. Initially, the API only allows the user to search tweets going back to seven days. You can only query the whole archive of tweets through the premium package, which is what we did. Next, while dealing with a limited number of API requests for Twitter, we decided to query the number of posts that contains each of the five companies’ ticker symbols for each day from July to October.

**Stock Quotes:** In order to get stock quotes of the 5 companies from July to October, we used the Finnhub API, which is a verified financial data API source offering 25 years of historical data of the US market. From the API, we fetched each day’s open and closing price and the volume traded.  

### Data Cleaning:
After scraping the reddit and twitter data we needed to clean it and then merge it to the historical stock data. To clean the reddit data we first had to categorize each post by company. We did this with many hot encoding. We added a column to the reddit posts dataframe for each company and set it to 1 if that post was relevant to that company and 0 otherwise. Notably there were many posts that were not relevant to any of the five companies we are focusing on. After categorizing the posts we aggregated them by date and company to get the final data frame containing the number of posts and upvotes per company per day. We also removed weekends and days with no trading. We then merged both the reddit and twitter data with the historical stock data. 

---

## Preliminary Analysis:
After constructing our dataset we looked at a few basic descriptive statistics. We found that on average from the subreddits we scraped there are only about 2 posts per day on each company. The most talked about company by far was Apple with over 7 posts per day. Netflix, Facebook, and Google were posted about significantly less each averaging around 1 post per day. Since those three companies have so few posts it is unlikely we will be able to build a reliable predictor just from reddit data. However, the data on Apple is more encouraging. Not only does Apple have over 7 posts per day from the investing related subreddits, there is also the possibility of scraping data from the Apple specific subreddit in the future. Another interesting aspect of our data that we found was that each company has a similar number of upvotes per post. Posts on Google received around 36 upvotes on average which was the smallest of the five companies. Posts about Apple and Amazon received on average around 41 upvotes while posts on Facebook and Netflix received on average 52 and 55 upvotes per post. Looking at the twitter data we found that people were far more likely to tweet about Facebook and Amazon stock than Apple, Netflix, or Google stock. We also found that our twitter data for each company has a fairly wide range. Tweets about Facebook ranged from under 300,000 to over 750,000. 

### Data Visualization
We initially plotted scatter plots below for each of the companies in FAANG and showed the relationship between the number of Twitter posts containing the company’s ticker symbol and the daily returns of each company’s stock. Plotting these points, we calculated that the average correlation between the stock’s daily return and the number of posts is approximately 0.0527 with Netflix containing the strongest relationship at a correlation of 0.1407. So far, there seems to be a very weak linear relationship between twitter posts and stock daily returns, which would most likely lead us to search for more of a sophisticated model for our data.

<p align = "center">
  <img src = "/Plots/plots1.png">
  <img src = "/Plots/plots2.png">
</p>

In addition, we wanted to examine the relationship between daily returns and the daily counts of each social media posts. First, as shown in the hisogram below, the daily return of a stock price mostly ranges from -5% to 5%. Next, We hypothesized that if there are more posts on social media, the absolute value of the daily returns would increase under the assumption that when a stock price changes greatly, people would talk about it more. However, as shown in the plots below, the red and ble point are disperesed in a disordered fashion, indicating that classifying a positive return and a negative return only using the post counts seems to be nearly impossible at this stage; neither for classifying absolute value of daily returns.

<p align = "center">
  <img src = "/Plots/download-6.png">
</p>

Thus, we believe we will have to incorporate more features and develop our model, which we will explain more in detail in the next part.

## Next Steps:

### Model Construction:
Our ultimate goal of this project is to create a stock forecasting model based on the social media data we have. Initially, we will attempt to create a boolean classification model, which simply classifies whether a stock price will rise or not the next day depending on the social media sentiment of the past. This will give us a rough guidance on how to deal with our  features. 

After doing so, we plan to create time series models using lags ranging from 1 week to 1 month. Our training set will be the data of August, which the features would include the data of July, and our test set will be the data of September, which includes the features of August. While incorporating various regression models for our time series models, we will do cross-validation for each type in order to determine which model fits the best.

### Model Evaluation:
One key concern about our model is that we are excluding the data of the weekends and holidays, when the stock market does not open. Thus, if our data appears to be too missing, we would have to come up with a new strategy to fill in the missing data. (i.e, averaging the closing price of the end of the week and opening stock price of the start of the week.) A stock forecasting model is highly vulnerable to overfitting. There are many noises in the market, and predicting future stock prices perfectly is nearly impossible. Thus, we do not expect our model to have an exceptionally low testing error, and avoiding overfitting would be the key to the success of our model. Thus, we would also incorporate regularization to our model to avoid it. 

