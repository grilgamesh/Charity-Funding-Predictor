# Charity-Funding-Predictor

Charity Funding Predictor Report
Overview 
AlphabetSoup are a non-profit organisation, who allocate funding to applicants. The purpose of this project was to build a tool to help in the decision making process, and predict, based on the available features, with upwards of 75% success, whether an applicant for funding would successfully make good use of it. To implement this, I created and trained an artificial neural network (NN) on prior data.


Results:


Data Preprocessing


What variable(s) are the target(s) for the model?
The IS_SUCCESSFUL column, featuring only binary values of 0 and 1.
What variable(s) are the features for the model?
APPLICATION_TYPE—Alphabet Soup application type
AFFILIATION—Affiliated sector of industry
CLASSIFICATION—Government organization classification
USE_CASE—Use case for funding
ORGANIZATION—Organization type
STATUS—Active status
INCOME_AMT—Income classification
SPECIAL_CONSIDERATIONS—Special consideration for application
ASK_AMT—Funding amount requested
What variable(s) should be removed from the input data because they are neither targets nor features?
EIN and NAME are identifiers and not relevant for classifying the data.
Compiling, Training, and Evaluating the Model


How many neurons, layers, and activation functions did you select for your neural network model, and why?
I initially chose two hidden layers, with eight neurons in each layer, and a Relu function in each case. This is because I wanted to start fairly small, knowing that I could increase it later if necessary, however the data had a large number of inputs:
 
and some numerical columns (e.g. ‘ASK_AMT’)  had a large number of values: 
Were you able to achieve the target model performance?
Despite many tweaks to the model, my very best score on unseen data was only 72.6%, so I did not meet the 75% threshold on this first pass.
What steps did you take in your attempts to increase model performance?
My first thought was to increase the number of hidden layers by one:
As that did not improve the accuracy, I increased the number of neurons in each layer:
As that also did not improve accuracy, I changed the function from relu to tan: 
None of this improved the performance on the model at all, leading me to two more potential improvements: firstly to further process the data, and secondly to allow tensorflow to auto-tune the NN model.
I therefore wrote a method to throw out outliers in the data, given defined parameters
and went through each column, choosing a cut-off level for outliers, and removing them.
This reduced some columns to only one variable, rendering them pointless, so they were dropped.
I decided also to try to reduce noise in the data by reducing the two numerical columns into bins (or fewer bins)

All of these simplifications combined reduced the number of input dimension from 49 to 35
I decided to use the TensorFlow library to automatically optimise the hyper parameters on this simplified dataset, rather than continuing to guess at parameters myself.resulting in a best score of 72.6% - barely any better than any of the other models I had already trained.

Due to this lack of improvement, I had tried increasing the number of neurons per layer from 30 to 100; I saw no further improvement.
I decided to see what would happen if I used tensorflow just on the original set of data; it still performed no better.
There was one last modification to the data set I tried: keeping in the ‘NAME’ column rather than removing it at the start (optimise_tensorflow_keep_NAME.ipynb). With this as the only filter different from the default, the NN went on to achieve a satisfying 80% accuracy. However, I find it deeply unethical to include such an identifier as training data, as the model is effectively just learning to stereotype the data based on keywords in the name of the applicant. This reminds me of experiments measuring racial discrimination in the labor market such as “Are Emily and Greg More Employable than Lakisha and Jamal? A Field Experiment on Labor Market Discrimination” by Marianne Bertrand & Sendhil Mullainathan (https://www.nber.org/papers/w9873).
Summary: 
Overall, I’m happy that I was close to the required value, which is still significantly better than random, but perhaps the data are just too noisy to be perfectly predictable.
This is understandable, as the realistic dataset I am working from is a summary of available categories, but these categories are never going to be the whole story - individual circumstances of each applicant, in different parts of the country, with different personal qualities, are also going to have a large effect on whether a grant was used effectively. Also it is worth considering that the data are marked up as a binary of ‘successful’ or ‘not’ - when surely in reality this should be a more continuous variable that would suit a regression problem.
Due to the data having an identifiable target column, we could try a Supervised Learning model such as scikitlearn on this dataset. These could be Logistic Regression and Random Forest methods. I would imagine Logistic Regression might not cope with the number of variables in the data, whereas Random Forest might work better due to its stochastic approach.

