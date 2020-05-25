## A journalist's basic guide to scrape and analyze a Twitter account with R

Before we start, if it makes you feel better, I’m a low-tech millennial. 
As journalists, we seek to publish stories that are newsworthy and have potential. With high demand in daily news output, we turn to social media for breaking news, original stories and trends. And there’s no doubt that social media plays a prominent role in our society today. A survey conducted in 2019 by Ofcom on news consumption among adults and older children in the UK found that 49 percent of over 4,000 participants use social media to consume news. Meanwhile, across the pond, 55 percent of American adults often get their news from social media, according to the 2019 report by Pew Research Center.

Unlike other social media, Twitter allows real-time engagement without exclusivity. You can reply, like or retweet anybody’s tweet at any given time. Twitter is a free platform for advertising and its data can prompt big stories. You can guess a user’s characteristics or personalities by reading their tweets. Many politicians also use Twitter to engage with their constituents. [The New York Times](https://www.nytimes.com/interactive/2019/11/02/us/politics/trump-twitter-presidency.html) did a long read analyzing President Donald Trump’s tweets to find the way he was using Twitter since the 2017 inauguration with interactive graphs. One of the many interest part is that the data shows President Trump tweeted attacks in the early morning or later in the evening, when he is unlikely accompanied by his advisers.

The flood of information on Twitter or other social media could drown or sweep journalists away from original, relevant contents. So it’s necessary to find a tool to filter and analyze the information. This is where R enters the conversation. RStudio is free for use, easy to learn and good for creating data visualization.

This step-by-step guide covers the basics of Twitter scraping and data plotting with RStudio Cloud. Let’s get started:

### Step 1: Getting Twitter API access

Before scraping data from Twitter, we will need access codes. You have to create a new Twitter application which will allow you to connect to the API. You need to have a Twitter account to do this. First, apply for a developer account [here](https://developer.twitter.com/en/apply-for-access.html). Once you’re approved by Twitter you can then create a new application. You will then receive the credentials which are only for you to use.
Here’s a 2-minute video showing [how to create a Twitter application](https://www.youtube.com/watch?v=LpLYQz_3hA0&t=2s).




<img src="photo/cuomo.png" alt="hi" class="inline"/>

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
