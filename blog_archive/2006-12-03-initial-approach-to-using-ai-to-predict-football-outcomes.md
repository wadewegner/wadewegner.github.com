---
author: admin
comments: true
date: "2006-12-03T20:09:16Z"
slug: initial-approach-to-using-ai-to-predict-football-outcomes
title: Initial approach to using AI to predict football outcomes
wordpress_id: 116
---

Note: an updated, more accurate method can be found [here](http://www.wadewegner.com/PermaLink,guid,76ca62d6-a214-450c-81fe-350bd73e4cbf.aspx).  
  
Over the past few months, I have been playing around with artificial neural networks (ANNs) in C#. For those of you unfamiliar with ANNs, it is an attempt to programmatically reproduce the way the brain processes data.

Below is a simple ANN architecture:

![](https://wadewegner.blob.core.windows.net/wordpress/content/binary/NeuralNetwork.png)

On the left we have inputs (i.e. data), and on the right we have an output (i.e. the outcome). The middle is a layer of neurons that essentially map the inputs to the output via weights (i.e. synapses).

The idea is that, given enough data to "train" the neural network", you can create a neural network that is capable of predicting an outcome.

There are many uses for ANNs; I am attempting to see if I can build a network to accurately predict football games.

I've created a VERY simple ANN, and come up with the following predictions for week 13 of the NFL:
 
	Arizona at St. Louis : Arizona  
	Atlanta at Washington : Atlanta  
	Dallas at N.Y. Giants : Dallas  
	Detroit at New England : New England  
	Indianapolis at Tennessee : Indianapolis  
	Jacksonville at Miami : Miami  
	Kansas City at Cleveland : Kansas City  
	Minnesota at Chicago : Chicago  
	N.Y. Jets at Green Bay : Green Bay  
	San Diego at Buffalo : Buffalo  
	San Francisco at New Orleans : New Orleans  
	Tampa Bay at Pittsburgh : Pittsburgh  
	Houston at Oakland : Oakland  
	Seattle at Denver : Seattle  
	Carolina at Philadelphia (Monday night) : Carolina

Now, I don't have a lot of faith in these predictions, because my inputs only consist of the following for the first 12 weeks:

	Away Team, Home Team, Did Away Team Win?

For example, the first line of my training data looks like this:

	17,25,0

Where 17 equals Miami, 25 equals Pittsburgh, and 0 means that the away team lost.

The idea is that the network is built based on 12 weeks of data, and given the inputs it can predict whether or not the away team wins or loses.

Once all the results are in, I'll post and share how well my predictions did! I'd be surprised if it was more than 50% accurate. In the future, I plan to try to find additional pieces of data to include in the training, so that the predictions become more accurate.

Note: I just tested this method against week 12 results (meaning I used weeks 1 through 11, excluding week 12), and found that I was only able to get it 50% accurate ... no better than randomly choosing.