# WallstreetbetsAnalysis


In the last year, investors have coined a new term: the 'meme stock'. These meme stocks are characterized by large amounts of uncertainty in short timespans and hype in small online communities. Reddit, more specifically it's subreddit Wallstreetbets, is one of these "small communities currently having close to 10 million subscribers. This thread is accustomed to users making bold claims about security movements. This culminated to January 27th where Gamestop Corporations stock grew approximately 2600% to $483 in about a month. This significant event proves that individual investors hold some influence over the market. The introduction of free and easy investing/trading has brought forth an age where individuals can sway the markets. 

# Data set


The data set I worked is a combination of Reddit's API (PRAW) and data by Raphael Fontes. PRAW is limited to the last 1000 posts, so I had to rely on individuals who collect the data. ** WARNING ** The dataset is very messy and requires a lot of cleanup before it is usable. For the purpose of this analysis, the data set specifically focuses between December 31st, 2020 to February 16th-- to observe the events of meme stocks in 2021.
https://www.kaggle.com/unanimad/reddit-rwallstreetbets


**Columns:** <br />
          **id:** unique identifier <br />
          **title:** string of characters used for the post<br />
          **score:** number of upvotes<br />
          **author:** original poster<br />
          **comments:** number of comments<br />
          **timestamp:** unix time when posted<br />
          
          
Additionally I used Alpha Vantage's API to gather stock data to compare.


### Use alpha vantage to gather daily and hourly stock data ###


```python
from alpha_vantage.timeseries import TimeSeries

API_key = 'OZHBQ2Q48QC0NFRZ'


### daily chart, include percent change ###
ts = TimeSeries(key = API_key,output_format='pandas')
data = ts.get_daily_adjusted('GME')
gme = data[0].reset_index()
gme_daily = gme[(gme['date'] >= '2020-12-31') & (gme['date'] <= '2021-02-16')]
gme_daily.to_csv('gme_daily_prices')
gme_daily=gme_daily.sort_values('date', ascending=True)
gme_daily['percent change'] = gme_daily['4. close'].pct_change()
### hourly chart, include percent change ###
data = ts.get_intraday('GME', interval = '60min', outputsize='full')
gme = data[0].reset_index()
gme_hourly_full = gme[(gme['date'] >= '2020-12-31') & (gme['date'] <= '2021-02-16')]
gme_hourly_full = gme_hourly_full.sort_values('date', ascending=True)
gme_hourly = gme[(gme['date'] >= '2021-01-25') & (gme['date'] <= '2021-01-29')]
gme_hourly = gme_hourly.sort_values('date', ascending=True)
gme_hourly['percent change'] = gme_hourly['4. close'].pct_change() 
```




**Columns:** <br />
          **date:** unique identifier <br />
          **open:** string of characters used for the post<br />
          **high:** number of upvotes<br />
          **low:** original poster<br />
          **close:** number of comments<br />
          **volume:** unix time when posted<br />

# Goal


1. How much was Wallstreetbets posting about meme stocks?
2. Which meme stock was mentioned most?
3. GME stock chart? Growth?
4. Are WSB mentions of GME related to the price increase?

### Hypothesis Tests

* **H0** : The number of mentions in the last two hours of trading has no influence of opening prices. <br />
  **H1** : The number of mentions in the last two hours of trading influences the opening price next day 
  
* **H0** : The number of mentions of a stock on WSB in the first 1-1/2 hours of trading DOES NOT influence the stock growth <br />
  **H1** : The number of mentions of a stock on WSB in the first 1-1/2 hours of trading DOES influence the stock growth 
  
# 1. How much was Wallstreetbets posting about meme stocks?
Firstly, I think it is worth diving into the number of mentions of each meme stock in the subreddit, Wallstreetbets. In this instance, I created a word counter and found the most mentioned words:


![Top 30 Words](Images/download%20(1).png)


Mentioned in the word cloud are: GME, AMC, NOK, and DOGE. These securities are Gamestop, AMC Theaters, Nokia, and DogeCoin respectivley. The behavior of these stocks clasiffies them as memestocks, or in the case of Dogecoin a meme cryptocurrency. 


Compared to other popular securities such as Tesla, Bitcoin, Amazon, Microsoft and the S&P500 index fund; The four mentioned memestocks have more mentions. 


# 2. Which meme stock was mentioned most?
```python
Stock	Count	
sp500	30
msft	44
amzn	619
btc	1578
tsla	2717
doge	15703
nok	20994
amc	45965
gme	102785
```

![Mentions in WSB](Images/Mentions%20in%20WSB.png)


![Daily GME Mentions](Images/Daily%20GME%20Mentions.png)


# 3. GME stock chart? Growth?

GME Hourly price
![Hourly Candlestick](Images/Hourly%20Candlestick.png)


GME Daily Price
Daily Candlestick
![Daily Candlestick](Images/Daily%20Candlestick.png)


Observed in Gamestops stock prices is the steady price of around $18 until mid January. Media and hype carried the stock up to record hieghts. As more investored experienced FOMO and gave into emotional investing, the stock reached ~$500 on January 28th.

**Hourly Change Data**
```python
count    472.000000
mean       0.004323
std        0.086254
min       -0.437637
25%       -0.018945
50%        0.000000
75%        0.015459
max        0.535226
Name: percent change, dtype: float64
```

**Daily Change Data**
```python
count    30.000000
mean      0.068010
std       0.334279
min      -0.615414
25%      -0.051929
50%       0.032281
75%       0.232600
max       0.996509
Name: percent change, dtype: float64
```


![Percent Change](Images/Percentage%20GME%20Change.png)


Gamestop's stock price fluctuated immensely. At times the stock gained or lost 40% in a single hour. Another statistic to notice is the 50 percentile of both. Since the stock rose and fell to similar levels, this appears reasonable. 

# 4. Are WSB mentions of GME related to the price increase?


![GME Daily Price vs Mentions](Images/GME%20Price%20vs%20Mentions.png)


Yes, they are related. The nature of meme stock imply that they inspire hype amongst individual investors. Additionally, large events like these are covered by news outlets and spread to many social media outlets. The manner in which media portrays stocks has an impact on the buyers and sellers of the securities. 

On closer inspection, there are mini-spikes in mentions before major increases in prices. 


![GME Hourly Price vs Mentions](Images/GME%20Hourly%20Prive%20vs%20Mentions.png)



![Early Mention vs Daily Stock](Images/Early%20Mentions%20vs%20Daily%20Stock%20Change.png)
















