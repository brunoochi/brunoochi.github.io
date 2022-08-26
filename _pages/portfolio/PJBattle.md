---
layout: page
title: Project Battle
permalink: /portfolio/PJBattle/
---
This project uses basic natural language processing techniques to perform analysis on customer reviews of two well known video games franchises - Battlefield and Call of Duty. Both are traditional first person shooters (FPS) with online multiplayer game modes. They directly compete for customer attention to keep their servers populated and the game alive, so I assume developers are keen to learn what customers have to say so that they can implement updates that respond to their expectation.

I start by scraping game reviews from metacritic.com; then I proceed to tokenize the data using Spacy and do some exploratory work using wordclouds, network graphs and sentiment analysis. Finally, I use sklearn to "cluster" Battlefield 5 reviews into common topics that supposedly reflect general concerns / opinions of the reviewers.

**Tokenization**: After filtering out for English language reviews, I apply Spacy tokenization, adjust the list of stopwords, and visualize the top word count for each game title. The list for Battlefield 5, COD Black Ops, and COD Infinite Warfare shows the word "bad" among the most quoted words.

![alt text](chart1.png "Chart1")

**Wordcloud**: I perform some experiments to see how results shown in bar charts would look like in a wordcloud. Wordclouds are visually nicer, but it seems like the wordcloud for Battlefield 5 doesn't really reflect what was shown in the bar charts. For instance, the word "ww2" is definitely too small. Further research needs to be done to understand what's the logic behind the wordcloud.

![alt text](chart1.png "Chart1")

**Co-Occurrence Network**: Co-occurrence network is a graph that shows words as nodes and co-occurrence of words as edges. The thicker the edge, the more often liked words appear together (keep in mind the position or proximity of the nodes have nothing to do with co-occurence).

I picked the top 100 words for Battlefield 5 and built the co-occurrence network with some modifications to to make top co-occuring words to stand out (fun-time, great-people, women-gameplay, etc).

![alt text](chart1.png "Chart1")

**Sentiment Analysis**: In this section, I use the NLTK library to score reviews and measure overall sentiment. The daily averages scores were plotted in the chart below. Scores of 1 are the most positive and -1 the most negative.

![alt text](chart1.png "Chart1")

We can clearly see that Battlefield 1 was very well received by reviewers - very few red bars compared to its peers. Battlefield 5 had a very bad launch with only a couple of blue bars until it achieved its 70th day after launch (assuming first review was on its launch day). Among the Call of Duty games, Infinite Warfare seems to have received more positive reviews than Black Ops.

Games like Battlefield and Call of Duty, get updates from developers periodically. Not only bug fixes and improvements are made, but also new items, new maps, and even new game modes are added which I thought would dramatically influence reviews over time. Unfortunately, looking the data, reviews does not seem to change a lot over time. I would say that, perhaps, the first 20-30 days reviews are a tend to be more extreme to either positive side or negative side, then reviews get "balanced out" over time. It seems like those first reviewers sets the overall tone for other reviewers to write their own assessment of the game - bad initial reviews will translate into a slightly negative trend over time.

**Topic Model**: The last task of this project is to run the topic model against Battlefield 5 reviews. The goal is to find and label reviews that seem to share common topics. As discussed in the sentiment analysis section, Battlefield 5 did not have great reviews, so I want to delve deeper into the text data and extract disscussion points raised by customers.

I use LDA (Latent Dirilecht Allocation) algorithm to calculate the probability of words belonging to a topic and the probability of these topics being assigned to reviews. I experiment with four topics and see what kind results I get. Let's check the top 15 words in probability of belonging to each topic (top probabilities appear on the right):

- Topic 0 : ['war', 'historical', 'ussr', 'bf1', 'money', 'playing', 'fun', 'dice', 'buy', 'played', 'people', 'lot', 'bugs', 'ea', 'don']
- Topic 1 : ['german', 'instead', 'played', 'got', 'feminism', 'story', 'time', 'ww2', 'best', 'good', 'women', 'war', 'history', 'ea', 'dice']
- Topic 2 : ['content', 'graphics', 'story', 'great', 'history', 'bf1', 'gameplay', 'don', 'war', 'maps', 'ea', 'buy', 'bad', 'ww2', 'good']
- Topic 3 : ['weapons', 'lot', 'great', 'far', 've', 'player', 'bf1', 'played', 'maps', 'time', 'fun', 'good', 'feels', 'campaign', 'multiplayer']

Now, we need to think about what each topic seem to be talking about. For example, topic 0 has words like "bugs" and "ea" in the top - let's assign the label of "Bugs" to it. For topic 1, we see "history", "war", "woman" - people seem to be discussing the historical accuracy of Battlefield 5 - label: "History". I label Topic 3 as "Other" and topic 3 as "Multiplayer". I assign the labels to each review by picking the topic that has the highest probability and get 166 reviews for bugs, 137 for history, 178 for multiplayer and 219 for others.

Links:
- Used this link as reference to build the co-occurrence network: Simple Co-occurrence Network
- Check the ipynb notebook here
