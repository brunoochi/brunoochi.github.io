---
layout: page
title: Project Battle
permalink: portfolio/PJBattle/
---
# Project Battle
This project uses basic natural language processing techniques to perform analysis on customer reviews of two well known video games franchises - Battlefield and Call of Duty. Both are traditional first person shooters (FPS) with online multiplayer game modes. They directly compete for customer attention to keep their servers populated and the game alive, so I assume developers are keen to learn what customers have to say so that they can implement updates that respond to their expectation.

I start by scraping game reviews from metacritic.com; then I proceed to tokenize the data using Spacy and do some exploratory work using wordclouds, network graphs and sentiment analysis. Finally, I use sklearn to "cluster" Battlefield 5 reviews into common topics that supposedly reflect general concerns / opinions of the reviewers.

## 1. Data
Customer reviews scraped from metacritic.com:
- Battlefield 5 Link to website
- Battlefield 1 Link to website
- Call of Duty BlackOps 4 Link to website
- Call of Duty Infinite Warfare Link to website

## 2. Tool Specs
- Python: 3.7 version
- Libraries: requests, beautifulsoup, pandas, matplotplib, networkx, langid, spacy, nltk, scikit-learn

## 3. Procedures
Data Extraction: Used request library to webscrape and beautifulsoup for data parsing. Extracted four dataframes (one for each game) with "user name", "user score", "date posted", and "review text" columns.

Tokenization: After filtering out for English language reviews, I apply Spacy tokenization, adjust the list of stopwords, and visualize the top word count for each game title. A quick glimpse of what has been talked about the games. The list for Battlefield 5, COD Black Ops, and COD Infinite Warfare shows the word "bad" among most quoted words. Also, it seems like zombies are a major part of COD games.

![alt text](chart1.png "Chart1")

Wordcloud: I perform some experiments to see how results shown in bar charts would look like as a wordcloud. Wordclouds are visually nicer, but it seems like the wordcloud for Battlefield 5 doesn't really reflect what was shown in the bar charts. For example, the word "ww2" is definitely too small. Further research needs to be done to understand what's the logic behind the wordcloud.

![alt text](chart1.png "Chart1")

Co-Occurrence Network: Co-occurrence network is a graph that shows words as nodes and link these nodes with edges (or lines). These edges are weighted using the jaccard coefficient, which means that thicker edges represent nodes that occurr together often (keep in mind the position or proximity of the nodes have nothing to do with co-occurence).

I picked the top 100 words for Battlefield 5 and built the co-occurrence network using this link as a reference. At first, there were so many edges that it was a little bit hard to interpret the graph, so I made some adaptations to make top co-occuring words stand out (fun-time, great-people, women-gameplay, etc). I also filtered out a single word (fun) to check co-occurring words in detail. Being able to write co-occurrence in Python allows for a lot of flexibility!

Sentiment Analysis: In this section, I use the NLTK library to score reviews and measure overall sentiment. The daily averages scores were plotted in the chart below. Scores of 1 are the most positive and -1 the most negative.

We can clearly see that Battlefield 1 was very well received by reviewers - very few red bars compared to its peers. Battlefield 5 had a very bad launch with only a couple of blue bars until it achieved its 70th day after launch (assuming first review was on its launch day). Among the Call of Duty games, Infinite Warfare seems to have received more positive reviews than Black Ops.

Games like Battlefield and Call of Duty, get updates from developers periodically. Not only bug fixes and improvements are made, but also new items, new maps, and even new game modes are added which I thought would dramatically influence reviews over time. Unfortunately, looking the data, reviews does not seem to change a lot over time. I would say that, perhaps, the first 20-30 days reviews are a tend to be more extreme to either positive side or negative side, then reviews get "balanced out" over time. It seems like those first reviewers sets the overall tone for other reviewers to write their own assessment of the game - bad initial reviews will translate into a slightly negative trend over time.

Topic Model: The last task of this project is to run the topic model against Battlefield 5 reviews. The goal is to find and label reviews that seem to share common topics. As discussed in the sentiment analysis section, Battlefield 5 did not have great reviews, so I want to delve deeper into the text data and extract disscussion points raised by customers.

I use LDA (Latent Dirilecht Allocation) algorithm to calculate the probability of words belonging to a topic and the probability of these topics being assigned to reviews. This is unsupervised machine learning, so we don't have pre-labelled data to feed and test our model, which makes it hard to validate results. Also, the analyst needs to set the number of topics he/she expects to find; I experiment with four topics and see what kind results I get (there is no right or wrong answwers).

Let's check the top 15 words in probability of belonging to each topic (top probabilities appear on the right):

- Topic 0 : ['war', 'historical', 'ussr', 'bf1', 'money', 'playing', 'fun', 'dice', 'buy', 'played', 'people', 'lot', 'bugs', 'ea', 'don']
- Topic 1 : ['german', 'instead', 'played', 'got', 'feminism', 'story', 'time', 'ww2', 'best', 'good', 'women', 'war', 'history', 'ea', 'dice']
- Topic 2 : ['content', 'graphics', 'story', 'great', 'history', 'bf1', 'gameplay', 'don', 'war', 'maps', 'ea', 'buy', 'bad', 'ww2', 'good']
- Topic 3 : ['weapons', 'lot', 'great', 'far', 've', 'player', 'bf1', 'played', 'maps', 'time', 'fun', 'good', 'feels', 'campaign', 'multiplayer']
Now, we need to think about what each topic seem to be talking about. Topic 0 has words like "bugs" and "ea" in the top - let's assign the label of "Bugs" to it. For topic 1, we see "history", "war", "woman" - people seem to be discussing the historical accuracy of Battlefield 5 - label: "History". Topic 2 is a little bit hard to interpret as the top words are not particularly distinctive - "good", "bad", "buy", "ea" - let the label be "Other". Lastly, topic 3 has words like "multiplayer", "campaign", and "fun"; people must be focusing on the different modes of the game - I label it as "Multiplayer".

Next, we assign the labels to each review. As mentioned earlier, LDA calculates the probability of each review belonging to each topic (ex. 80% topic 0, 10% topic 1, 0% topic 2, 0% topic 3). For simplicity, I choose the topic with highest probability and I get 166 reviews for bugs, 137 for history, 178 for multiplayer and 219 for others.

The last step is to check reviews and labels and see if they make sense. For example, this review has 98% probability of belonging to "History":

>"Let me get it straight - this game throws any historical accuracy right throw the window. And I'm not even talking about this whole "robotic females" fiasco. I'm talking about "Norwegian heavy water facility sabotage" - the operation which literally prevented Nazi Germany from developing a nuclear weapon . In REAL history Allies lost ~40 brave men in sequence of operations and onlyLet me get it straight - this game throws any historical accuracy right throw the window. And I'm not even talking about this whole "robotic females" fiasco. I'm talking about..."

This got 99% probability of being "Multiplayer".

>"Is it fun? Yes, it definitely is I’ve loved every minute of the 25+ hours I’ve put into the multiplayer. The multiplayer is a lot more tactical then Battlefield 1 and a little slower Paced as well, (but not to slow) a lot more destruction and more realistic destruction like pieces of building hanging when you blow a hole into the wall, or being able to shoot through the wall with highIs it fun? Yes, it definitely is I’ve loved every minute of the 25+ hours I’ve put into the multiplayer. The multiplayer is a lot more tactical then Battlefield 1 and a little slower Paced as well, (but not to slow) a lot more destruction and more realistic destruction like pieces of building hanging when you blow a hole into the wall, or being able to shoot through the wall..."

These results don't look bad at all. But if you screen over the other reviews you will find lots of labels that makes no sense. Also, picking the highest probability labels is very simple, but what should we do for "borderline" reviews? Is 4 a reasonable number of labels, in the first place? It is the work of the analyst to improve results by tweaking the parameters, performing better data processing, or even considering methods other than LDA to estimate topics.

## 4. Conclusion
This project used basic NLP techniques to analyze customer reviews of two major FPS game franchises - Battlefield and Call of Duty. NLP is not a field of expertise that I am very familiar with, so this project was quite challenging (and rewarding).

## 5. Links
- Used this link as reference to build the co-occurrence network: Simple Co-occurrence Network
- Check the ipynb notebook here
