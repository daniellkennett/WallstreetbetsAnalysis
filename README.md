# WallstreetbetsAnalysis    ![GME](Images/gamestop-original.original.jpg)

# Table of contents
1. [Introduction](#introduction)
    * [Data set](#introsub1)
3. [Goal](#par1)
    * [Hypothesis Tests](#subpar1)
    * [How much was Wallstreetbets posting about meme stocks?](#par2)
    * [GME stock charts](#par3)
    * [Are WSB mentions of GME related to the price increase?](#par4)
4. [Running Hypothesis Tests](#par5)
5. [Conclusion](#con)

# Introduction <a name="introduction"></a>



In the last year, investors have coined a new term: the 'meme stock'. These meme stocks are characterized by large amounts of uncertainty in short timespans and hype in small online communities. Reddit, more specifically it's subreddit Wallstreetbets, is one of these "small communities currently having close to 10 million subscribers. This thread is accustomed to users making bold claims about security movements. This culminated to January 27th where Gamestop Corporations stock grew approximately 2600% to $483 in about a month. This significant event proves that individual investors hold some influence over the market. The introduction of free and easy investing/trading has brought forth an age where individuals can sway the markets. 

Reading material for history of GME or context:https://abcnews.go.com/Business/gamestop-timeline-closer-saga-upended-wall-street/story?id=75617315

## Data sets <a name="introsub1"></a>


The data set I worked is a combination of Reddit's API (PRAW) and data by Raphael Fontes. PRAW is limited to the last 1000 posts, so I had to rely on individuals who collect the data. ** WARNING ** The dataset is very messy and requires a lot of cleanup before it is usable. For the purpose of this analysis, the data set specifically focuses between December 31st, 2020 to February 16th-- to observe the events of meme stocks in 2021. In addition to the PRAW Dataset, I used Alpha Vantage to gather stock data. 
https://www.kaggle.com/unanimad/reddit-rwallstreetbets


#### Reddit API(PRAW) Code 


<details>
         <summary><b>PRAW Code</b></summary>
<br>



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

      
#### Alpha Vantage API Code


<details>
         <summary><b>Alpha Vantage Code</b></summary>
<br>

```python
from alpha_vantage.timeseries import TimeSeries

API_key = 'OZHBQ2Q48QC0NFRZ'
ts = TimeSeries(key = API_key,output_format='pandas')
### daily chart, include percent change ###

def stock_pull(stock, startdate, enddate, interval=None):
    if interval==None:
        data = ts.get_daily_adjusted(stock)
        data = data[0].reset_index()
        data = data[(data['date'] >= startdate) & (data['date'] <= enddate)]
        data = data.sort_values('date', ascending=True)
        data['percent change'] = data['4. close'].pct_change()
    else:
        data = ts.get_intraday(stock, interval = interval, outputsize='full')
        data = data[0].reset_index()
        data = data[(data['date'] >= startdate) & (data['date'] <= enddate)]
        data=data.sort_values('date', ascending=True)
        data['percent change'] = data['4. close'].pct_change()
    return data

gme_hourly = stock_pull('GME', '2020-12-31', '2021-02-16', '60min')
gme_daily = stock_pull('GME', '2020-12-31', '2021-02-16')
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
          <b>close:</b> closing price<br />
          <b>volume:</b> number of buys and sells of security<br />
</details>
 <br />



# Goals <a name="par1"></a>


1. How much was Wallstreetbets posting about meme stocks? Which meme stock was mentioned most?
2. GME stock charts
3. Are WSB mentions of GME related to the price increase?

### Hypothesis Tests <a name="subpar1"></a>
* **H0** : The number of mentions in a hour of trading HAS NO influence of price change. <br />
  **H1** : The number of mentions in a hour of trading HAS  influence of price change 
  
* **H0** : The number of mentions of a stock on WSB in the first 2 hours of trading DOES NOT influence the stock growth <br />
  **H1** : The number of mentions of a stock on WSB in the first 2 hours of trading DOES influence the stock growth 
  
* **H0** : The number of mentions in the last two hours of trading DOES NOT influence of opening prices. <br />
  **H1** : The number of mentions in the last two hours of trading DOES influences the opening price next day 
  

  
# 1. How much was Wallstreetbets posting about meme stocks? Which meme stock was mentioned most? <a name="par2"></a>
Firstly, I think it is worth diving into the number of mentions of each meme stock in the subreddit, Wallstreetbets. In this instance, I created a word counter and found the most mentioned words:


![Top 30 Words](Images/download%20(1).png)


Mentioned in the word cloud are: GME, AMC, NOK, and DOGE. These securities are Gamestop, AMC Theaters, Nokia, and DogeCoin respectivley. The behavior of these stocks classifies them as memestocks, or in the case of Dogecoin a meme cryptocurrency. 


Compared to other popular securities such as Tesla, Bitcoin, Amazon, Microsoft and the S&P500 index fund; The four mentioned memestocks have more mentions. 
| Stock   | Count    |    
| :---    | :----:   |  
| gme     | 102785   | 
| amc     | 45965    | 
| nok     | 20994    |
| dog     | 15703 |
| tsla    | 2717 |
| btc     | 1578 |
|amzn     | 619|
| msft    | 44|
| sp500   | 30       |



![Mentions in WSB](Images/Daily%20Mentions%20for%20GME,%20NOK,%20AMC.png?raw=true)



### GME specifically

![Daily GME Mentions](Images/Daily%20GME%20Mentions.png)


# 2. GME stock chart? Growth? <a name="par3"></a>



![Hourly Candlestick](Images/Hourly%20Candlestick.png)





![Daily Candlestick](Images/Daily%20Candlestick.png)


Observed in Gamestops stock prices is the steady price of around $18 until mid January. Media and hype carried the stock up to record heights. As more investors experienced FOMO and gave into emotional investing, the stock reached ~$500 on January 28th.

**Hourly Change Data**

| Stock   | Count    |    
| :---    | :----:   |  
| count     | 472   | 
| mean    | 0.004323965    | 
| std    | 0.086254    |
| max     | 0.535226 |
| min    | -0.437637 |
| 25%     | -0.018945 |
|50%     | 0.000000|
| 75%    | 0.015459|


![Percent Change](Images/Percentage%20GME%20Change.png)


Gamestop's stock price fluctuated immensely. At times the stock gained or lost 40% in a single hour. Another statistic to notice is the 50 percentile of both. Since the stock rose and fell to similar levels, this appears reasonable. 

# 3. Are WSB mentions of GME related to the price increase? <a name="par4"></a>


![GME Daily Price vs Mentions](Images/GME%20Price%20vs%20Mentions.png)


Yes, they are related. The nature of meme stock imply that they inspire hype amongst individual investors. Additionally, large events like these are covered by news outlets and spread to many social media outlets. The manner in which media portrays stocks has an impact on the buyers and sellers of the securities. 
To prove this, a spearman's rank correlation is preferred due to the monotonic relationship. 

# Running Hypothesis Tests <a name="par5"></a>

### 1. The number of mentions in a hour of trading HAS  influence of price change 

H0: <img src="https://render.githubusercontent.com/render/math?math=$\rho=0$">

H1: <img src="https://render.githubusercontent.com/render/math?math=$\rho \neq 0$">

```python
stats.spearmanr(gmefull['count'],gmefull['percent change'])

SpearmanrResult(correlation=-0.10321609558737457, pvalue=0.02493075330552498)
```

### 2. The number of mentions of a stock on WSB in the first 2 hours of trading DOES influence the stock growth 

H0: <img src="https://render.githubusercontent.com/render/math?math=$\rho=0$">

H1: <img src="https://render.githubusercontent.com/render/math?math=$\rho \neq 0$">

```python
stats.spearmanr(growth_vs_hr['count'], growth_vs_hr['percent change'])
SpearmanrResult(correlation=-0.0877017386768316, pvalue=0.6449129625582433)
```

![Early Mention vs Daily Stock](Images/Early%20Mentions%20vs%20Daily%20Stock%20Change.png)


This image visually represents the LACK of correlation between number of mentions in the first two hours of trading and the growth through the trading day. 


### 3. The number of mentions in the last two hours of trading DOES influences the opening price next day

H0: <img src="https://render.githubusercontent.com/render/math?math=$\rho=0$">

H1: <img src="https://render.githubusercontent.com/render/math?math=$\rho \neq 0$">

```python
stats.spearmanr(gme_daily_joined['count'], gme_daily_joined['percent change'])
SpearmanrResult(correlation=-0.0035603027138667903, pvalue=0.9851027762191317)
```

I cannot object the null hypothesis. Any correlation between mentions in the last two hours of trading and the price change is due by chance.

# Conclusion <a name="con"></a>

This analysis on Wallstreetbets and GME's price makes a lot of assumptions. 
1. Assumes that GME is the only stock. A better measure would to include all meme stocks. 
2. Assumed that movements happened on an hourly and daily perspective. A more indepth study can explore smaller time periods. 
3. Did not consider government or firm intervention. On January 27th, many trading platforms halted the trade of meme stocks. If allowed to continue growing, the two factors MAY have been more correlated. 
4. All publicity is good publicity. Not true. A Sentimental analysis may be a better fit for this analysis. 

As it stands, the first hypothesis states that there is a small negative correlation of mentions to stock growth. Therefore if you invest based off on the number of mentions in an hour, you are more likely to loose money. 
