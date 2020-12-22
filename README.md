# Fetal Health 

Obstetricians have one of the most important jobs in this world, to help bring babies into the world. However, working in the health industry has many downsides, especially when a health care specialist has to deliver bad news, for example to expecting parents. It may occur that a baby is diseased before it is even born. Experts found ways to identify the health of a fetus based on cardiography. The heart rate of a baby can reveal the health of a fetus. The dataset consists of 2126 observations, extracted directly from cardiography measuring the heartbeat of a baby. There are 21 input variables and one response variable. The response variable is the health of the fetus and classified into three categories: 1- normal, 2- suspect, 3- pathologic. 

II.  Data Mining Methods
Based on the observations from the dataset on fetal health, I used 6 different models to predict the response variable Y on a new observation. This type of classification problem can be solved with a supervised machine learning model such as KNN, Classification Tree/ Random Forest, and Ordinal Logistic Regression. Fortunately, the dataset did not have any missing values and contained only numeric data.

II.I Ordinal Logistic Regression <br>
One of the models that can be used for this classification problem is ordinal logistic regression. Ordinal because the response variable (=Fetal Health) is ordered from 1 to 3, Normal, Suspect and Pathologic. First, I built a model with all independent variables and then worked my way backwards by selecting only significant variables on the 0.001 level of significance. I split the dataset into training and testing using 80 % of the data for training and 20% for testing. To get a better understanding of how the input variables affect the output variable, I predicted the probabilities of one observation falling into a particular category between 1 to 3. Because the most significant variables are Abnormal Short-Term Variability (ASTV) and % of Time with Abnormal Long-Term Variability (PTALTV), I calculated how a 1-unit increase in ASTV would affect the outcome. Based on the value of the coefficients, I found that a 1-unit increase in ASTV results in a 0.2% decrease in the probability of an observation to be classified as Normal or healthy. In addition, a 1-unit increase in PTALTV, reduces the chance by 0.1% of an observation to be classified as Normal or healthy. The formula below demonstrates how I calculated the probabilities. The coefficient values are directly from R and the value of 12.407 is the value of the intercept 1|2 (Class: Normal).
Formula: Y = (β1* X1)+(β2*X2 )+(β3+X3) 
                P (1)=  1/(1+EXP(-((12.407-Y)))
I found that building an ordinal logistic regression model was more complex and especially interpreting the results. That is because the coefficient values are used to calculate the probabilities. Once I found the probabilities for each class, I identified the highest probability and could predict the final class of an observation. The tables in Fig. 1 and Fig. 2 show the impact of a 1-unit change of both, ASTV and PTALTV, when holding all other variables constant. Please note that the tables do not contain all variables and the numbers would differ if we would use all variables. Finally, I used the confusion matrix to find the overall prediction accuracy which was ~85.0%. 

II.II  KNN
Secondly, I used KNN to build other models. The first step in the model development is to normalize all input variables to bring them onto a comparable scale. Then I applied a 5-fold CV to identify the best value for K. Fig. 3 illustrates that the best value for K is 3. The prediction accuracy on a new observation using 5-fold CV was ~88.4%. Then, I built a second model using KNN but this time I used a 10-fold CV. The best value for K was also 3 (Fig. 4). The prediction accuracy on a new observation using KNN with 10-fold CV resampling was ~88.0%.

Because M2 had an accuracy of 88.4%, it performed slightly better than M3. The accuracy matrix (Fig.5) illustrates that from 440 observations in the testing set, 389 were correctly classified while 51 observations were misclassified. This raises concern when classifying a pathologic case as normal and may be caused by the class imbalance which I will address at the end.

II.III Classification Tree
The third data mining technique I applied to this dataset was a classification tree. I first split the dataset into training and testing using the 80-20 holdout method. I built a tree model using the training data as my input. To find the best size of the tree, I applied ‘cv.tree’ to the model using ‘prune.misclass’ as the FUN parameter and a 10-fold CV. 
Based on the result, the best size of the tree was 17 or 13 illustrated in Fig.6. We can see that choosing any size between 13 and 17 significantly decreases the chance of misclassification. In Fig. 7, we can see that the most significant input variables are ASTV (=Abnormal Short-Term Variability), PTALTV (=Percentage of Time with Abnormal Long-Term Variability) and the Histogram Mean. These variables are more important in determining the class. With a 91.8% overall accuracy, the classification tree clearly exceeds the performance of the other three models built thus far. 

II.IV Bagging
The overall prediction accuracy of the classification tree was already pretty high. But to further improve the performance, we can apply Bagging. Again, I used the training dataset from the previous model to train the new model. I set mtry to 21, which is the total number of predictor variables. I applied the final model to the test set. The accuracy matrix in Fig. 8 illustrates that the overall prediction accuracy was 92.5% and the misclassification error was 7.5%.


II.V Random Forest
Lastly, I developed a model using the Random Forest. This helps us to find the most important variables. I used the training set to build the model and set mtry to 5, which is the square root of the input variables. Fig.9 illustrates that the accuracy would decrease if we were to remove PTALTV, ASTV and HistMean from our model. You could also say that the misclassification would be higher if we were to remove these variables. The model had an accuracy of 93.0% based on the accuracy matrix illustrated in Fig. 10. 

Implications 
One major concern I have to address is the class imbalance which may have contributed to the high accuracy. The dataset mainly contained observations where the response variable was of class 1 (=Normal) with 1655 occurrences, while class 2 (=Suspect) and 3 (=Pathologic) represented 590 and 528 occurrences, respectively. Some ways to fix this is either to collect more data or remove 1000 observations with class 1. However, this method would shrink the dataset and may decrease the overall accuracy. Another option is to use SMOTE which stands for “Synthetic Minority Oversampling Technique”. Since we haven’t learned it in class, I didn’t apply it but in real life that is definitely something to pay attention to. Besides the class imbalance issue, I didn’t have any other implications with this dataset.
In conclusion, the best model was the Random Forest. It had the highest accuracy out of all the models. The model is not perfect, but it performed well on the test data. As I mentioned above, the biggest issue is to classify a diseased fetus as healthy which has significant consequences for the expecting parent. Overall, I really enjoyed working on this project and I learned a lot from it. I hope that I will be able to apply these data mining techniques in the future and solve real-life problems.
