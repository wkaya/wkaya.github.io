---
layout: post
title: Predicting Bank Telemarketing Outcomes
---
With data gathered between May 2008 and Nov 2010 by a Portuguese bank on their telemarketing calls, we can predict whether a client will subscribe term deposit if the observations are analyzed as a classification problem.

Each marketing call has a set of personal features associated with the client, such as job type, marital status, education level, and age, which are client profile-related. This set of features also include a few that are related to the marketing call itself such as the number of previous calls and the month of the year for a call.

It also has a set of global features independent of the particular client, which indicate the social and economic context, such as consumer price index, employment variation rate, and Euro Interbank Offered Rate.

![Features]({{ site.url }}/images/b1.png)

Since there is a class imbalance between the cases where the client subscribed term deposit (success) vs the cases where the client did not subscribe term deposit (failure): 10% vs 90%, the data was upsampled with random oversampling to ensure better results.

A handful of models were trained using 70% of the dataset, and tested on the holdout 30% to predict the outcomes based on those features. The models were evaluated for F1 score and Accuracy. Since we’re interested in predicting the successful calls, we focus on the true positives which is the bottom right quadrant on the confusion matrix.

Looking at the confusion matrix associated with different models, it's easy to see that the Bernoulli Naive Bayes model and the Decision Tree model don't do very well, because the False Positive rate was much higher than for the better models, meaning these two models predicted about 2900 cases to be successes when these were actually failures. The better models made only about 1600 such predictions.

![Bernoulli-DT]({{ site.url }}/images/b2.png)

The Gaussian Naive Bayes model did the worst in this investigation, since its confusion matrix shows it did almost as bad as a dummy classifier for predicting False Positives.

![Gaussian-DC]({{ site.url }}/images/b3.png)

The Random Forest model produced the best accuracy score (0.88) out of all models, but its True Positive rate is also the worst out of all models. In this case, it's much better at predicting True Negatives (predicting actual failures) to compensate for the accuracy score, and very bad at predicting actual successes.

![Random-Forest]({{ site.url }}/images/b4.png)

After eliminating the above, the two remaining models (Linear Regression and SVM) are both very good, but ultimately Linear Regression was chosen because it has a slight advantage in predicting True Negatives (cases of actual failures). It also has the best F1 score overall, which is a weighted measurement of precision and recall.

![LR-SVM]({{ site.url }}/images/b5.png)

However, the F1 score is dependent on the positive class decision probability threshold that is chosen for this model. In this case, the best F1 score was achieved when a threshold of 0.74 was selected.

![F1-Threshold]({{ site.url }}/images/b6.png)

Taking into account the tradeoff between precision and recall, consider that our purpose is to predict successful outcomes (more True Positives and thus higher Recall), so the default threshold of 0.5 is better than trying to achieve the highest F1 score (at a threshold of 0.74) or accuracy.

![Precision-Recall-Tradeoff]({{ site.url }}/images/b7.gif)

In conclusion, looking at the most significant coefficients from the Linear Regression model, it becomes clear that the top 3 features predicting the success of the bank's telemarketing are actually not related to the individual client, but associated with the global economic climate.

Clients are more likely to subscribe term deposit when the consumer price index and the euro interbank offered rate are high. The interbank offered rate is just interest rate at European banks lending to other banks, so when it’s higher, the term deposit rate is more attractive. When the consumer price index is higher, there’s more inflation, so clients may be more conservative and are more likely to subscribe term deposit.

Clients are less likely to subscribe when the employment variation rate is high since it represents employees leaving or taking a job, and corresponds with robust economy, so when it’s lower, the economy might be worse, and people are more conservative with saving, thus more likely to subscribe term deposit.

![Top 3 features]({{ site.url }}/images/b8.png)

Additionally, False Positives may also be useful to the bank, because they are the group predicted by the model using the same set of features to be successes, and so more likely to have potential as future subscribers, whom the bank should consider enticing with other incentives.

![False Positives]({{ site.url }}/images/b9.png)



Data source: [Bank Marketing Data Set](https://archive.ics.uci.edu/ml/datasets/bank+marketing)

[Moro et al., 2014] S. Moro, P. Cortez and P. Rita. A Data-Driven Approach to Predict the Success of Bank Telemarketing. Decision Support Systems, Elsevier, 62:22-31, June 2014


### Tools
* Seaborn
* Matplotlib
* Pandas/Numpy
* SciKitLearn
* Tableau
* Plotly
