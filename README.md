## A journalist's basic guide to scrape and analyze a Twitter account with R

Before we start, if it makes you feel better, I’m a low-tech millennial. 
As journalists, we seek to publish stories that are newsworthy and have potential. With high demand in daily news output, we turn to social media for breaking news, original stories and trends. And there’s no doubt that social media plays a prominent role in our society today. A survey conducted in 2019 by Ofcom on news consumption among adults and older children in the UK found that 49 percent of over 4,000 participants use social media to consume news. Meanwhile, across the pond, 55 percent of American adults often get their news from social media, according to the 2019 report by Pew Research Center.

Unlike other social media, Twitter allows real-time engagement without exclusivity. You can reply, like or retweet anybody’s tweet at any given time. Twitter is a free platform for advertising and its data can prompt big stories. You can guess a user’s characteristics or personalities by reading their tweets. Many politicians also use Twitter to engage with their constituents. [The New York Times](https://www.nytimes.com/interactive/2019/11/02/us/politics/trump-twitter-presidency.html) did a long read analyzing President Donald Trump’s tweets to find the way he was using Twitter since the 2017 inauguration with interactive graphs. One of the many interest part is that the data shows President Trump tweeted attacks in the early morning or later in the evening, when he is unlikely accompanied by his advisers.

The flood of information on Twitter or other social media could drown or sweep journalists away from original, relevant contents. So it’s necessary to find a tool to filter and analyze the information. This is where R enters the conversation. RStudio is free for use, easy to learn and good for creating data visualization.

This step-by-step guide covers the basics of Twitter scraping and data plotting with RStudio Cloud. Let’s get started:

### Step 1: Getting Twitter API access
Before scraping data from Twitter, we will need access codes. You have to create a new Twitter application which will allow you to connect to the API. You need to have a Twitter account to do this. First, apply for a developer account [here](https://developer.twitter.com/en/apply-for-access.html). Once you’re approved by Twitter you can then create a new application. You will then receive the credentials which are only for you to use.
Here’s a 2-minute video showing [how to create a Twitter application](https://www.youtube.com/watch?v=LpLYQz_3hA0&t=2s)

<img src="photo/Access-key.png" alt="hi" class="inline"/>

### Step 2: Set up your RStudio Cloud account 
Sign up for [RStudio Cloud](https://rstudio.cloud) account, create a new Project and then create a new R script.
You can also follow this 2-minute video showing [how to quickly setup your account](https://www.youtube.com/watch?v=U-pLWJO6-P4)

<img src="photo/new-R-script.png" alt="hi" class="inline"/>

### Step 3: Scraping Tweets
R Script is your playing field. We will use the package ‘twitteR’ to extract the tweets. Note that if you are not working on RStudio Cloud, you will have to ‘install.packages()’ before running ‘library()’.

```markdown
library (twitteR)
# Then create Twitter connection
# fill in the info from the app
api_key <- "6uHzxAc8qoTe6AwVomHLTc1uN"
api_secret <- "OkxnR7hqiUqkGU5M3qZEogVEfmXzwUfeWdClUyDLwbsdTbaqo2"
access_token <- "704811365962289152-rV1qGq3wxStFCTrzFChnw996w2ZjLmz"
access_secret <- "0FZiiB4ZiUQCyknWYQBI6ab5HzXzsnMSaq3T4tCe1Jbm2"
# create twitter connection
setup_twitter_oauth(api_key, api_secret, access_token, access_secret)
```
Now time to choose a Twitter account to analyze. In this example, we will look at [@NYGovCuomo](https://twitter.com/NYGovCuomo), the account of Andrew M. Cuomo, governor of New York.

<img src="photo/cuomo.png" alt="hi" class="inline"/>

```markdown
# pick a user to analyze
username='NYGovCuomo'
# we will be using the maximum number of 3200 tweets that Twitter allows viewing
cuomo=userTimeline(username,n=3200)
# convert tweets into dataframe
ac_df=twListToDF(cuomo)
# we can save the dataframe for later use 
write.csv(dataframe,"tweets.csv", row.names = FALSE)
```

### Step 4: Plotting
For the first plot, we will look at a timeline with the name of each account Cuomo replies to:

```markdown
# Import the ‘stringr’ package to remove names with old-school retweet text or ‘RT’
library(stringr)
trim <- function (x) sub('@','',x)
# The rt column shows who was retweeted & the rtt shows whether or not it was a retweet
ac_df$rt=sapply(ac_df$text,function(tweet) trim(str_match(tweet,"^RT (@[[:alnum:]_]*)")[2]))
ac_df$rtt=sapply(ac_df$rt,function(rt) if (is.na(rt)) 'T' else 'RT')
```

Once we Run the lines above, we will get a dataset that includes the column called ‘created’ containing specific time each tweet was created in the form of ‘year-mm-dd hh-mm-ss,’ for instance ‘2020-05-05 17:27:40’ and the overall tweets contributors. We will arrange the usernames according to its chronological timeline in the display.

```markdown
# We will need to import the ‘plyr’ package to split the data apart.
library(plyr)
# order the replyToSN in the order that they were tweeted
ac_dfx=ddply(ac_df, 
             .var = "replyToSN", 
             .fun = function(x) 
             {return(subset(x, 
                            created %in% min(created),
                            select=c(replyToSN,created)))
             }
)
ac_dfxa=arrange(ac_dfx,-desc(created))
ac_df$replyToSN=factor(ac_df$replyToSN, levels = ac_dfxa$replyToSN)

# import the ‘ggplot2’ package to plot the result
library(ggplot2)
ggplot(tw.dfs) + geom_point(aes(x=created,y=replyToSN))
```

<img src="photo/pre-reply-stat.png" alt="hi" class="inline"/>

The long line at the top are tweets that contain no replyToSN value, so it appears as NA (Not Available). We can go a little further and plot the display of when Governor Cuomo replied, retweeted and/or neither. We will assign black dots to represent replies, red for retweets and blue to show tweets that are not replies or retweets. 

```markdown
# add color distributions and titles to the plot
ggplot(ac_df)+geom_point(aes(x=created,y=replyToSN), col="blue", size=1)
# add colors
ggplot()+
  geom_point(data=subset(
    ac_df,subset=(!is.na(replyToSN))),
    aes(x=created,y=replyToSN),col="black") + 
  geom_point(data=subset(
    ac_df,subset=(!is.na(replyToSN))),
    aes(x=created,y=rt),col="red")+
  geom_point(data=subset(ac_df,subset=(is.na(replyToSN) & is.na(rt))),
             aes(x=created,y=screenName),col="blue")+
  labs(title = "Cuomo's Reply Tweet Stats",
       subtitle = "Twitter accounts he replies to from April 28 to May 22",
       caption = "Source: data collected from Twitter's REST API via twitteR")
```
<img src="photo/reply-stats.png" alt="hi" class="inline"/>

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

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/aczx321/data-project/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
