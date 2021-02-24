# WallstreetbetsAnalysis


In the last year, investors have coined a new term: the 'meme stock'. These meme stocks are characterized by large amounts of uncertainty in short timespans and hype in small online communities. Reddit, more specifically it's subreddit Wallstreetbets, is one of these "small communities currently having close to 10 million subscribers. This thread is accustomed to users making bold claims about security movements. This culminated to January 27th where Gamestop Corporations stock grew approximately 2600% to $483 in about a month. This significant event proves that individual investors hold some influence over the market. The introduction of free and easy investing/trading has brought forth an age where individuals can sway the markets. 

# Data set


The data set I worked is a combination of Reddit's API (PRAW) and data by Raphael Fontes. PRAW is limited to the last 1000 posts, so I had to rely on individuals who collect the data. 
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
2. 




## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/daniellkennett/gitpages-practice/edit/main/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/daniellkennett/gitpages-practice/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
