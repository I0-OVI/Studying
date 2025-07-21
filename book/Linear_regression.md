### Linear regression
> one sentence to conclude: a fit line which minimizes the sum of difference between predicted values and actual values.

There is a classic case when we talk about this method: for a house with certain square, how much we would pay for it.
![image](./picture/linear_regression.png){width=80%}
Linear regression creates a line with equation y=mx+c. To find the best fitting line, we would use a ruler in reality but how would the machine do this. First, we should know about the loss function.
The function calculates the difference between the $\hat{y}$ and y.(the predicted value is denoted as $\hat{y}$)
$$Loss= \frac{1}{N}* \sum_{i=1}^{N} {(\hat{y}-y)^2}$$