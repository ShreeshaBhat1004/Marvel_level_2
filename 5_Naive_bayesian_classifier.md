## Naive Bayesian Classifier:
- It is a supervised learning algorithm
- It classifies the data point based on independent probabilities of a data point without considering
  influence of other data points.
- We need to implement a text classifier using naive bayesian classifier.

### Understanding Naive bayesian classifier
**We can Understand this algorithm with an example:**
Lets say we have a dataset of 12 emails in which 8 are normal messages and 4 are spam messages. The count for some of the words in normal and spam are:
> Normal:\
> Friend - 8\
> Dear - 5\
> Lunch - 3\
> Money - 1
>
> Spam:\
> Friend - 1\
> Dear - 2\
> Lunch - 0\
> Money - 4

Then we calculate the probability of seeeing each word, given that we saw the word in either a normal message or spam.\
> Normal: \
> p(Dear|N)=0.47\
>p(Friend|N)=0.29\
>p(Lunch|N)=0.18\
P(Money|N)=0.06
> 
> Spam: \
>p(Dear|S)=0.29\
>p(Friend | S) = 0.14\
>p(Lunch | S)=0.00\
>p(Money|S)=0.57

Then we make an initial guess about the probability of seeing a normal message
> p(N) = 0.67\
> p(S) = 0.33

Now we are ready to classify if a message is spam or normal considering that our message is "*Dear Friend*". For that,\
> a = p(N)*p(Dear|N)*p(Friend|N) = 0.09\
> b = p(S)*p(Dear|S)*p(Friend|N) = 0.01

We can see that
$a > b$\
Hence, the message "*Dear friend*" is most probably a normal message. This is how **Naive Bayesian classifier** works. 
```
       ┌───────────────────────────────────────────────────────────────────────────────────────────────────┐
        │                                 Naive Bayes Classifier Example                                   │
        └────────────────────────────────────────────────┬────────────────────────────────────────────────┘
                                                        │
                                                        │
                                                        ▼
                                          ┌────────────────────────────┐
                                          │        Dataset: Emails     │
                                          │   12 emails (8 normal, 4 spam)   │
                                          └────────────────────────────┘
                                                        │
                                                        │
                                                        ▼
                                          ┌────────────────────────────┐
                                          │      Word Frequencies      │
                                          │  Normal:                   │
                                          │    Friend - 8, Dear - 5,   │
                                          │    Lunch - 3, Money - 1    │
                                          │  Spam:                     │
                                          │    Friend - 1, Dear - 2,   │
                                          │    Lunch - 0, Money - 4    │
                                          └────────────────────────────┘
                                                        │
                                                        │
                                                        ▼
                                          ┌────────────────────────────┐
                                          │  Conditional Probabilities │
                                          │  Normal:                   │
                                          │    p(Dear|N) = 0.47        │
                                          │    p(Friend|N) = 0.29      │
                                          │    p(Lunch|N) = 0.18       │
                                          │    p(Money|N) = 0.06       │
                                          │  Spam:                     │
                                          │    p(Dear|S) = 0.29        │
                                          │    p(Friend|S) = 0.14      │
                                          │    p(Lunch|S) = 0.00       │
                                          │    p(Money|S) = 0.57       │
                                          └────────────────────────────┘
                                                        │
                                                        │
                                                        ▼
                                          ┌────────────────────────────┐
                                          │        Class Priors        │
                                          │    p(N) = 0.67             │
                                          │    p(S) = 0.33             │
                                          └────────────────────────────┘
                                                        │
                                                        │
                                                        ▼
                                          ┌────────────────────────────┐
                                          │     Message: "Dear Friend" │
                                          │  a = p(N) * p(Dear|N) * p(Friend|N) = 0.09 │
                                          │  b = p(S) * p(Dear|S) * p(Friend|S) = 0.01 │
                                          └────────────────────────────┘
                                                        │
                                                        │
                                                        ▼
                                               ┌───────────────┐
                                               │    a > b      │
                                               │ "Dear Friend" │
                                               │  is a normal  │
                                               │    message    │
                                               └───────────────┘
```
### Code Implementation
```python
import pandas as pd
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score, confusion_matrix

# Load the dataset directly from the Kaggle link
data = pd.read_csv('/content/emails.csv') 

# Designate features and labels
features = data['text']
labels = data['spam']

# Create a CountVectorizer to convert emails into a bag-of-words representation
vectorizer = CountVectorizer()
X = vectorizer.fit_transform(features)

# Split into training and testing sets (80/20 split)
X_train, X_test, y_train, y_test = train_test_split(X, labels, test_size=0.2, random_state=42) 

# Create a Multinomial Naive Bayes classifier
model = MultinomialNB()

# Fit the model to the training data
model.fit(X_train, y_train)

# Make predictions on the test data
predictions = model.predict(X_test)

# Evaluate performance
print("Accuracy:", accuracy_score(y_test, predictions))
print("Confusion Matrix:\n", confusion_matrix(y_test, predictions))
```
![image](https://github.com/ShreeshaBhat1004/Marvel_level_2/assets/111550331/c0bfc661-0bb7-4571-89d1-757d16f5c7c5)
