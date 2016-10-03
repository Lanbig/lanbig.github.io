---
layout: post
title: Linear Regression and Cost function
image: "/images/posts/feature_images/linear-cost-function.png"
author: ben
---

In this part of this exercise, I will implement linear regression with one variable to predict profits for a food truck. 
Matlab is the tools I used for this project. Link to ![Github](https://github.com/Lanbig/Machine-Learning/tree/master/machine-learning-ex1)

The first figure shows the scatter plot of profit and population in the city. 
![Scatter plot and linear line](/images/posts/content_images/linear-fit.png)
The scatter plot of the training data reveals the positive relationship between profit and population. Also, the blue line is the regression line.

```matlab
function plotData(x, y)
%PLOTDATA Plots the data points x and y into a new figure 
%   PLOTDATA(x,y) plots the data points and gives the figure axes labels of
%   population and profit.

% ====================== YOUR CODE HERE ======================

plot(x, y, 'rx', 'MarkerSize', 10); % Plot the data 
ylabel('Profit in $10,000s'); % Set the y?axis label 
xlabel('Population of City in 10,000s'); % Set the x?axis label

figure; % open a new figure window
% ============================================================

end
```

<br />
In this part, I will fit the linear regression parameters θ to our dataset using gradient descent. 
```
x^y
```
For population = 35,000, we predict a profit of 4519.767868
For population = 70,000, we predict a profit of 45342.450129

Visualizing J(theta_0, theta_1)

Surface plot

Contour plot
![Surface plot](/images/posts/content_images/linear-cost-function.png)
![Contour plot](/images/posts/content_images/linear-cost-function2.png)
