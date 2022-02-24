# Fraudulent Charges Predictions

## Can you predict fraudulent charges accurately using credit card transactional data?
* We used a data set of credit card purchases over a span of a few years.
* By determining and weighing various features, is it posible to predict if a charge is fraudulent or not.

## Reason for Selecting Topic
* Credit card fraud with the drastic increase of online purchases, is a significant risk for all of us.
* This model could be used by card companies to try and predict patterns in purchases to try and increase the rate of identifying and stopping fraudulent charges.

## Source Data
IEEE Computational Intelligence Society Fraud Detection presented by Vesta Corporation - The database uses a variety of features, kept anonymous of course, to show details about the transactions.
* Transaction timedelta as well as amount.
* Information about the payment card, such as card type, category, bank and even country.
* The address in addition to the distance between purchases and the email domain.
* A grouping of data showing how many addresses associated with the payment card, which were kept masked for security reasons. 

This dataset was made available in part of a contest by the Vesta Corporation to try and increase the security and accuracy of credit card purchases.

https://www.kaggle.com/c/ieee-fraud-detection/overview

## What questions we hope to answer with this data
* Is it possible to correctly predict whether a credit card purchase made was fraudulent or not.
* Do certain emails or card types have a higher fraud percentage.

## Tools used
### Database
* Amazon Web Services (AWS)
* Amazon RDS and S3
* Boto 3
* AWS Wrangler 

### Data Analyizing
* Pandas
* Numpy
* Python

### Machine Learning
* Scikitlearn - metrics, preprocessing, ensemble, neural_network, exceptions
* Tensorflow - Keras

### Dashboard
* Tableau
* Adobe Photoshop

# Machine Learning Model
* Our initial dataset yielded over 500K rows and 394 columns, showing timedelta, purchase amounts, card information, email and other address information, as well as other features displaying various information about each purchase.
* We then created categoricals, which is a column of labels, and using the get_dummies function into indicator columns. Which contain either 0s or 1s.
* After that a validation set was created using a fraction of the total rows, we opted for an 80%/20% split
* The dataset turned out to be very inbalanced so we implemented a Keras function called class_weights to balance it out. Simply put, depending on the inbalance it gives the portion of the data with the much lower number more importance to even out with the greater portion. An example, 500 samples of No and 1500 samples of Yes. The function would give the samples a weight of 3:1 respectively.
* We proceeded to determine the correlation between the variables to determine how they were associated with each other. We removed highly correlated fields except one of each group.
* Followed it up by getting the importances using all of the different variables defined previously in our code. Such as, our class_weights, correlations, sample of X, and feature importances.
* The StandardScaler was used to transform the Dataframe to finalize our X and Y test DF's

## Keras Model
The MLPClassifier, or Multi-Layer Perceptron classifier is an artificial neural network that generates a set of outputs from a set of inputs. As other models, it is characterized by several laters of input nodes connected as a directed graph (a signal path through the nodes only goes one way) between the input and output layers. 
* There were three layers used with the first two having 256 hidden layers activated on a "relu" and the third containing 1 hidden layer with the "sigmoid" activation. 
* It was given a max_iter of 10 with an adaptive learning rate, and we used the most common solver of "adam" because it works the best on large datasets. (solver for weight optimization)
* In total it gave us 201K trainable parameters
* We trained for a total of 200 epochs, and found after that there was a very slight drop off of our AUC score.
* The model ended up with a test accuracy of 91%
![image](https://user-images.githubusercontent.com/88358771/155445447-d7e2758d-972c-4241-9938-af298b3c7774.png)

* This is a printed version of our final epoch. It lists the model loss, true positives, false negatives, true negatives, false positives, and finally the AUC score.

  ![image](https://user-images.githubusercontent.com/88358771/155445396-e66ae090-9337-489c-a631-26cc5d8e57be.png)


* The output on the predictions was in probability form from 1-100 in a decimal form. Then we converted it to either a 1 or a 0, by rounding anything above a 50% to a 1.

Below is a picture of our model's AUC score and Loss over the 200 epochs ran.

![image](https://user-images.githubusercontent.com/88358771/155445234-e4e59f62-e425-400c-a553-a041365fe269.png)

![image](https://user-images.githubusercontent.com/88358771/155445257-89bb7a0e-12ae-497e-9cb1-41118889140f.png)


## Second ETL
After the neural network gave us our results in probability form, we were left with binary cartegorical fields with a positive number for true, or a negative number for false. 
* We uploaded the data into Tableau
* Using countifs, we were able to calculate how many times each email domain was in the dataset.
* Then using that were able to get the counts and percentages for how many times a fraud threat was predicted with each specific emails.
* Another aspect of the data was the card type, ie Visa, Mastercard, Amex, etc.
* We followed the same procedure with using countifs to count how many times each was used, as well as the percentage of fraud associated with each card.
* After all of the calculated fields were created, the data was loaded into a new dataset from a CSV.

Link to Google Slide presentation made with Photoshop
https://docs.google.com/presentation/d/18B8TswA3bCDmy5kV9OQGJ9oAVx-6OAJN11BGg8FJ9AE/edit#slide=id.p

Link to Tableau Dashboard
https://public.tableau.com/app/profile/josh.daniels1595/viz/CLASSPROJECT/SECUR-ITCENTER

![image](https://user-images.githubusercontent.com/88358771/155248303-f34af85b-e1a7-4ac2-878f-d7f2d0bac9f4.png)

