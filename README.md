# data-science-challenge

Welcome! The following challenge is to evaluate your strengths as a data scientist. The expectation is not that you would finish all of it, but represents all of the sorts of problems you might expect working on a typical problem. You should limit the exercise to no more than a few hours. When you're finished put your code in a VCS of your choice and share the link!

## Problem Statement

We are creating a machine learning algorithm that will predict the price of Bitcoin in USD in the next second based on the price from the last 60 seconds. The variable `price_high` is the target variable we wish to predict, our *Y*. The rest of the data can be used for your feature set, *X*. I have created a lookback function to expand the feature set and a min-max scaler to standardize the features. You can use this as your feature set, or modify it as you wish. The dataset consists of the first 10,000 seconds of Bitcoin data a single day, January 1, 2020 (until about 8:00 am). The data can be found in `bitcoin.csv`. We have a model in place, trained in `bitcoin-predictor.ipynb`. I started by using a deep neural network, because everyone keeps talking about how cool they are. Problem is, the model is not very good. Actually the model is completely awful and predicts `0` for everything. So, a lot of room for improvement!

The main challenge is this: can you make a model that actually works? You can use a neural network in tensorflow as I've done, or you can scrap it altogether and use regression, random forest, svm, anything you like. No model or package is off the table. Feel free to modify the jupyter notebook directly or put it in whatever workflow suits you best.

The main idea is to get a decent working model up and running quickly, but some other considerations you should think about are listed below. You don't have to answer all of the questions, but pick at least one or two and try to answer them (you can add your answers in this readme if you'd like).

Questions! 

1. If you used a different model, why'd you choose this model? 
  
I chose basic forecasting models because it's easier to diagnose why something could be off and it's easier to understand how to tune the parameters.  If I can spin up a basic model in a couple of hours to help me grasp the problem, I'll have an easier time building more complex models if I've understood the problem from the perspective of a model I've used before. 

I also read that ARIMA is a better model for short term forecasting, while the LSTM is better for long term. Since we needed to predict just one minute into the future, it seemed like a win-win (simpler model, less compute, better predictions). 

2. What about it made it work for this problem? Is this model complex and if so, is the complexity necessary? 

The model isn't exceptionally complex as it has relatively set parameters and tuning the model is pretty simple. 

3. Is it intuitive enough to explain it to a lay-person? 
  
I think so - most people can predict the season based on what temperature it is outside and how the plants look because they've experienced seasons before.  ARIMA does the same thing - it just recognizes repetitive data trends.

4. What was your optimizing metric? What were the hyperparameters and why'd you choose them?
  
The model has relatively set hyperparameters (p,d,q)
       - p is the number of autoregressive terms,
       - d is the number of nonseasonal differences needed for stationarity, and
       - q is the number of lagged forecast errors in the prediction equation. 

Parameters are chosen based on full and partial autocorrelation analyses to see how many lags fall above the level of significance. 

5. Did you include any regularization strategies in your model? If so, why'd you choose the one you did?
  
I did use a looping function to see which parameters fit the SARIMA model the best, but for ARIMA I used the autocorrelations and judgement to tune.

6. Did you include visualizations? (everyone loves a good graphic)
  
I used a bit of matplotlib and I threw in an Altair cause I've been using them a lot with fastpages and they're fun!

7. How do we know the model is good? How understandable are the diagnostics? How will we know how good the model is predicting in production?
  
The model predicts pretty well in the VERY SHORT TERM. I would do more work to check the test data. I would also set some assertions and pytests in the dagster pipeline.

8. If we see data for more than a single day's worth of prices, how do expect the model to perform? Will it generalize well to new data? Will retraining with this new data be an issue for this model?
  
Oh it'll definitely have diminishing returns.  Quickly.  Retraining the model wouldn't be an issue and would improve it over time. A different model would be better if you change the needed output - if you wanted to predict farther out, the ARIMA may not be as effective. 

9. **What question would you ask of the data, or add to this analysis that I haven't thought of?**
  
Honestly, I want to see this performed against a full year of data (although maybe minute by minute instead). 


## Note on Dependencies

Feel free to install dependencies however you wish (docker, pyenv, conda, etc.). This model was trained in a conda virtual environment, and you can find all of the details in the `spec-file.txt` (the output of `conda list`) file or the explicit packages in `spec-file-explicit.txt` (the output of `conda list --explicit`). You may upgrade/downgrade dependencies as you see fit, as long as it doesn't effect the solution.
