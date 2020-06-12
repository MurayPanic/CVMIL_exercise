# Chapter 7 Modeling Complex data densities.

## Q: What is this chapter about?

**A:** This chapter it is about modeling complex data densities using hidden variable. 

Usually, we choose a distribution to model the data according to the feature of the data. For example, we choose Bernoli distribution for discrete binary data and choosecategorical distribution for discrete multivalue data. In many situation, we choose normal distribution to model some unknown continuous data. But that does not enough. It is unlikely that all the data follow normal distribution. Some data may have multi-modal and other different characteristics.

To solve model complex data densities, we can use hidden variable. The introduction of hidden variable is based on the views that a complex data density is a maginal distribution of a joint distribution between the data and hidden variables. For my understanding, the complex data density is the result of combining a series of distributions weighted with the hidden variable. 


## Q: What is the benefits of hidden variables?

**A:** Compare to modeling data using a single normal distribution, introducing the hidden variables have the following benefits:

- Able to model multi-modal data;
- Alleviate the influence of outliers in the data;
- Easier parameters estimation.

## Q: How to indroduce a hidden variables in a model?

**A:** Consider that we have a variable $x$, and we would like to model its distribution with $Pr(x|\theta)$ where $\theta$ is the parameters we need to estimate . The we introduce a hidden variable $h$ and view $Pr(x|\theta)$ as a marginalization of the joint distribution $Pr(x,h|\theta)$. That is:

$$
Pr(x|\theta)= \int Pr(x,h|\theta)dh
$$

## Q: How to estimate a model with hidden variables? What is its difference from the normal one?

**A:** To estimate the probablistic model, we can still use the maximum likehood method. Whe have two choices to make MLE of our parameters: we can either not include hidden variables into our target function:

$$
\hat \theta = \arg \max_{\theta}= \left[\sum^{I}_{i=1}log[Pr(x_i|\theta)]\right]
$$

or not include hidden variables into our target function:

$$
\hat \theta = \arg\max_{\theta}= \left[\sum^{I}_{i=1}\log[\int Pr(x_i,h_i|\theta)dh_i]\right]
$$

In the first form, if we solve it by calculating the derivatives and equate them to zero, we are not likely to solve $\theta$ in closed form. But in the second form, the introduction of the hidden variables enable us to us the Expectation-Maximization (EM) algorithm to estimate the parameters $\theta$.

## Q: What is the EM algorithm?

**A:** EM algorithm is a techinique for optimization. It is a general purpose tool for fitting parameters in model with specific forms. Its main idea is to calculate the lower bound of the target function,then maximize the lower bound function iteratively and then force the lower bound function to approximate the maxima of the target function.

For a target function with a form:
$$
\hat \theta = \arg\max_{\theta}= \left[\sum^{I}_{i=1}\log[\int Pr(x_i,h_i|\theta)dh_i]\right]
$$

We chose its lower bound function as
$$
\mathcal{B}{{Pr(h_i)},\theta}= \sum^{I}_{i=1}\int Pr(h_i) \left[\frac{Pr(x_i,h_i|\theta)}{Pr(h_i)}\right]dh_i\\
\sum^I_{i=1}log\left[\intPr(x_i,h_i|\theta)dh_i\right]
$$

The above inequality is achieved by Jensen inequality. The lower bound of the target function is a function of the parameters $\theta$ and a set of $I$ distributions of hidden variables $Pr(h_i)$. The EM algorithm manipulates the ditribution $Pr(h_i)$ and parameters $\theta$ alternatively to increase the lower bound.

At the begginning, the parameters and the disributions are set randomly. Then, the algorithm execute the E- and M- steps alternatively:

+ In the expectation steps, it calculate the distribution $Pr(h_i)$ to improve the lower bound. The $Pr(h_i)$ equals to the posterior probability of $Pr(h_i|x,\theta)$. This calculation can be done using Bayes rule.

+ In the maximization step, it calculate the parmeter $\theta$ to improve the lower bound. In this step, the $Pr(h_i)$ calculated in the previous E-step is subsitituted into the target function. The $Pr(h_i)$ is treated as known value.The $\theta$ that maximize the target function can be obtained by clacluting the derivatives, equating them to zero and solving the equation
 
## Q: By introducing hidden variables, what models will we get?
**A:** It depends on the type of distributions we used to model the hidden variables. Remember that, the original probalistic model can be considered as the maginalizationof a joint distribution between the data $x$ and hidden variable $h$. Thus the original probalistic model can be written as a form of multiplication between the conditional distribution of data $x$ and the distribution of the hidden variable $h$. Therefore, the distribution of the hidden variable $h$ affects the final form of the original probalistic model.

In this chapter, we model the conditional distribution $Pr(x|h)$ with normal distribution and choose different type of distribution for describing hidden varible $h$ to three types of model:

+ Mixture of gaussian: Using a categorical distribution to describe hidden variables. The final form of the model is still Gaussian.
+ Robust model: Using a Gamma distribution to describe hidden variables. The final form of the model follows t-distribution.
+ Subspace model: Using a standard normal distribution to describe hidden variables. Note that the hidden variables and factors are involved into the mean of condictional distribution $Pr(x|h)$. It is a little bit different from the previous two model. The final form of the model is still Gaussian.


## Q: What is the mixture of Gaussian model?

**A:** Mixture of Gaussian model is essentially a weighted some of $K$ normal distribution with different means and variance. It is able to model data of multi modal.

## Q: What is the robust model?
**A:** The robust model is also called the t-distribution model. Because it is using a t-distribution to model the $Pr(x|\theta)$. It is essentially an infinite weitghted sum of normal distribution with the ame means but different variance. 

It is called robust model because it is not sensitive to outliers in the data. This charateristic is attributed to the t-distribution. Because t-distribution has a longer tail compare to normal distribution.

## Q: What is the subspace model?
**A:** The subspace model is also called the factor analyzer. It is a product of factor analysis. It uses a series of factors to describe the variance in the data. The matrix of factors are actually construct a subspace of the full variance. Thus, it is called subspace model. Factor analysis is a technique to handle high dimensions.

## Q: Can I combine these models together?
**A:** We can combine these models and generate models with different combined feature. Although we characterized these three models with their distribution of the hidden variables. In fact, these three models represent three practices to model complex data densities:

+ Mixture models: Aims to model data that have multi-modal distribution. It views the complex data densities as finite sum of a series simple distributions.

+ t-ditribution models: Reduce model sensitivities to outliers. It use t-distribution to replave normal distribution.

+ sub-space models: Using factor analysis to reduce the dimension of variance.

Thus when we create our own models, we can choice these three actions according to our needs.


