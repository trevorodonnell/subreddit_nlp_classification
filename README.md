# Problem Statement

I work for the Service Employees International Union, a labor union representing almost 1.9 million workers in over 100 occupations in the United States and Canada. The SEIU is considering whether to expand the scope of their organization, to include servers as well. They want to better understand what the server experience is, and the main factors that differentiate it from the server experience. 
My job is to create a predictive model that will classify whether a post comes from either the retail or server subreddit based on the title and text of the post. 

# EDA

Retail has more unique authors, both in absolute and relative terms, with about seventy unique auhtors per one hundred posts, where servers only have about sixty-five unique authors per one hundred posts. It seems that many more authors have been deleted from the retail subreddit, presumably due to corporate interest in mitigating slanderous content, where independent restaurants may not have incentives or resources to do so. Retailers are much more likely to not comment on their posts, with almost half their posts recieving no comments. 

Notably, servers only had less than a tenth of their total posts left without commentary. We examine that servers are only slightly more unlikely to upscore a post compared to retailers. It seems average our servers are slightly more verbose than their retail counterparts. However, the longest post from the retail subreddit is close to double length of the longest post from server subreddit. We can see that servers have higher maximums for their comment count columns and their score columns.

The post length variables are have a fairly similar, otherwise having a range with a maximum value around 15,000 and an extremely positively skewed distribution. We are essentially missing an entire years worth of posts in both dataframes. Other than that the two individual datasets follow similiar patterns; both are fairly flat, spare 2016 which experiences a discernible ebb in posts.

February and September are low post producing months, January and May and high post producing months for both subreddits. End of year month posts peak in November for the retail subreddit, presumably due to big promotions such as Black Friday, while servers peak in December due to busier shifts during the winter holidays.

Putting the numeric data from the dataframes into correlation matrices didn't offer very many insights. The only moderately strong correlation which occured in both dataframes, was between the number of comments and the score for a given post. This relationship was stronger in the retail subreddit, with a correlation coefficient of .68, compared to the weaker, but still present correlation of .45 in the server subreddit. 

I was hoping by creating a concatenated dataframe, I might be able to find insights between variables across the dataframes, and with the exception of the time variables, which can be overlooked due to their autocorrelated nature, nothing exciting manifested. 

Focusing on the relationship between our two most strongly correlatied variables, within the merged dataframe, number of comments per post and the post score. The value of the correlatative strength of these variables is .55, a moderately strong and positive correlation. 

# Modeling & Evaluations

Instantiating our TFIDF (term frequency-inverse document frequency) vectorizer. With foundations similar to a CountVectorizer this goes beyond the standard bag-of-words approach, increasing the value of a tokenized word if occurs it frequently in fewer documents, while decreasing the value of more ubiquitous terms. We will use english arguement for our stop_words parameter, removing common English words based on a predetermined list which stored in the TFIDF transformer. 

Our data is almost perfectly balanced each with each value having close to 50% of the total count, which will act as our baseline score. 

Our logistic regression model scored close to 94% accuracy on our training data, and approximately 91% on our testing data. These are pretty good scores considering we are using unscaled variables without adjusting any of the logistic regression model hyperparameters. Furthermore we can also tell that our model is overfit, with the testing data's accuracy score four percent lower than the training data; which suggests our model suffers due to high variance. 

Our KNN model had a training score of 62.5% and a test score 54.0%. Not only do these scores illustrate how wildly little predicitive power our models have, they also suffer from significantly higher variance, making them even more overfit than our logistic regression models. Granted, this model didn't maximize hyperparameter tuning, but given its poor intital performance (and massive computational cost for my puny Mac laptop) we will forgo using this model as our production model and will defer to the previously explored logistic regression model. The logistic regression model has the additional advantage of having interprettable coefficients.  

The coefficients from the logistic regression model that best indicate whether a reddit post was written by a server instead of a retail worker are perhaps obvious, and certainly telling. Servers distinguish themselves by refering to their place of employment (restaurant), themselves (server), the way the reference their cusomters (table), and their incentives (tip and bar). 

Terms that are most indictative of not being written by a server (and therefore a retail worker) also include a lot of innocous work references, but are indeed more specific to the realm of the retail domain. The place of work (store), the type of work they are doing (retail), how they refer to their cusomters (customers), as well as their positions and objects they interact with. Worth mentioning that "lady" is more associated with retail than service. 

In terms of our classifications metrics, we aforementioned that the accuracy score for our test data was approximately 91%. Our sensitivity/recall score was ~87.4% and our precision score at ~94.6%, which tells us that our model is more likely to predict false negatives than false positives. 

# Recommendations

Based on our analysis, modeling, and evaluation of the model performance and examination of its coefficients; we put forth the following recommendations for the SEIU:

1. Do not include servers into SEIU

2. Based on the coefficients, it seems like the server experience is myopic, conceited, opportunist, and possibly alcoholic.

3. This does not align with the core values of industrious retail workers and could be a liability to our cause.
