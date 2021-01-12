---
author: admin
comments: true
date: "2006-12-04T00:29:18Z"
slug: using-ai-to-predict-football-outcomes
title: Using AI to predict football outcomes
---

In a [previous post](http://www.wadewegner.com/PermaLink,guid,a50d0f5b-ecbe-4716-9276-9ec9cb7656a4.aspx), I attempted to create a very simple artificial neural network (ANN) to predict the outcome of NFL games. I found that the approach I took was really no better than random guesses, as I had an average accuracy of 50%.

Consequently, I decided upon a different approach. I used the same network architecture, but I added data which allowed the network to make more accurate predictions. This time, I used the following data:

	Week Number (1-17)  
	Away team Yards per Game differential  
	Away team Yards per Play differential  
	Away team First Downs per Game differential  
	Away team Time of Possession differential  
	Home team Yards per Game differential  
	Home team Yards per Play differential  
	Home team First Downs per Game differential  
	Home team Time of Possession differential  
	Bias (binary value of 1)

**Note:** since originally writing this, I expanded the differentials. Essentially, I'm grabbing everyting from the [NFL stats page](http://www.nfl.com/stats/2006/regular) (requires less work on my end) and creating differentials between the defense and offense.

A typically row pattern looks like this ...

	01031033044044027029025049B

... where the first two characters represent the week number, the next three the away team yards per game, and so on. This value is then converted into a binary array (of 105 binary values), and added to a collection of binary arrays. This collection represents the training data. Each "pattern" is pushed throw the network and found to be either true (meaning the away team won) or false (meaning the away team lost). The error gradient is calculated, and then backpropogation (a subject for another post) refines the weights, and continues to try and get the data to converge. I decided it was practical to go to an RMSE value of .15 on the training data, before making the prediction.

With this approach, I made predictions for week 11 (excluding week 11 data, of course) ten times, and averaged the values. I then compared it to the actual results of the game. I found the accuracy to be 81.25%, which means it made an accurate prediction 13 of 16 times!

So, I decided to include week 12 again, and make predictions for week 13. Here are my predicted winners:

	St. Louis  
	* Atlanta  
	* Dallas  
	* New England  
	Indianapolis  
	* Jacksonville  
	* Cleveland  
	Minnesota  
	* N.Y. Jets  
	* San Diego  
	* New Orleans  
	* Pittsburgh  
	* Houston  
	* Seattle  
	Carolina

	* Accurately predicted!

So, for week 13, I have accurately predicted 11 of the 15 games (73% accuracy)! This approach has MUCH more promise than the previous approach, yet isn't as good as I'd like it to be. I have some ideas one how to make this even MORE accurate! Stay tuned!