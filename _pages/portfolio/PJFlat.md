---
layout: page
title: Project Flat
permalink: /portfolio/PJFlat/
---
This project uses basic natural language processing techniques to perform analysis on customer reviews of two well known video games franchises - Battlefield and Call of Duty. Both are traditional first person shooters (FPS) with online multiplayer game modes. They directly compete for customer attention to keep their servers populated and the game alive, so I assume developers are keen to learn what customers have to say so that they can implement updates that respond to their expectation.

I start by scraping game reviews from metacritic.com; then I proceed to tokenize the data using Spacy and do some exploratory work using wordclouds, network graphs and sentiment analysis. Finally, I use sklearn to "cluster" Battlefield 5 reviews into common topics that supposedly reflect general concerns / opinions of the reviewers.

**Tokenization**: After filtering out for English language reviews, I apply Spacy tokenization, adjust the list of stopwords, and visualize the top word count for each game title. The list for Battlefield 5, COD Black Ops, and COD Infinite Warfare shows the word "bad" among the most quoted words.

![alt text](chart1.png "Chart1")

**Wordcloud**: I perform some experiments to see how results shown in bar charts would look like in a wordcloud. Wordclouds are visually nicer, but it seems like the wordcloud for Battlefield 5 doesn't really reflect what was shown in the bar charts. For instance, the word "ww2" is definitely too small. Further research needs to be done to understand what's the logic behind the wordcloud.
