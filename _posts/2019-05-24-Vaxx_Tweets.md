---
layout: post
title: Twitter Topics -- Anti-vaccination and Pro-vaccination
---
Although it's easy to tell whether a Twitter user is antivax or provax when they're tweeting about vaccination, is there any way to differentiate between these two groups when they're not? Natural Language Processing provides the tools to analyze tweet content and investigate the salient topics in a group's messages.

The tweets from the antivax and provax groups were obtained by first searching the web for a few popular articles and accounts that are widely retweeted by each camp. Then the user IDs of accounts that retweeted these were gathered via tweepy. Thereafter, the tweets available via Twitter's API from each user ID were collected and stored in MongoDB, since these tweets do not have a fixed schema, and a non-relational database is most suited in this case.

Everything else besides the text portion of each tweet is stripped from the dataset; the text strings are further cleaned so that only English content remains; and usernames, punctuations, and numbers are removed from the strings. Empty words (very, because, which, and, etc.) and repeated words common to both groups (followers, liked, retweets, vaccine, etc.) are also removed. An attempt at stemming and lemmatization did not result in significant improvement in this investigation, but may prove worthwhile elsewhere.

![Method1]({{ site.url }}/images/c1.png)

The remaining sentence is then broken up into units of a single word or 2 adjacent words. From each of the two camps, we try to find common words or units of 2 adjacent words, then group the similar ones together and interpret the result for salient topics. The process of breaking up the string into word units relies on CountVectorizer or TFIDF vectorizer. After which, the process of finding common word units and grouping them together, or extracting latent topics from the word units, can be done via a few topic modeling techniques including Latent Semantic Analysis and None Negative Matrix Factorization.

![Method2]({{ site.url }}/images/c2.png)

Below we see the groupings or topics of similar single word units from both camps:

![Antivax single words]({{ site.url }}/images/c3.png)
![Provax single words]({{ site.url }}/images/c4.png)

It appears that the antivax group has some topics about fitness, exercize, and healthy eating, which may imply belief in taking an active role on the part of the individual in controlling their own health. Meanwhile, the provax group has some topics on politics and prevention of diseases, healthcare and voting, which may imply an approach to health through public legislation and group effort.

Since we're only getting limited insight from single word units, we should find the results more interpretable when we also look at the two-adjacent word units.

![Antivax adjacent words]({{ site.url }}/images/c5.png)
![Provax adjacent words]({{ site.url }}/images/c6.png)

Topics of 2 adjacent word units shown above seem to suggest that the antivax group places significant emphasis on fitness and exercize, and there is also concern about aluminum in food causing Alzheimer's, but there’s also astrology, cosmic abandonment, natural law, and the occult. A possible guess in light of the topics on inspiration and leadership might point all these toward a similar focus on the individual’s efforts to improve their own health.

In the same vein, the provax group shows political interests in the Brexit Remainers Unite and White House topics. There's also promotions of public health awareness and accident prevention, and calls to help the underprivileged, which fit with their group effort approach.

Although only the top 5 topics were shown in the above cases, we can use clustering techniques to group together more of the other (say, the top 30) topics to visualize how they're distributed. For example, using K-Means clustering or DBSCAN clustering, the high dimensions in the topic space can be compressed to 3D space for visualizing with T-SNE.

K-Means

| Cluster # <img width=1/> | tweets <img width=1/> |
| ---:|---:|
| 0 | 1922 |
| 3 | 28 |
| 1 | 20 |
| 4 | 15 |
| 2 | 15 |

![K-Means clusters with T-SNE]({{ site.url }}/images/c7.gif)

DBSCAN

| Cluster # <img width=1/> | tweets <img width=1/> |
| ---:|---:|
| 0 | 1808 |
| -1 | 95 |
| 4 | 20 |
| 7 | 19 |
| 3 | 15 |
| 5 | 14 |
| 11 | 6 |
| 6 | 6 |
| 9 | 5 |
| 10 | 3 |
| 8 | 3 |
| 2 | 3 |
| 1 | 3 |

![DBSCAN clusters with T-SNE]({{ site.url }}/images/c8.gif)

In these visualizations, the yellow dots represent the most salient tweets, Cluster 0, from antivax users. The 4 other clusters assigned by the K-Means algorithm only take up a few regions in the space. With DBSCAN clustering, the black dots scattered randomly around the entire space are actually the noise designated by DBSCAN, meaning they don’t belong in any of the 12 clusters.

The visualizations showed one oversized group compared to all others, which seems to indicate a general focus in the tweeted content, and such overall trends can be summarized by how each group is distinguished by their outlook on health:

![Antivax and Provax topic 0s]({{ site.url }}/images/c9.png)

Again, these appear to fit closely with the themes below:
Antivax -- individual fitness and exercize
Provax -- group health promotion and illness prevention

These twitter accounts seem to be quite focused in their topics, and it’s probably not hard to guess which camp a user belongs to if one observes a handful of their tweets.


Data source: [Twitter APIs](https://help.twitter.com/en/rules-and-policies/twitter-api)



### Tools
* Tweepy
* MongoDB
* nltk
* SciKit-Learn
* Matplotlib
* Plotly
