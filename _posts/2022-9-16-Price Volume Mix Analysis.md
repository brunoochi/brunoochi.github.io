---
layout: post
title: Price Volume Mix Analysis
categories: [Analysis]
---
PVM Analysis (as I like to refer to) breaks down sales by its drivers - price and volume - and measures the effect that each had in changing sales from period X to period Y. Let's say that we are in August 2022 and a restaurant wants to understand why its sales declined compared to August 2021. One way to go about it would be analyze if customers are spending on cheaper meals or if customers are not coming anymore. In other words, sales = average spending per customer x number of customers 

PVM analysis can be understood geometrically, like this:
<img src="#" alt>

Now, the "M" in PVM stands for "Mix". I believe that most versions of the PVM Analysis like to add a "Mix" effect that represents the portion of change in sales that are not solely exaplained by prices or volume. For simplicity, I think that it's best to just assume that the area occupied by "Mix" belongs to price. You could obviously assign it to volume. It's just an assumption.

So, I modified the PVM analysis a little bit to accommodate a situation where sales should be broken down into 3 drivers. Here's the business situation.

**"Desafinado Karaoke Lounge"** is a fake chain of karaoke lounges that operates in 10 different locations in Japan. Karaoke boxes in Japan usually requires that users register for a member card that allows the company to keep track of their metrics in exchange for rewards. So, we could divide sales into the following drivers:

- Average Spending per Member
- Average Numbers of Visit by the Member 
- Number of Members

Desafinado wants to understand why some locations do not perform as well as others. Should it encourage more membership? Or should it start a campaign to get inactive users back? Or should they try to get customers to buy higher value snacks and drinks? Since we have three variables, we need to imagine the volume of a cube instead of the area of a square.

So, I built a dummy data for our fake company and proceeded to calculate the difference in volume from one period to the other. I think there are a couple of different ways that we could calculate it. But here is the notebook if you are interested in the details.

This three paramater version of the PVM Analysis uploaded as a Tableau dashboard in Tableau public. Here's an image sample of the result.
<img src="{{ site.baseurl }}/images/blog/dashbordview.jpg" alt="PVM Analysis" width="80%" height="80%">