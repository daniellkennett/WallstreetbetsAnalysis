# WallstreetbetsAnalysis


In the last year, investors have coined a new term: the 'meme stock'. These meme stocks are characterized by large amounts of uncertainty in short timespans and hype in small online communities. Reddit, more specifically it's subreddit Wallstreetbets, is one of these "small communities currently having close to 10 million subscribers. This thread is accustomed to users making bold claims about security movements. This culminated to January 27th where Gamestop Corporations stock grew approximately 2600% to $483 in about a month. This significant event proves that individual investors hold some influence over the market. The introduction of free and easy investing/trading has brought forth an age where individuals can sway the markets. 

Reading material for history of GME or context:https://abcnews.go.com/Business/gamestop-timeline-closer-saga-upended-wall-street/story?id=75617315

# Data set


The data set I worked is a combination of Reddit's API (PRAW) and data by Raphael Fontes. PRAW is limited to the last 1000 posts, so I had to rely on individuals who collect the data. ** WARNING ** The dataset is very messy and requires a lot of cleanup before it is usable. For the purpose of this analysis, the data set specifically focuses between December 31st, 2020 to February 16th-- to observe the events of meme stocks in 2021.
https://www.kaggle.com/unanimad/reddit-rwallstreetbets

<details>
         <summary><b>PRAW Code</b></summary>
<br>


#### Reddit API(PRAW) Code 
```python
import praw
reddit = praw.Reddit(client_id = <ID HERE,
client_secret = <SPECIAL API KEY HERE>,
user_agent = <NAME OF PROJECT HERE>,
username =<USERNAME HERE>,
password = <PASSWORD HERE>)



subreddit = reddit.subreddit('wallstreetbets')
hot_subreddit = subreddit.new(limit = None)

topics_dict = { "title":[], 
                "score":[], 
                "id":[], "url":[],  
                "comms_num": [], 
                "created": []}

for submission in hot_subreddit:
    if submission.score > score and submission.upvote_ratio > upvote_ratio: 
        topics_dict["title"].append(submission.title)
        topics_dict["score"].append(submission.score)
        topics_dict["id"].append(submission.id)
        topics_dict["url"].append(submission.url)
        topics_dict["comms_num"].append(submission.num_comments)
        topics_dict["created"].append(submission.created)

        
wsb = pd.DataFrame(topics_dict)
```

</details>
<br />


<details>
         <summary><b>Columns:</b></summary>
<br>
          <b>id:</b> unique identifier <br />
          <b>title:</b> string of characters used for the post<br />
          <b>score:</b> number of upvotes<br />
          <b>author:</b> original poster<br />
          <b>comments:</b> number of comments<br />
          <b>timestamp:</b> unix time when posted<br />
</details>
 <br />

      
#### Alpha Vantage API for stock prices


<details>
         <summary><b>Alpha Vantage Code</b></summary>
<br>

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

</details>
<br />




<details>
         <summary><b>Columns:</b></summary>
<br>
          <b>date:</b> unique identifier <br />
          <b>open:</b> opening price<br />
          <b>high:</b> highest price in time period<br />
          <b>low:</b> lowest price in time period<br />
          <b>close:</b> nclosing price<br />
          <b>volume:</b> number of buys and sells of security<br />
</details>
 <br />



# Goal


1. How much was Wallstreetbets posting about meme stocks?
2. Which meme stock was mentioned most?
3. GME stock chart? Growth?
4. Are WSB mentions of GME related to the price increase?

### Hypothesis Tests
* **H0** : The number of mentions in a hour of trading HAS NO influence of price change. <br />
  **H1** : The number of mentions in a hour of trading HAS  influence of price change 
  
* **H0** : The number of mentions in the last two hours of trading DOES NOT influence of opening prices. <br />
  **H1** : The number of mentions in the last two hours of trading DOES influences the opening price next day 
  
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



![Hourly Candlestick](Images/Hourly%20Candlestick.png)





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
To prove this, a spearman's rank correlation is preferred due to the monotonic relationship. 

# Hypothesis Tests

### 1. The number of mentions in a hour of trading HAS  influence of price change 

H0: <img src="https://render.githubusercontent.com/render/math?math=$\rho=0$">

H1: <img src="https://render.githubusercontent.com/render/math?math=$\rho \neq 0$">

```python
stats.spearmanr(gmefull['count'],gmefull['percent change'])

SpearmanrResult(correlation=-0.10321609558737457, pvalue=0.02493075330552498)
```

### 2. The number of mentions in the last two hours of trading DOES influences the opening price next day

H0: <img src="https://render.githubusercontent.com/render/math?math=$\rho=0$">

H1: <img src="https://render.githubusercontent.com/render/math?math=$\rho \neq 0$">

```python
stats.spearmanr(growth_vs_hr['count'], growth_vs_hr['percent change'])
SpearmanrResult(correlation=-0.0877017386768316, pvalue=0.6449129625582433)
```

![Early Mention vs Daily Stock](Images/Early%20Mentions%20vs%20Daily%20Stock%20Change.png)


This image visually represents the LACK of correlation between number of mentions in the first two hours of trading and the growth through the trading day. 


### 3. The number of mentions of a stock on WSB in the first 1-1/2 hours of trading DOES influence the stock growth 

H0: <img src="https://render.githubusercontent.com/render/math?math=$\rho=0$">

H1: <img src="https://render.githubusercontent.com/render/math?math=$\rho \neq 0$">

```python
stats.spearmanr(gme_daily_joined['count'], gme_daily_joined['percent change'])
SpearmanrResult(correlation=-0.0035603027138667903, pvalue=0.9851027762191317)
```




















