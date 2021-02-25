# WallstreetbetsAnalysis


In the last year, investors have coined a new term: the 'meme stock'. These meme stocks are characterized by large amounts of uncertainty in short timespans and hype in small online communities. Reddit, more specifically it's subreddit Wallstreetbets, is one of these "small communities currently having close to 10 million subscribers. This thread is accustomed to users making bold claims about security movements. This culminated to January 27th where Gamestop Corporations stock grew approximately 2600% to $483 in about a month. This significant event proves that individual investors hold some influence over the market. The introduction of free and easy investing/trading has brought forth an age where individuals can sway the markets. 

# Data set


The data set I worked is a combination of Reddit's API (PRAW) and data by Raphael Fontes. PRAW is limited to the last 1000 posts, so I had to rely on individuals who collect the data. ** WARNING ** The dataset was very messy and requirs a lot of cleanup before it was usable.
https://www.kaggle.com/unanimad/reddit-rwallstreetbets


**Columns:** <br />
          **id:** unique identifier <br />
          **title:** string of characters used for the post<br />
          **score:** number of upvotes<br />
          **author:** original poster<br />
          **comments:** number of comments<br />
          **timestamp:** unix time when posted<br />
          
          
Additionally I used Alpha Vantage's API to gather stock data to compare.


**Columns:** <br />
          **date:** unique identifier <br />
          **open:** string of characters used for the post<br />
          **high:** number of upvotes<br />
          **low:** original poster<br />
          **close:** number of comments<br />
          **volume:** unix time when posted<br />

# Goal


1. How much was Wallstreetbets posting about meme stocks?
2. Which meme stock was mention most?
3. GME stock chart? Growth?
4. Are WSB mentions of GME related to the price increase?

### Hypothesis Tests

* **H0** : The number of mentions in the last two hours of trading has no influence of opening prices. <br />
  **H1** : The number of mentions in the last two hours of trading influences the opening price next day 
  
* **H0** : The number of mentions of a stock on WSB in the first 1-1/2 hours of trading DOES NOT influence the stock growth <br />
  **H1** : The number of mentions of a stock on WSB in the first 1-1/2 hours of trading DOES influence the stock growth 

![Top 30 Words](Images/download%20(1).png/)


![Daily Candlestick](Images/Daily%20Candlestick.png)


![Early Mention vs Daily Stock](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/Early%20Mentions%20vs%20Daily%20Stock%20Change.png)


![Daily GME Mentions](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/Daily%20GME%20Mentions.png)


![GME Hourly Price vs Mentions](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/GME%20Hourly%20Prive%20vs%20Mentions.png)



![GME Daily Price vs Mentions](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/GME%20Price%20vs%20Mentions.png)


![Hourly Candlestick](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/Hourly%20Candlestick.png)



![Mentions in WSB](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/Mentions%20in%20WSB.png)


![Percent Change](https://github.com/daniellkennett/WallstreetbetsAnalysis/blob/main/Images/Percentage%20GME%20Change.png)
