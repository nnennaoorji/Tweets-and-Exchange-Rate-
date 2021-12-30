# Can Tweets Help To Predict Exchange Rate Movements In Nigeria?


# Background

Nigeria is an import-dependent economy. Historically, upward movements in the exchange rate have been a notorious driver of inflation via it's pass-through effect to domestic prices. Inflation expectation is a fundamental factor considered in forecasting inflation. However, accurately capturing this information has been fraught with difficulty. This study takes advantage of raw sentiments about the naira derived from tweets, and daily frequency exchange rate data, in modeling sentiments as a predictor of exchange rate trends. Ultimately, the study aims to improve the reliability of essential data used to forecast inflation and exchange rate trends in Nigeria. This analysis is useful to the Central Bank, policy institutions, business entities and generally, any individual or organization of which macroeconomic data is of interest. 

# Data 

- **Tweets**: Using a scraper for social networking services (snscrape), 122,763 tweets were pulled from Twitter.
- **Twitter Sentiments**: VADER Model was used perform sentiment analysis on tweets, as well as obtain sentiment scores which were used in modeling.
- **Exchange Rate**: Daily frequency exchange rate data for naira (NGN)/USD rates at the Bureau de Change (BDC) market segment for the period 2014-2020 were sourced from the Central Bank of Nigeria.

The table below summarizes the data cleaning process:
| Data | Action |
|---|---|
| Tweets | - Dropped duplicates - Scraper did not find tweets with the relevant search terms for Feb 2016, May 28th, 2017, April 1&2, 2016 - Removed links and special characters using Regex - Post cleaning, 116,330 unique tweets from 51,226 users  |
| Twitter Sentiments | - Aggregated sentiments into daily averages - Filled in nulls with average sentiment scores - Post cleaning, 2,526 observations total |
| Exchange Rate  | - Dataset excluded weekends & holidays - Filled in nulls with last known observations ('ffill') - Filled in nulls for 1/1/2014 with next known observation ('bfill') - Post Cleaning, 2,526 observations total |

# Preprocessing 

To get the data ready for the modeling process, the following actions were taken:
- Applied the Term Frequency - Inverse Document Frequency (TF-IDF) Vectorizer to obtain words of relative importance in the corpus and in doing so, set the following as hyperparameters: ngram_range=(1,6), max_df=0.8, min_df=0.02; 
- Identified and removed stop words;
- Conducted Sentiment Analysis using VADER Model to obtain positive, negative, neutral and compound sentiment scores.
- Computed the percentage change in exchange rates;
- Computed lags of postive, negative, and compound sentiments scores for up to 3 days
- Checked for stationarity of each time series using visual plots and the Augmented Dickey Fuller (ADF) test.
- Split the dataframe into training and test sets to faciliate forecasting using SARIMAX model.

# Modeling
  
The following models were specified to address the problem statement:
 
>1. **Granger Causality Test**: This was to test for causality by determining if sentiment scores offered useful information in forecasting the rate of change in exchange rate. With the metric for success set at p value < 0.05, the analysis found signifcant bi-directional causality between rate of change in exchange rate and sentiment – negative & compound sentiment scores. No significant causal relationship was found to exist between rate of change in exchange rate & positive sentiment scores.

>2. **Autoregressive Distributed Lag Model (ARDL)** The model included lagged sentiment scores of up to 3 days as predictors of rate of change in exchange rate. With the metric for success set at p value < 0.05, the analysis found a significant relationship between negative sentiment score lagged by one day, and rate of change in exchange rate. The relationship found was such that for every 1 unit change in negative sentiment in a previous day, NGN/USD exchange rate was expected to increase by 1.14 percentage point the next day. Results from an Unconstrained Error Correction Model (UECM) and cointegration test specified subsequently showed that this relationship persists in the long run.

> 3. **Seasonal Auto-Regressive Integrated Moving Average with eXogenous factors (SARIMAX) model** Attempted to forecast exchange rate movements using lagged exchange rate change and sentiment scores as explanatory variables. However, the model did not fare well, as accuracy and MSE scores were negligible, hinting at a very high bias model. This can, however, be explained by the fact the model was too simplistic is assuming one variable as sole predictor of a complex variable such as exchange rate. 
  
# Conclusion

- **Yes**, twitter sentiments can help to predict the trend of exchange rate in Nigeria. It appears negative sentiments matter more.
- Study specified 3 models – Granger Causality,  ARDL & SARIMAX in analyzing the relationship between NGN/USD exchange rate and sentiments from 116,330 tweets by 51,226 users between 2014 and 2020.
- Granger Causality Test showed bi-directional causality running from sentiments (negative & compound scores) to exchange rate & vice versa.  
- ARDL model corroborated this finding as significant long run relationship was reported for (negative) sentiments and exchange rate movements.


# Recommendations / Next Steps

- For future analysis, this study recommends scaling up the number of tweets analyzed to improve reliability of results.

- Future analysis could decompose sentiments into specific moods to better understand the nature of the relationship.

- Approach could be applied and/or utilized along with other tools to forecast other macroeconomic variables.








