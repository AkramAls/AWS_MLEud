# Report: Predict Bike Sharing Demand with AutoGluon Solution
#### Akram Al-Selwadi

## Initial Training
### What did you realize when you tried to submit your predictions? What changes were needed to the output of the predictor to submit your results?
I had to go back and include some EDA and add certain code blocks that were needed are missing. Having to figure out what was needed was too diffucult and time consuming as the course doesn't cover those and it stopped my progress. I also reliazed when trying to submit my code that getting Autogluon on my local computer was impoosible as it will not let you do so. I also had to be careful with my budget for AWS.

### What was the top ranked model that performed?
My second model preformed well with a score of 0.47874. With improvement score of -33.957602.

## Exploratory data analysis and feature creation
### What did the exploratory analysis find and how did you add additional features?
The datetime feature was dropped creating coulmns of data for year, month, day (dayofweek) as features using feature extraction. Using data visualization from seaborn and matplotlib libaries, we look for insights from the features. For example, making a corrlation heatmap, looking at the temp and atemp on the corrlation heatmap, there is a high positive correlation of 0.98. Because two different feature have such a high corrlation we can drop one of them and choose atemp.

### How much better did your model preform after adding additional features and why do you think that is?
The best improvement was about 132% after doing some feature engineering with spliting the datetime value into speparate features.

## Hyper parameter tuning
### How much better did your model preform after trying different hyper parameters?
Performed better than the first test, but no better than then after the second test. I assume because there was too many parameters to run though or the dataset wasn't ideal for that thrid test.

### If you were given more time with this dataset, where do you think you would spend more time?
Dropping some feature to test if some features were holding back performance. I would propably also try to test with Linear Regression instead of Autogluon.

### Create a table with the models you ran, the hyperparameters modified, and the kaggle score.

	model 	timelimit 	presets 	hp-method 	score
0 	initial 	time_limit = 600 	presets='best_quality' 	none 	1.80148
1 	add_features 	time_limit=600 	presets='best_quality' 	problem_type = 'regression' 	0.47874
2 	hpo 	time_limit=600 	presets='optimize_for_deployment' 	tabular autogluon 	0.53890

### Create a line plot showing the top model score for the three (or more) training runs during the project.


![model_train_score.png](project/model_train_score.png)

### Create a line plot showing the top kaggle score for the three (or more) prediction submissions during the project.


![model_test_score.png](project/model_test_score.png)

## Summary
With EDA, we got great insight on our models performance evalutation. I did wish that some code blocks and infomation that were missing were included and that some functions were already there. Going online to find documentation for some of the models and libaries was too time consuming and took foucs on what the course was trying to teach. Issues like installing Autogluon on my local device becomes a hassle. Including maybe having more updated steps of the UI's for Kaggle and AWS.