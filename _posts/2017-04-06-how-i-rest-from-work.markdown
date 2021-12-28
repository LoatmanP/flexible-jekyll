---
layout: post
title: Predicting your Time Preferences with Streamlit
date: 2021-12-28 13:32:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: discountrate.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Delay Discounting, Behavioral Economics, Intertemporal Choice, Economic Patience]
---
# Delay Discounting Rate Estimator for Time Preferences

Intertemporal choice behavior has been shown to be associated with several indicators of health and wealth. Individuals who are less patient (i.e., choosing rewards that are of lesser value but available sooner over rewards of more value but are available later) tend to have worse health and wealth outcomes compared to their more patient counterparts.

One way to to assess patience is to give individuals a series of choices that involve rerwards that are distributed through time and then use choice-based modeling to estimate their intertemporal preferences. 

This web app is designed to estimate discounting rates from choices using an Exponential or Hyperbolic Discounting Model to determine your intertemporal preferences and level of patience.

Please select the model you wish to evaluate your intertemporal preferences, input your choices, and then click "Submit Your Choices" to determine how patient you are!

(see below for the python code I used to build the discounting models)

![I and My friends]({{site.baseurl}}/assets/img/we-in-rest.jpg)

<div class="iframe-container iframe-container-for-wxh-500x350"
style="-webkit-overflow-scrolling: touch; overflow: auto;">

<iframe src="https://share.streamlit.io/loatmanp/discountwebapp/main/main.py">

  <p style="font-size: 110%;"><em><strong>IFRAME:</strong> There is
  iframe content here but your browser version does not support
  iframes.</em> Please update your browser to its current version 
  and try again.</p>

</iframe>

</div>

## Building the Exponential Discounting Classifier

### Import the required Python libraries

```python
import scipy.optimize
import pandas as pd
import numpy as np
```

### Initialize the parameters

<span style="color:black"><b>discountRate</b> = The estimated daily discount rate of the decision-maker </span>

<span style="color:black"><b>rho</b> = determinism parameter (all other variance other than discounting) </span>
 


```python
class ExponentialClassifier(object):
   def __init__(self, discountRate=-5, rho=.01):
   self.discountRate = discountRate
   self.rho = rho
```

### Build a method to fit the parmaters

<span style="color:black"><b>ranges</b> specifies the parameter grid ranges for the grid search </span>

<span style="color:black"><b>scipy.brute</b> is used to optimize the parameters (fmin is used a local grid search and is performed at the end of the more coarse brute search) </span>

<span style="color:black"><b>X</b> features of the choice items (smaller, sooner reward; larger, later rewird; smaller, sooner delay; larger, later delay </span>

<span style="color:black"><b>y</b> decision-maker choices (0,1)  </span>

<span style="color:black"><b>sse</b> sum of squared error (to be calculated in another method) </span>****

```python
def fit(self, X, y):
   ranges = [[-8, .01, .1], [0, 1.1, .1]]
   opt = scipy.optimize.brute(self.sse, ranges, args=(X, y,), finish=scipy.optimize.fmin)
   self.discountRate = opt[0]
   self.rho = opt[1]
   return self
```
### Calculate the sum of squared error (SSE) to feed the fit method

<span style="color:black"><b>params</b> params[0] delay discount rate, params[1] rho  </span>

<span style="color:black"><b>cost</b> cost function </span>

<span style="color:black"><b>sse</b> sum of squared error </span>

<span style="color:black"><b>yhat</b> the predicted value of y </span>

```python
 def sse(self, params, X, y):
     if (params[0] < -7.3) or (params[0] > -.01) or (params[1] <= 0.):
         return 100000000000000000000000 #return something absurd
     yhat = self.choice(X, params)
     boundary = .00000001
     yhat_clip = np.clip(yhat, boundary, 1 - boundary)
     cost = -y * np.log(yhat_clip) - (1 - y) * np.log(1 - yhat_clip)
     sse = np.sum(np.power(cost, 2))
     return sse
```

### Evaluate decision-maker choices using the Exponential Discounting Model

<span style="color:black"><b>ssVal</b> present (discounted) value of the smaller, sooner choice item (all smaller, sooner choice items are available today, so are not discounted  </span>

<span style="color:black"><b>llVal</b> the present (discounted) value of the larger, more delayed choice item </span>

<span style="color:black"><b>pLL</b> the probability of selecting the larger, more delayed choice item using a logistic scaling function </span>

```python
 def choice(self, X, params=[]):
       # if params are NOT provided, use internally stored values
       if len(params) == 0:
           rho = self.rho
           discRate = np.exp(self.discountRate)
       # if params ARE provided, use them
       else:
           discRate = np.exp(params[0])
           rho = params[1]
       ssVal = X[:,1]
       # ssVal = (X[:, 1] * np.exp((-discRate * X[:, 2])))
       llVal = (X[:, 3] * np.exp((-discRate * X[:, 4])))

       pLL = 1.0 / (1.0 + np.exp(-rho * (llVal - ssVal)))

        return pLL
  ```
### Get the model's predicted probabilities for model performance evaluation

<span style="color:black"><b>predict_proba</b> method to get the predicted probabilities of the model to evaluate model performance (same method as scikit-learn's predict_proba method) </span>

```python
def predict_proba(self, X, params=[]):
    return self.choice(X, params)
```

### Build a method for absolute predictions (0,1)

<span style="color:black"> same method as scikit-learn's predict method</span>

```python
def predict(self, X, params=[]):
    return self.choice(X, params).round()
 ```
### Build a method for setting the parameters
 
```python
def set_params(self, discountRate=-5, rho=.01):
   self.discountRate = discountRate
   self.rho = rho
   return self
   ```
   
### Build a method for getting the estimated parameters
```python
def get_params(self, deep=True):
   return {'discountRate': self.discountRate, 'rho': self.rho}
```

