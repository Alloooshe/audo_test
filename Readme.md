# solution 
We implement very simple solution using KNeighborsClassifier.

## First, we discuss the data : 
- O*NET includes alot of information, but actually in our user response we only have 12 attributes related to intrests and work values, so that is the data we use for now.
- Intrests and work values files include data for most occupations, they also includes 6 values represnting the top 3 intrest/work value for each occupation.
- In our user input we have 12 values but we can also calculate the 6 more features by getting top 3 intrest/work value according to the user's scores.
- With the (intrests,work values) data we can build a simple KNN classifier that would match user's input to an occupation title
- note user's data scales are different from scales used in O*NET, this is probably because user data was collected using Holland codes and test. however, it is not trivial how this convertion was performed.  
## The O*NET contains much more information, I will discuss this later on

## How it works
- we fit KNN classifier, the reasons for choosing it are : 1- simplicty and speed if deployed in production 2- it is a modeling algorithm hence no need to much more data for training, 3- since we have a large number of occupations it does not make too much sense to train a classifier.
- we do not split the data into train/test since we are modeling the data and not really training anything, the testing ofcourse can be done be looking at KNN performance when used with user data.
- we use standard normalization for the input data 
- during inference we calculate 6 additional features for the user, which represents the top categories of interest/work value according to the user's input.

## How to imporve 
- First obvious problem is the mismatch between user's data and modeling data, it is better to use original data without converting to simple interests/work values or find better conversion.  
- we can use RIASEC data to increase the number of features we use for modeling, for example, since the occupations has detailed description and RIASEC has mapping between interests and different majors and careers, then we can build the KNN around textual features using sentence2vec. 

 
search query: interests scores + work values scores + RISACE careers embeddings(sentence2vec) 

documents to search : title (sentence2vec) + occupation description  (senctece2vec)

KNN to best match queries and documents,here we can optimize the vectors for queries and documents during training using the labeled data(which i hope you have).  


the workflow will look like this 
1. user -> interests/work values -> RISACE careers (as text embedding) + interests + work values ->query embedding. 
2. match queries with precalculated documents embeddings.
3. get top 10, optimize this result.