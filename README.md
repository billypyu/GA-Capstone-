## Problem Statement:
By analyzing the earning call transcripts, would classification model provide value for speculative investors to trade next day in the U.S. automobile companies?

## Data
- 61 conference call transcripts was obtained from 2014 Q1 to 2019 Q1
1. Text sources: Earning call transcripts in the major automobile maker such as Ford,General Motors, Tesla, Fiat Chrysler are retrieved from seekingalpha.com with essential
subscription.
2. Numerical sources: Stock price and volumes are retrieved from Yahoo Finance

## Feature Engineering:
- Create a sentiment score columns for each transcripts by management discussion and analyst QA.
- Among the call transcripts, information such as earnings per share, year-over-year revenue status, esp status etc. will be extracted from the transcripts.
- The change of the stock price (open price - close price)/ close price from Yahoo Finance will be converted to the binary target variable.
- Some Python libray such as Textstat will be used to generate significant predictor features related to text physical properties such as number of and number of syllables, text complexity such as Smog Index, and lexicon complexity such as number of difficult words.

## EDA and Modeling Process
1. After merging the text and numerical dataset, transcripts will be splitted into Management Discussion (DA) and Analyst Question and Answer (QA). Features engineering will be derived from these two sessions. New features such as syntax sentiment, polarity, subjectivity, number of difficult words, readability indices, etc. will be added. After all, the numerical columns will be part of the data analysis
2. Second, text alone will be vectorized with n-grams for a new modeling process. Features of importance could provide investors what phrase or words matters
3. When the generalized model is completed, confusion matrix will be adapted to examine the performance of the classification. Additionally, a specialized model that only included dataset for one company will be modeled, followed by above engineering process. This aims to audit whether the classification model may provide a better prediction in one company rather than the entire industry.
4. Lastly, new stop words will be collected and introduced to the model for another round of evaluation if needed.

## Model Selection
Before modeling, one of the concern is multicollinearity as some of features are highly correlated if we use linear regression to predict continuous values. Meanwhile, we can not guarantee the independence of each features because management and analyst may have a similar point of view or sentiment in the call transcripts. However, referred to the setup of the problem statement, we want to predict either the market is up or down for speculative investors, classification model such as tree-based model, GBM-based model will work well in this dataset. Since we will use the combination of both, adding or removing correlated variables should not hit the accuracy scores but only decrease the computing time necessary. Since boosted trees use individual decision trees, they also are unaffected by multi-collinearity.

The ideal candidate will be Random forest and XGBoost for this data set. The importance matrix of an xgboost model is actually a data table object with the first column listing the names of all the features actually used in the boosted trees. The second column is the Gain metric which implies the relative contribution of the corresponding feature to the model calculated by taking each feature's contribution for each tree in the model. A higher value of this metric when compared to another feature implies it is more important for generating a prediction. Therefore, the ultimate goal will be providing insights to speculative investors on what to look for before next trading day.

The final adaption with XGBoost will provide good execution spend and model performance. While reviewing the bigrams for feature of importance, surprisingly, entity, location, and name are excluded. Generalized model will be defeated by noisy since each company has unique aspects and business operation, even though normalize the scale of the data set. However, the use of unsupervised learning such as KNN may be not applicable in this case.

Neural Network doesn't provide any value to classify the output. It may attributed to two issues:

    1. The non-random part of the relationship between the input and output is too small compared to the random part (one could argue that stock prices are like this). I.e. the input are not sufficiently related to the output. There isn’t an universal way to detect this as it depends on the nature of the data.
    2. Too much noise in the dataset as feature columns contains similar information that people are repeatedly talking in the call transcript
    The test and training size of is too small.

Therefore, neural network may not be the solution for this dataset

## Recommendation
1. The implication of using this model could help investors to understand of tone and language complexity of the management team and therefore suggest the market is either go up or down next day.
2. Investors could receive up or down signal from the classification model. They could compare whether these signals will be helpful to validate their intuition on next day trading when the earning call is released.
3. In general, the EDA process and model result suggest that firms with a high readability index tend to miss the target, suggesting a possibility that management team tries to increase their language complexity to avoid taking some issues. Therefore, investors could refer to the change of the readability index during the conference calls. While confirming with the numerical model, readability and negative sentiment by Analyst’s QA are weighted heavily on coefficients.
4. Automobile industry focuses on the expansion of North America and China, Inventory, margin profit, and future plan that are the important features, suggested by the XG Boost model. Generalized model (all companies) will be defeated by noises since each company has unique aspects and business operation strategy, even though normalizing the scale of the data set. However, the use of unsupervised learning such as KNN may be not applicable in this case.
5. The change of N-grams has a high impact on the model. For example, the bigrams provides more information than sing grams or trigrams, without scarifying too much on accuracy and overfiting.

## Limitations
1. More text data need to be added. Only four automobile makers’ quarterly call
transcripts from 2014-2019 were collected. Due to the small data set, this model is subjected to a limited use in a few automobile manufactures.
2. Neural network does not provide value to improve accuracy; also, lack of interpretation may make neural network not suitable for majority of investors.
3. Try to make more classes: ideal candidates could be market is up, down, flat. Alternative classification could be negative or positive value with a discrete range of 0-10%, 10-20%, 30-40%, 50% or above.
