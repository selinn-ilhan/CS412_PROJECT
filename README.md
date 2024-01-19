# CS412_PROJECT
The project is a basic note prediction algorithm, which has tried multiple classification and regression methods in order to achieve a higher accuracy prediction.
The end result is a python notebook, which has comments and markdowns to better explain which functions and which code block is used for what purpose.
The steps that can be found within the notebook are as follows:
## Initialization
This step marks the beggining of the project. From the code that was provided to use, we have the code2convos dictionary. From that dictionary, we created the user_texts and gpt_texts to train the data better. These two new dictionaries also have their corresponding lists to use in prompt matching, named similarly as user_texts_list and gpt_texts list. The final dictionary we acquired was only the code block from the answers that ChatGPT created, named only_codes. This step also makes the scores.csv into the dataframe scores.
In the end, there are 4 dictionaries(code2convos, user_texts, gpt_texts, only_codes), 1 dataframe(scores) and two lists(user_texts_list, gpt_texts_list) created during this step.

## Prompt Matching & Feature Engineering
### Prompt Matching
For prompt matching, we used TFIDF. We initially tried using word2vec, however the data we acquired didn't get the codes block due to the format we have put it in, and without the codes, the result was almost useless, so we proceeded with TFIDF. Instead we tried alternative methods to calculate the distance between the vectors. We used cosine, manhattan and eudclidean distance. The euclidean distances were much standardized than that of cosine. But the values were also lower. The euclidean distance danced around the 0.4 value, while cosine had values ranging from 0.98 to 0.25. Manhattan distance didn't perform well in the first place, giving values below 0.1. We didn't want to put all the data in one place by using euclidean, so we proceeded with cosine similarity. The values of questions with their individual users is named as a dataframe as "question_mapping_scores".

### Data Preprocessing
The above mentioned lists and dictionaries were transformed into dataframes:
- df_codes
- df_gpttexts
- df_usertexts
These dataframes were preprocessed in order to get better esult. The NA values have been dropped, they were all merged together as a complete dataframe named 'df'. The punctuation was removed, the letters were all made into lowercase and the most frequent block "python copy code" was removed so that the data was clearer. Then the stop words were removed by tokenization, and the words were stemmed. After all these preprocessing steps, the texts were joined together, ready for the feature engineering.

### Feature Engineering
For feature engineering here are some of the tables and features that were made:
- the occurence of top 10 words in user_texts in gpt_texts
- the most reoccuring word in all of the dataframes combined
- the least used words
- the correlation between the length and grade in
  - gpt answer length
  - user text length
  - code length
- the wordclouds for user_texts and gpt_texts

The features we have decided to use were:
- Most Frequent 5 words
- gpt answer length
- the times "import" was used
These were all merged into one complete dataframe, concatanated with the question_mapping_score.

## Prediction Model
The model was fit on X being every colum except the code and the grade, and y being the grade. The random_state was fixed at 42, the test_size was 0.2. 
We trained multiple classifier models as well as regression. 
The classifiers are as follows:
- Decision Tree Classifier with 0.32 accuracy.
- Gaussian Naive Bayes Classifier with 0.21 accuracy.
- Random Forest Classifier with 0.43 accuracy.
- SVC with 0.11 accuracy.
- KNN with 0.11 accuracy.
- Gradient Boosting Classifier with 0.32 accuracy.
The regressions are as follows:
- Gradient Boosting Regressor with R2 Test 0.30.
- Random Forest Regressor with R2 Test 0.30.
- Decision Tree Regressor with R2 Test -0.31.
*Little Experiment*
At the end of the prediction tab, there's a small experimentation. Before we started the project we were wondering whether it would be possible to predict the scores without using the feature engineering and prompt matching.

We decided to try it to see what would happen. First, we preprocessed the gpt_texts and user_texts dataframes, dropping the na values, stemming, removing stopwords, removing punctuations and lowering all the letters.
Sadly, but not unexpectedly, the results weren't very successful.  These classifiers are as follows:
- Random Forest Regressor with TFIDF, Prediction based on the answers Chat_GPT generated with R2 -0.75.
- Random Forest Regressor with TFIDF, Prediction based on the prompts users wrote with R2 -0.69.
