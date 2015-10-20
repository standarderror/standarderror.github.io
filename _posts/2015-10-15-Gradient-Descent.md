---
title: Gradient descent
updated: 2015-10-15
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

<span>$x^2$</span>


Gradient descent is a method of searching for model parameters which result in the best fitting model. There are many, many search algorithms, but this one is quick and efficient for many sorts of supervised learning models.

Let's take the simplest of simple regression models for predicting <span>$y$</span>: <span>$\hat{y} = \alpha + \beta x$</span>. Our model has two parameters - $\alpha$, the intecept, and $\beta$, the coefficient for $x$ - and we want to find the values which produce the best fitting regression line.

How do we know whether one model fits better than another? We calculate the _error_ or _cost_ of the predictions. For regression models, the most common cost function is mean squared error:

$$ Cost = J(\alpha , \beta) = \frac{1}{2n} \sum{(\hat{y} - y)^2} = \frac{1}{n} \sum{(\alpha + \beta x - y)^2}$$

The cost function quantifies the error for any given $\alpha$ and $\beta$. Since it is squared, it will take the form of an inverted parabola.

At the bottom (or _minima_) or the parabola is the lowest possible cost. We want to find the $\alpha$ and $\beta$ which correspond to that minima.

The idea of _gradient descent_ is to take small steps down the surface of the parabola until we reach the bottom. The _gradient_ of _gradient descent_ is the gradient of the cost function, which we find by taking the partial derivative of the cost function with respect to the model coefficients. Steps:

1. For the current value of $\alpha$ and $\beta$, calculate the cost and the gradient of the cost function at that point
2. Update the values of $\alpha$ and $\beta$ by taking a small step towards the minima. This means, subtracting some fraction of the gradient from them. The formulae are:

$$ \alpha = \alpha - \gamma \frac{\partial J(\alpha, \beta)}{\partial \alpha} = \alpha - \gamma \frac{1}{n} \sum{2(\alpha + \beta x - y)}$$
$$ \beta = \beta - \gamma \frac{\partial J(\alpha, \beta)}{\partial \beta} = \beta - \gamma \frac{1}{n} \sum{2x(\alpha + \beta x - y)}$$

Steps 1 & 2 are repeated until one is satisfied that the model is not getting any better (cost isn't decreasing much with each step).

The learning rate parameter $\gamma$ controls the _size of the steps_. This is a parameter which we must choose. There is some art in choosing a good learning rate, for if it is too large or too small then it can prevent the algorithm from finding the minima.
