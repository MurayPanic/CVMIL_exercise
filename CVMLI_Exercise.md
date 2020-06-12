
# Chapter 6

## Problem 6.1 
Consider the following problems.
i Determining the gender of an image of a face.  
ii Determining the pose of the human body given an image of the body.  
iii Determining which suit a playing card belongs to based on an image of that card.  
iv Determining whether two images of faces match (face veriﬁcation).  
v Determining the 3D position of a point given the positions to which it projects in two cameras at diﬀerent positions in the world (stereo reconstruction).  
For each case, try to describe the contents of the world state w and the data x. 
Is each discrete or continuous? If discrete, then how many values can it take? Which are regression problems and which are classiﬁcation problems? 

**Answers:**
1. Determining the gender of an image of a face

	The world state is the gender. It is discrete.  
	The data state is the image of a face. It is discrete.  
	This is a classification problems.
   
2. Determining the pose of the human body given an image of the body.

	The world state is the pose of the human body. It is continuous.  
	The data state is an image of the body. It is continuous.
	This is a regression problems.

3. Determining which suit a playing card belongs to based on an image of 
that card.

	The world state is the suit that a playing card belongs to. It is discrete.  
	The data state is the image of that card. It is also discrete.  
	This is also a classification problem

4. Determining whether two images of faces match (face veriﬁcation).

	The world state is a binary value, either two faces match (True) or
not (False).  
	The data state is the images of faces. It is discrete.  
	This is a classification problem.

5. Determining the 3D position of a point given the positions to 
which it projects in two cameras at diﬀerent positions in 
the world (stereo reconstruction).

	The world state is 3D position of a point in a word. 
It is continuous.  
	The data state is the projection in two cameras at different
positions in the world. It is a continuous problem.
	This is a regression problem.


## Problem 6.2

Describe a classiﬁer that relates univariate discrete data $x \in {1 \cdots K} $ to 
a univariate discrete world state $w \in {1 \cdots M}$ for both discriminative and generative model types.



**Answers**



*Discriminative  model:*

We model the world state $w$ according to the discrete data $x$, that means we are going to calculate $Pr(w|x)$.

Because the world state is discrete and is multivalued. We use a categorical distribution to model the world state. That is $Pr(w)= Cat_w[\lambda]$, where  $\lambda = \{\lambda_1, \lambda_2 \cdots \lambda_M\}$ and $\lambda_w \in [0,1]$ and $\sum_{w=1}^{M} \lambda_w = 1$. The $\lambda$ is the parameter vector. We make $\lambda$ a function of the data $x$.  It could be a function in any form. Here we represent this function as  $\lambda = \lambda(x)$.

So we have :
$$
Pr(w|x,\lambda) = Cat_w[\lambda(x)]
$$
where $\lambda = \{ \lambda_1, \lambda_2 \cdots \lambda_M \}$ are the unknown parameters of the model.



*Generative model*:

We model the contingency of data according to the world state. That means we are going to calculate $Pr(x|w)$.

Because data is univariate and discrete. We choose the categorical distribution to model the data $x$ and make the parameters of the distribution to be a function of the world state $w$. That is:
$$
Pr(x|w,\lambda) = Cat_x[\lambda(w)]
$$
where $\lambda =\{\lambda_1, \lambda_2 \cdots \lambda_K\}$ are the unknown parameters of the model and it could be expressed as a function of the world states: $\lambda = \lambda(w)$.



We also need a prior $Pr(w)$ over the world states. To make it easy for inference and ensure that the posterior and the world states have the same form after calculation, we choose the Dirchlet distribution to model the world state:
$$
Pr(w)= Dir_w(\alpha_w)
$$
where $\alpha_w$ is the prior probability of the observing the state $w$.

The inference algorithm takes new datum take $\hat x$  and returns the posterior distribution on $Pr(w|x,\lambda)$ over the world state $w$ using Bayes rule:
$$
Pr(w|x)=\frac{Pr(w)Pr(x|w)}{Pr(x)} = \frac{Pr(w)Pr(x|w)}{\sum_{w=1}^{M}Pr(x|w)Pr(w)}
$$



## Problem 6.3

Describe a regression model that relates univariate binary discrete data $x \in \{0,1\}$  to a univariate continuous world state $w \in [-\infty, \infty]$. Use a generative formulation in which $Pr(x|w)$  and $Pr(w)$  are modeled. 



**Answers:**

We use a generative formation for this regression model. Because the data is univariate and binary , we use a Bernoulli distribution to model the data state $x$ and make its parameter a function of the world state $w$. That is:
$$
Pr(x|w)= Bern_x[\lambda(w)]
$$
where $\lambda$ is the parameters of the Bernoulli distribution and it is a function of the world state $w$. Because the $\lambda$ should be ranged between $[0,1]$ , thus we define the function of the world state $w$ as:
$$
\lambda(w)= sig(w)
$$
where $sig[\cdot]$ is the sigmoidal function.

We also need a prior distribution to to describe the world state $w$. Because the world state is univariate and continuous,  we use the Normal distribution to describe the world state $w$. That is :
$$
Pr(w)= Norm_x[\mu, \sigma^2]
$$
where $\mu$ and $\sigma$ are the parameters of the normal distribution



The inference algorithm takes the new datum $\hat x$ and returns the posterior distribution $Pr(w|x)$ over the world state $w$ using Bayes' rule:
$$
Pr(w|\hat x)=\frac{Pr(\hat x|w)Pr(w)}{\sum_{k=0}^{1}Pr(\hat x|w=k)Pr(w=k)}
$$


## Problem 6.4

Describe a discriminative regression model that relates a continuous world state $w\in [0,1]$ to univariate continuous data $x \in [-\infty, \infty]$. Hint: Base your classiﬁer on the Beta distribution. Ensure that the constraints on the parameters are obeyed.



**Answers:**

We are going to develop a discriminative regression model. That means we are going to modeling the contingency of the world according to the data. Because the world state $w$ is continuous and bounded between $[0,1]$, we use the Beta distribution to model the world and make its parameters a function of the data $x$. That is:
$$
Pr(w|x)=Beta_w(\alpha(x), \beta(x))
$$
where $\alpha$ and $\beta$ are parameters of the distributions. Because the parameters of the Beta distribution should be range between $[0, \infty]$. Thus, we define the function of the data $x$ as:
$$
\alpha(x)= |x|, \beta(x)=|x|
$$



## Problem 6.5

Find expressions for the maximum likelihood estimates of the parameters in the discriminative linear regression model (section 6.3.1). In other words ﬁnd the parameters ${\phi_0, \phi_1, \sigma^2}$ that satisfy


$$
\hat\phi_0, \hat\phi_1, \hat\sigma^2 = \mathop{\arg\max}_{\phi_0, \phi_1, \sigma^2} \left[\prod_{i=1}^I Pr(w_i|x_i, \phi_0, \phi_1, \sigma^2)\right]\\
=\mathop{\arg\max}_{\phi_0, \phi_1, \sigma^2} \left[\sum_{i=1}^I \log[ Pr(w_i|x_i, \phi_0, \phi_1, \sigma^2)]\right]\\
=\mathop{\arg\max}_{\phi_0, \phi_1, \sigma^2} \left[\sum_{i=1}^I \log[ Norm_w[\phi_0+\phi_1x, \sigma^2]]\right]\\
$$
where $\{w_i, x_i\}^I_i=1$ are paired training examples.



**Answer:**

To find out the parameters $\{\phi_0, \phi_1, \sigma^2\}$ that maximize the above equation, we need to calculate the derivatives regarding to each parameter respectively and equal the derivatives to $0$. Let $L$ be $\sum_{i=1}^I \log[ Norm_w[ \phi_0+\phi_1x, \sigma^2]]$ .



First simplify the $L$ for easy calculation:
$$
\begin{align}
L&=\sum_{i=1}^I \log[ Norm_w[ \phi_0+\phi_1x, \sigma^2] ]\\
&=\sum_{i=1}^I \log[\frac{1}{\sqrt{2\pi\sigma^2}}\exp\left[-0.5 \frac{(w_i-(\phi_0+\phi_1x_i))^2}{\sigma^2}\right]]\\
&=\sum_{i=1}^I \left\{\log(\frac{1}{\sqrt{2\pi\sigma^2}})-0.5 \frac{(w_i-(\phi_0+\phi_1x_i))^2}{\sigma^2}      \right\}\\
&=I\log\left(\frac{1}{\sqrt{2\pi\sigma^2}}\right) - 0.5\sum^{I}_{i=1}\frac{(w_i-(\phi_0+\phi_1 x_i))^2}{\sigma^2}\\
&=-I\log(\sqrt{2\pi}\sigma)- 0.5\sum^{I}_{i=1}\frac{(w_i-(\phi_0+\phi_1 x_i))^2}{\sigma^2}\\
&=-I\log(\sqrt{2\pi})-I\log(\sigma)- 0.5\sum^{I}_{i=1}\frac{(w_i-(\phi_0+\phi_1 x_i))^2}{\sigma^2}\\
&=-\frac{I}{2}\log(2\pi)-\frac{I}{2}\log(\sigma^2)-\sum^{I}_{i=1}\frac{(w_i-(\phi_0+\phi_1 x_i))^2}{2\sigma^2}\\
\end{align}
$$


Calculate the derivative of $L$ regards to $\phi_0$ :
$$
\begin{align}\frac{\partial L}{\partial\phi_0} &=\left(-0.5\sum_{i=1}^{I}\frac{(w_i-(\phi_0+\phi_1 x_i))^2}{\sigma^2} \right)'\\
&=\sum_{i=1}^{I}\frac{w_i-(\phi_0+\phi_1x_i)}{\sigma^2}
\end{align}
$$
Equal $\frac{\partial L}{\partial \phi_0}$ to 0  we have:
$$
\frac{\partial L}{\partial \phi_0}=0\\
\sum_{i=1}^{I}\frac{w_i-(\phi_0+\phi_1x_i)}{\sigma^2}=0\\
\phi_0=\frac{\sum_{i=1}^{I}(w_i-\phi_1x_i)}{I}
$$

Notice that $\frac{\sum^{I}_{i=1}w_i}{I}$ and $\frac{\sum^{I}_{i=1}x_i}{I}$  are essentially the average of $w_i$ and $x_i$ respectively, the $\phi_0$ can be rewritten as:
$$
\phi_0= \mu_w-\phi_1\mu_x
$$
where $\mu_w=\frac{\sum^{I}_{i=1}w_i}{I}$ and $\mu_x = \frac{\sum^{I}_{i=1}x_i}{I}$.

Calculate the derivative of $L$ regards to $\phi_1$:
$$
\begin{align}
\frac{\partial L}{\partial \phi_1}&=\left(-0.5\sum_{i=1}^{I}\frac{(w_i-(\phi_0+\phi_1 x_i))^2}{\sigma^2} \right)'\\
&=\sum_{i=1}^{I}\frac{w_i-(\phi_0+\phi_1x_i)}{\sigma^2} \times x_i
\end{align}
$$

Equal $\frac{\partial L}{\partial \phi_1}$ to 0 and solve for $\phi_1$:
$$
\frac{\partial L}{\partial \phi_1}=0 \\
\sum_{i=1}^{I}\frac{w_i-(\phi_0+\phi_1x_i)}{\sigma^2} \times x_i=0\\
\sum_{i=1}^{I}(w_i -(\phi_0+\phi_1x_i))\times x_i=0\\
\sum_{i=1}^{I}(w_i-(\mu_w-\phi_1\mu_x)-\phi_1x_i)x_i=0\\
\phi_1=\frac{\mu_w\sum_{i=1}^{I}x_i -  \sum_{i=1}^{I}w_ix_i}{\sum_{i=0}^{I}(\mu_x-x_i)x_i}
$$
Calculate the derivative of $L$ regards to $\sigma^2$:
$$
\frac{\partial L}{\partial \sigma^2}=\left(-\frac{I}{2}\log(\sigma^2)-\sum_{i=1}^{I}\frac{(w_i-(\phi_0+\phi_1x_i))^2}{}\right)'\\
=-\frac{I}{2\sigma^2}+\sum_{i=0}^{I}\frac{(w_i-(\phi_0+\phi_1x_i))^2}{2(\sigma^2)^2}
$$
Equal$\frac{\partial L}{\partial \sigma^2}$ to 0 and solve for $\sigma^2$, we have:
$$
\frac{\partial L}{\partial \sigma^2} = 0 \\
-\frac{I}{2\sigma^2}+\sum_{i=0}^{I}\frac{(w_i-(\phi_0+\phi_1x_i))^2}{2(\sigma^2)^2} = 0\\
-I\sigma^2 + \sum_{i=1}^I(w_i-(\phi_0+\phi_1x_i))^2=0\\
\sigma^2=\frac{\sum_{i=1}^I(w_i-(\phi_0+\phi_1x_i))^2
}{I}
$$


Collect all the results together, we have:
$$
\phi_0= \mu_w-\phi_1\mu_x\\
\phi_1=\frac{\mu_w\sum_{i=1}^{I}x_i -  \sum_{i=1}^{I}w_ix_i}{\sum_{i=0}^{I}(\mu_x-x_i)x_i}\\
\sigma^2=\frac{\sum_{i=1}^I(w_i-(\phi_0+\phi_1x_i))^2
}{I}
$$


## Problem 6.6

 Consider a regression model that models the joint probability $Pr(x,w)$ between the world $w$ and the data $x$ as 
$$
Pr\left(\begin{bmatrix}w_i \\ xi\end{bmatrix}\right)= Norm_{[w_i,x_i]^T}\left[
\begin{bmatrix}\mu_w \\ \mu_x\end{bmatrix}, 
\begin{bmatrix}\sigma^2_{ww}  & \sigma^2_{xw} \\ \sigma^2_{xw} & \sigma^2_{xx} \end{bmatrix}
\right].
$$
Use the relation in section 5.5 to compute the posterior distribution $Pr(w_i|x_i)$. Show that it has the form 
$$
Pr(w_i|x_i)= Norm_{w_i}[\phi_0+\phi_1x, \sigma^2],
$$


and compute expressions for $\phi_0$ and $\phi_1$ in terms of the training data $\{w_i,x_i\}^{I}_{i=1}$ by substituting in explicit maximum likelihood estimates of the parameters $\{\mu_w, \mu_x, \sigma^2_{ww}, \sigma^2_{xw}, \sigma^2_{xx} \}$.



**Answers: **

The relation in Section 5.5 is essentially calculating the conditional distributions of a normal distribution.  That is the results of the Problem 5.5. We applied it to the problem here and we can write out the posterior function $Pr(w_i|x_i)$.
$$
\begin{align}Pr(w_i|x_i) &= Norm_{w_i}\left[\mu_w+\frac{\sigma^2_{xw}}{\sigma^2_{xx}}(x_i-\mu_x), \sigma^2_{ww}-\frac{(\sigma^2_{xw})^2}{\sigma^2_{xx}} \right]\\
&= Norm_{w_i}\left[(\mu_{w}-\frac{\sigma^2_{xw}}{\sigma^2_{xx}}\mu_x)+\frac{\sigma^2_{xw}}{\sigma^2_{xx}}x_i,\sigma^2_{ww}-\frac{(\sigma^2_{xw})^2}{\sigma^2_{xx}}\right]
\end{align}
$$
It is obvious that $Pr(w_i|x_i)$ has a form of $Norm_{w_i}[\phi_0+\phi_1x,\sigma^2]$. Accordingly, we have
$$
\begin{align}
\phi_0 &= \mu_x- \mu_x\frac{\sigma^2_{xw}}{\sigma^2_{xx}}\\
\phi_1 &= \frac{\sigma_{xw}}{\sigma^2_{xx}}
\end{align}
$$


Now we calculate the MLE estimation of $\{\mu_w, \mu_x, \sigma^2_{ww}, \sigma^2_{xw}, \sigma^2_{xx} \}$

We rewrite the joint probability as :
$$
Pr(w_i,x_i)= Norm_{[w_i, x_i]}[\mu, \Sigma]
$$
where $\mu=[\mu_w,\mu_x]^T$, $\Sigma = \begin{bmatrix}\sigma^2_{ww}  & \sigma^2_{xw} \\ \sigma^2_{xw} & \sigma^2_{xx} \end{bmatrix}$.



The maximum likelihood function is :
$$
\begin{align}
\prod_{i}^{I}Pr(w_i, x_i) &= \prod_{i}^{I} Norm_{[w_i, x_i]}[\mu, \Sigma]\\
\sum_{i}^{I}\log Pr(w_i, x_i) &= \prod_i^I \log(Norm_{[w_i, x_i]}[\mu, \Sigma])
\end{align}
$$
Let $L = \log(Norm_{[w_i, x_i]}[\mu, \Sigma])$, expand it we have:
$$
L= -\frac{1}{2}\log(2\pi)-\frac{1}{2}\log(|\Sigma|)-\frac{1}{2}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)
$$
We need to calculate the partial derivatives regards to $\Sigma$  and $\mu$ respectively and equates the derivative to zero to obtain the  MLE estimation.

Here, I used the technique mentioned by Thomas Minka. Please refer to his [Old and new matrix algebra useful for statistics](https://tminka.github.io/papers/matrix/) for details on how to calculate derivatives of matrix/vector. 

First we calculate $\frac{\partial L}{\partial \Sigma}$:
$$
\frac{\partial L}{\partial \Sigma}=\left(-\frac{1}{2}\log|\Sigma|\right)^{'}_{\Sigma} -\left( \frac{1}{2}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu) \right)^{'}_{\Sigma}
$$
For the former part:
$$
\because \left(-\frac{1}{2}\log|\Sigma|\right)^{'}_{\Sigma}=-\frac{1}{2}tr(\Sigma^{-1}d\Sigma)\\
\therefore 
\begin{align}
\frac{d(-\frac{1}{2}\log|\Sigma|)}{d\Sigma}&=-\frac{1}{2}\Sigma^{-1}\\
&=\frac{1}{2} \cdot (2\Sigma^{-1}-(\Sigma^{-1}\circ \bf{I}))\\
&= -\Sigma^{-1}+\frac{1}{2}(\Sigma^{-1} \circ \bf{I})
\end{align}
$$
For the latter part
$$
\begin{align}
\because \left( \frac{1}{2}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu) \right)^{'}_{\Sigma} &= \left( \frac{1}{2}tr\left((\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \Sigma^{-1}\right) \right)^{'}_{\Sigma}\\
&=\frac{1}{2}tr\left((\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^Td \Sigma^{-1}\right)\\ 
&=\frac{1}{2}tr\left(-(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}(d\Sigma)\Sigma^{-1}\right) \\
&=\frac{1}{2}tr\left(-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}(d\Sigma)\right) \\

\therefore  \frac{d \left( \frac{1}{2}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu) \right)}{d\Sigma} &=-\frac{1}{2}\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}\\
&= \frac{1}{2}\left\{ (-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}) \\
+ (-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1})^T \\
-((-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}) \circ \bf{I}) \right\}

\end{align}
$$



Thus, we have:
$$
\begin{align}
\frac{\partial L}{\partial \Sigma} &= -\Sigma^{-1}+\frac{1}{2}(\Sigma^{-1} \circ \bf{I}) - \frac{1}{2}\left\{ (-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}) \\
+ (-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1})^T \\
-((-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T \cdot \Sigma^{-1}) \circ \bf{I}) \right\}\\
&=\left \{- \Sigma^{-1}+ \Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}\right\}-\frac{1}{2}\left\{-\Sigma^{-1}+\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1} \circ \bf{I}\right\}

\end{align}
$$


Because the derivatives are summable, we sum it from $1$ to $I$ and equate it to zero :
$$
\sum^{I}_1\frac{\partial L}{\partial \Sigma}=0\\
\sum^I_1 \left( \left \{- \Sigma^{-1}+ \Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}\right\}-\frac{1}{2}\left\{-\Sigma^{-1}+\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1} \circ \bf{I}\right\} \right)=0
$$


Notice that the latter part is essentially equals to multiple the elements on the diagonal of the matrix, the upper formula can be reduced to :
$$
\sum^I_1 \left( \left \{- \Sigma^{-1}+ \Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}\right\}\times \begin{bmatrix}\frac{1}{2} & 1 & \dots & 1\\ 1 & \frac{1}{2}& \dots & 1 \\ \vdots & &\ddots &\vdots \\ 1 &\dots &\dots &\frac{1}{2} \end{bmatrix}  \right)=0 \\

\Sigma = \frac{\sum^I_i(\begin{bmatrix}w_i\\x_i\end{bmatrix}-\mu)(\begin{bmatrix}w_i\\x_i\end{bmatrix}-\mu)^T}{I}
$$


Up to now we have already obtain the MLE estimation of  $\Sigma$. We notice that this estimation depends on the value of $\mu$. We now need to estimate it. Once again, we calculate the derivative  of $L$ regarding to $\mu$.
$$
\begin{align}
\frac{\partial L}{\partial \mu} &= \left(-\frac{1}{2}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu) \right)^{'}_{\mu}\\
&=-\frac{1}{2}( ((\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T)^{'}_{\mu} \Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu) + (\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1}((\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^{'}_\mu)\\
&=-\frac{1}{2}(-\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu) - (\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)^T\Sigma^{-1})\\
&=\Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)
\end{align}
$$
Equate the $\sum^I_1\frac{\partial L}{\partial \mu}$ to zero  and solve for  $\mu$ , we can obtain its MLE estimation:
$$
\sum^I_1 \Sigma^{-1}(\left[\begin{matrix}w_i \\xi\end{matrix}\right]-\mu)=0\\
\mu=\frac{\sum^I_1 \begin{bmatrix}w_i\\x_i\end{bmatrix}}{I} 
$$
According to the MLE estimation of $\Sigma$ and $\mu$, we can write out explicitly the MLE estimation of  $\{\mu_w, \mu_x, \sigma^2_{ww}, \sigma^2_{xw}, \sigma^2_{xx} \}$.
$$
\begin{align}
\mu_w = \frac{\sum^I_1 w_i}{I},& \mu_x= \frac{\sum^I_1 x_i}{I}\\
\because \Sigma =\begin{bmatrix}\sigma^2_{ww}  & \sigma^2_{xw} \\ \sigma^2_{xw} & \sigma^2_{xx} \end{bmatrix}&=  \frac{\sum^I_i(\begin{bmatrix}w_i\\x_i\end{bmatrix}-\mu)(\begin{bmatrix}w_i\\x_i\end{bmatrix}-\mu)^T}{I}\\
&=\frac{1}{I}\sum^I_1 \begin{bmatrix} (w_i - \mu_w)^2 & (w_i-\mu_w)(x_i-\mu_x) \\(w_i-\mu_w)(x_i-\mu_x) & (x_i-\mu_x)^2 \end{bmatrix}\\
\therefore \sigma^2_{ww}= \frac{\sum^I_1(w_i-\mu_w)^2}{I}\\
\sigma^2_{xx} = \frac{\sum^I_1(x_i-\mu_x)^2}{I}\\
\sigma^2_{xw} = \frac{\sum^I_1(w_i-\mu_w)(x_i-\mu_x)}{I}
\end{align}
$$


Now we are able to explicitly write out $\phi_0$ and $\phi_1$ with $\{w_i,x_i\}$
$$
\begin{align}
\phi_0 &= \mu_x- \mu_x\frac{\sigma^2_{xw}}{\sigma^2_{xx}}\\
&= \frac{\sum^I_1 w_i}{I}- \frac{\sum^I_1 x_i}{I} \times \frac{\sum^I_1(w_i-\mu_w)(x_i-\mu_x)}{{\sum^I_1(x_i-\mu_x)^2}}\\
\phi_1 &= \frac{\sigma_{xw}}{\sigma^2_{xx}}\\
&= \frac{\sum^I_1(w_i-\mu_w)(x_i-\mu_x)}{{\sum^I_1(x_i-\mu_x)^2}}
\end{align}
$$


## Problem 6.7

For a two-class problem, the decision boundary is the locus of world values $w$ where the posterior probability $Pr(w = 1|x)$ is equal to $0.5$. In other words, it represents the boundary between regions that would be classiﬁed as $w = 0$ and $w = 1$. Consider the generative classiﬁer from section 6.4.2. Show that with equal priors $Pr(w = 0) = Pr(w = 1) = 0.5$ points on the decision boundary (the locus of points where $Pr(w = 0|x) = Pr(w = 1|x)) $obey a constraint of the form 
$$
ax^2 + bx + c = 0, 
$$
where and ${a,b,c}$ are scalars. Does the shape of the decision boundary for the logistic regression model from section 6.4.1 have the same form?

**Answers:**

According to the Bayesian rule we can calculate $Pr(w|x)$:
$$
\begin{align}
Pr(w=0|x)&=\frac{Pr(x|w=0)Pr(w=0)}{Pr(x|w=0)Pr(w=0)+Pr(x|w=1)Pr(w=1)}\\
&=\frac{Pr(x|w=0)\times 0.5}{Pr(x|w=0\times 0.5+Pr(x|w=1)\times 0.5}\\
&= \frac{Pr(x|w=0)}{Pr(x|w=0)+Pr(x|w=1)}
\end{align}
$$
Similarly we have:
$$
Pr(w=1|x)=\frac{Pr(x|w=1)}{Pr(x|w=0)+Pr(x|w=1)}
$$
Because the decision boundary is defined as $Pr(w=0|x)=Pr(w=1|x)$. We have :
$$
\begin{align}
Pr(w=0|x)&=Pr(w=1|x)\\
\frac{Pr(x|w=0)}{Pr(x|w=0)+Pr(x|w=1)}&=\frac{Pr(x|w=1)}{Pr(x|w=0)+Pr(x|w=1)}\\
Pr(x|w=0)&=Pr(x|w=1)\\


\end{align}
$$
Remember that in that section, we model the data $x$ using a normal distribution, therefore we have:
$$
\begin{align}
Pr(x|w=0)&=Pr(x|w=1)\\
Norm_x[\mu_0,\sigma^2_0]&= Norm_x[\mu_1,\sigma^2_1]\\
\frac{1}{\sqrt{2\pi}\sigma_0}\exp(\frac{(x-\mu_0)^2}{2\sigma_0^2}) &= \frac{1}{\sqrt{2\pi}\sigma_1}\exp(\frac{(x-\mu_1)^2}{2\sigma_1^2})\\
-\log \sigma_0+ \frac{(x-\mu_0)^2}{2\sigma^2_0}&=-\log \sigma_1 +\frac{(x-\mu_1)^2}{2\sigma_1^2}\\
(\frac{1}{2\sigma_0^2}-\frac{1}{2\sigma_1^2})x^2-(\frac{\mu_0}{\sigma_0^2}-\frac{\mu_1}{\sigma_1^2})x&+(\log\sigma_0-\log\sigma_1+\frac{\mu_0^2}{2\sigma^2_0}-\frac{\mu_1^2}{2\sigma^2_1})=0

\end{align}
$$
We can see that the data points that are located on the decision boundary  are in a form of $ax^2+bx+c=0$ . It is a shape of **parabolic**.



For the logistic regression model in Section 6.4.2, we can similarly calculate its decision boundary:
$$
\begin{align}
Pr(w=1|x)&= Pr(w=0|x)\\
Bern_{w=1}[sig(\phi_0+\phi_1x)] &= Bern_{w=0}[sig(\phi_0+\phi_1x)]\\
\frac{1}{1+\exp [-\phi_0-\phi_1x]}&=1-\frac{1}{1+\exp [-\phi_0-\phi_1x]}\\
\phi_0+\phi_1x&=0
\end{align}
$$
We can see that the decision boundary is in a form of $ax+b=0$. It is a **line**.



## Problem 6.8

 Consider a generative classiﬁcation model for $1D$ data with likelihood terms 
$$
\begin{align}
Pr(x_i|w_i=0)&=Norm_{x_i}[0,\sigma^2]\\
Pr(x_i|w_i=1)&=Norm_{x_i}[0,1.5\sigma^2].
\end{align}
$$
What is the decision boundary for this classiﬁer with equal priors $Pr(w = 0) = Pr(w = 1) = 0.5$? Develop a discriminative classiﬁer that can produce the same decision boundary. Hint: base your classiﬁer on a quadratic rather than a linear function.



**Answers:**

Similar to Problem 6.7 we can calculate posterior according to Bayes' rule:
$$
Pr(w_i=0|x_i)= \frac{Pr(x_i|w_i=0)}{Pr(x_i|w_i=0)+Pr(x_i|w_i=1)}\\
Pr(w_i=1|x_i)= \frac{Pr(x_i|w_i=1)}{Pr(x_i|w_i=0)+Pr(x_i|w_i=1)}
$$
Then we can calculate the decision boundary accordingly:
$$
\begin{align}
Pr(w_i=1|x_i)&=Pr(w_i=0|x_i)\\
Pr(x_i|w_i=0)&=Pr(x_i|w_i=1)\\
Norm_{x_i}[0,\sigma^2]&= Norm_{x_i}[0,1.5\sigma^2]\\
\frac{1}{\sqrt{2\pi}\sigma}\exp[-\frac{x^2_i}{2\sigma^2}]&=\frac{1}{\sqrt{2\pi}\times1.5\sigma}\exp[-\frac{x^2_i}{3\sigma^2}]\\
x^2_i-6\log\frac{2}{3} \times \sigma^2 &=0
\end{align}
$$
We can also develop a discriminative classifier with the same decision boundary. Let's define a quadratic function of data $x$, as 
$$
f(x)=x^2_i-6\log\frac{2}{3} \times \phi^2
$$
where $\phi$ is a parameter.



Since the world state is a binary, we use a Bernoulli distribution to model it. Again to guarantee the input data is within the range $[0,1]$ , we use a sigmoid function to restrict it. Our discriminative classifier is defined as:
$$
Pr(w|x)= Bern_{w}[sig(f(x))]
$$


Now calculate the decision boundary:
$$
\begin{align}
Pr(w=0|x_i)&=Pr(w=1|x_i)\\
\frac{1}{1+\exp(-f(x))}&=1-\frac{1}{1+\exp(-f(x))}\\
1&=exp(-f(x))\\
0 &= f(x)\\
x^2_i-6\log\frac{2}{3} \times \phi^2 &= 0
\end{align}
$$





## Problem 6.9

Consider a generative binary classiﬁer for multivariate data based on multivariate normal likelihood terms
$$
\begin{align}
Pr(x_i|w_i=0) &= Norm_{x_i}[\mu_0,\Sigma_0]\\
Pr(x_i|w_i=1) &= Norm_{x_i}[\mu_1,\Sigma_1]
\end{align}
$$
and a discriminative classiﬁer based on logistic regression for the same data 
$$
Pr(w_i|x_i)=Bern_{w_i}\left[sig[\phi_0+\phi^Tx_i]\right]
$$
where there is one entry in the gradient vector $\phi$ for each entry of $x_i$. How many parameters does each model have as a function of the dimensionality of $x_{i}$? What are the relative advantages and disadvantages of each model as the dimensionality increases?

**Answers:**

For generative models, there are $4$ parameters as a function of the dimensionality of $x_i$. They are $\{ \Sigma_0,  \Sigma_1\}$ , which are squared covariance matrix, and $\{\mu_0,\mu_1\}$, which are vectors that are of the same dimensionality of $x_{i}$ .

For discriminative models, there are also $2$ parameters that are as a function of the dimensionality  of $x_i$. It is  $\{\phi_0, \bf{\phi}\}$, which are vectors that are of the same dimensionality of $x_{i}$ . 



Let's suppose the dimensionality of $x_i$ as $I$. 

For generative models, supposed that the $I$ increases $\Delta I$, the dimensionality of  $ \{ \Sigma_0,  \Sigma_1\}$ will be increased by $(I+\Delta I)^2 - I^2= 2I\Delta I+ \Delta I^2$ for each matrix. We can see that the dimensionality will increased in a quadratic form. The dimensionality of $\{\mu_0,\mu_1\}$ will increased by $ \Delta I$  for each vectors. It suggests that approximately $2I\Delta I+ \Delta I^2 +2\Delta I$ more parameters. 



For discriminative models, supposed that the $I$ increases $\Delta I$, the dimensionality of  $\{\phi_0, \bf{\phi}\}$ will also increased by $\Delta I$ by each vector. We can see that the dimensionality of parameters will increased in a linear form. It suggests that we only need to estimate $2\Delta I$ more parameters.



More parameters suggests that more data are needed for estimation. As the dimensionality increase, the dimensionality of parameters in generative models increased much more rapidly than that in discriminative models. It suggests that with the same increment in dimensionality, the required amount of data in discriminative model is much lower than generative model.



## Problem 6.10

One of the problems with the background subtraction method described is that it erroneously classiﬁes shadows as foreground. Describe a model that could be used to classify pixels into three categories (foreground, background, and shadow).



**Answers:**

Here we describe a generative classification model for classifying pixels into three categories (foreground, background, and shadow). Here, our goal is to infer a labels $w_n \in \{0,1,2\}$ which indicates whether the $n^{th}$ pixel in the image is part of a known background $(w=0)$ or whether a foreground object is occluding it $(w=1)$ or whether a shadow of a foreground objects $(w=2)$. This is based on the RGB pixel data $x_n$ at that pixel. 



Because we are developing a generative model, we are modeling the contingency of data according to the world state.

We use a normal distribution to model the  background:
$$
Pr(x_n|w=0) = Norm_{x_n}[\mu_{n0}, \Sigma_{n0}]
$$
For the foreground class,  again, we use a uniform distribution:
$$
Pr(x_n|w=1)=
\left\{
\begin{array}
 1 1/255^3  &\qquad 0<x^R_n, x^G_n, x^B_n <255,\\
0 &\qquad otherwise
\end{array}
\right.
$$
For the shadow class we can also use a uniform distribution. However we limit the pixel color within certain ranges $[\alpha, \beta]$:
$$
Pr(x_n|w=1)=
\left\{
\begin{array}
 1 1/(\beta -\alpha)^3  &\qquad \alpha<x^R_n, x^G_n, x^B_n <\beta,\\
0 &\qquad otherwise
\end{array}
\right.
$$
The shadow range is empirical.

For more about shadow detection using hue. Please refer to 

> Cucchiara, R., Grana, C., Piccardi, M., & Prati, A. (2003). Detecting moving objects, ghosts, and shadows in video streams. *IEEE transactions on pattern analysis and machine intelligence*, *25*(10), 1337-1342.



In addition, we can use a discriminative model to handle, this tasks. We could modeling the posterior using a categorical distribution with $3$ categories. That is
$$
Pr(w|x)= Cat_{w}[\lambda(x)]
$$
where $\lambda$ is a function of the data $x$.



# Chapter 7



## Problem 7.1

 Consider a computer vision system for machine inspection of oranges in which the goal is to tell if the orange is ripe. For each image we separate the orange from the background and calculate the average color of the pixels, which we describe as a $3 \times 1$ vector $x$. We are given training pairs ${x_i,w_i}$ of these vectors, each with an associated binary variable $w \in \{0,1\}$ that indicates that this training example was unripe ($w = 0$) or ripe ($w = 1$). Describe how to build a generative classiﬁer that could classify new examples $x^∗$ as being ripe or unripe.



**Answer:**

Our goal is to input the vector $x$ and return a label $w\in \{0,1\}$ which indicates whether the orange is unripe ($w=0$) or ripe ($w=1$).  We are going to use a generative approach to develop a classifier to tell whether the orange is ripe or not. We will model the possibility of the vector $x$ and parameterize it by the label state $w$.  Because We will use a multivariate normal distribution to describe the vector $x$:
$$
Pr(x|w)= Norm_x[\mu_w, \Sigma_w]
$$
where $\mu_w$ and $\Sigma_w$ are the mean and covariance of the distribution and are functions of $w$.

Because, $\mu_w$ and $\Sigma_w$ are functions of $w$ and $w$ is a binary value. Alternatively, we can write out the model explicitly：
$$
Pr(x|w=0)=Norm_x[\mu_0,\Sigma_0]\\
Pr(x|w=1)=Notm_x[\mu_1, \Sigma_1]
$$
Because the label state $w$ is a binary value, we can use a Bernoulli distribution to describe the prior:
$$
Pr(w)= Bern_w[\lambda]
$$


Then, we can learn the parameters $\theta=\{\mu_0,\Sigma_0, \mu_1, \Sigma_1 \}$ from the training pairs ${w_i, x_i}$. One way isto calculate the MLE of the parameters:
$$
\hat \mu_0,\hat \Sigma_0= \arg \max_{\mu_0,\Sigma_0} \left[\prod Norm_{x_i}[\mu_0,\Sigma_0]\right]\\
\hat \mu_1,\hat \Sigma_1= \arg \max_{\mu_1,\Sigma_1} \left[\prod Norm_{x_i}[\mu_1,\Sigma_1]\right]
$$
After we learned the parameters $\theta$ from the data, we can calculate the posterior probability with the new data $x^*$ using the Bayes' rule:
$$
Pr(w=1|x^*)=\frac{Pr(x^*|w=1)Pr(w=1)}{\sum^{1}_{i=0}Pr(x^*|w=i)Pr(w=i)}
$$



## Problem 7.2

It turns out that a small subset of the training labels $w_i$ in the previous example were wrong. How could you modify your classiﬁer to cope with this situation?

**Answers: **

If I know that the correct subset, I will correct them and then learn the parameters $\theta$ again from the corrected training set again.



## Problem 7.3

 Derive the M-step equations for the mixtures of Gaussians model (equation 7.19). 



**Answers:**

The M-step equations are obtained by taking the derivatives of the equation 7.18 and equate it to zero. Before we calculate the derivatives of the equation 7.18, we simplify it a little bit.
$$
\begin{align}
\hat \theta^{[t+1]} &= \arg\max_\theta \left[ \sum^I_{i=1} \sum^K_{k=1} r_{ik} \log(\lambda_k \cdot Norm_{x_i}[\mu_k, \Sigma_k])\right] (7.18)
\end{align}
$$
Let our target function $L$ be:
$$
\begin{align}
L&=\sum^I_{i=1} \sum^K_{k=1} r_{ik} \log(\lambda_k \cdot Norm_{x_i}[\mu_k, \Sigma_k]) +\upsilon\left((\sum^K_{k=1}\lambda_k)-1\right)\\
&=\sum^I_{i=1} \sum^K_{k=1} r_{ik}\cdot(\log (\lambda_k) + log(Norm_{x_i}[\mu_k, \Sigma_k])) + \upsilon\left((\sum^K_{k=1}\lambda_k)-1\right)\\
&= \sum^I_{i=1} \sum^K_{k=1} r_{ik}\cdot\left(\log (\lambda_k)+log(\frac{1}{(2\pi)^{D/2}|\Sigma_k|^{1/2}}\exp(-0.5(x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k))\right)+ \upsilon\left((\sum^K_{k=1}\lambda_k)-1\right)\\
&= \sum^I_{i=1} \sum^K_{k=1} r_{ik}\cdot\left(\log (\lambda_k) -\frac{D}{2}\log{2\pi -\frac{1}{2}\log(|\Sigma_k|)-\frac{1}{2}} (x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k)\right)+ \upsilon\left((\sum^K_{k=1}\lambda_k)-1\right)
\end{align}
$$
where $\upsilon\left((\sum^K_{k=1}\lambda_k)-1\right)$ is the LaGrange multiplier that used to constrained the sum of $\lambda_k$ equals to one. Otherwise  it will get punished by the coefficient $\upsilon$.



First lets, calculate the estimation of $\lambda_k$. We calculate the  $\frac{\partial L}{\partial \lambda_k}$:
$$
\begin{align}
\frac{\partial L}{ \partial \lambda_k} &= \sum^I_{i=1}\frac{r_{ik}}{\lambda_k}+\upsilon
\end{align}
$$
We should pay attention to two things here. First, we can notice that  $\sum^K_{k=1}$ is omitted. Because for each $i$, there is only one $k$ that has none zero derivatives. You can verified this by calculating $\frac{\partial L}{\partial \lambda_1}$  and you will realized the above equation is  just a general form. The second thing is , although $r_{ik}$ contains $\lambda_k $, but that $\lambda_k$ is already known in the E-step and different from the $\lambda_k $ we would like to estimate here.

If we equate the above equation to zero, we will get:
$$
\sum^I_{i=1}r_{ik}+\upsilon\lambda_k=0 \qquad (*)
$$


If we sum up this equation from $1$ to $K$, we will get:
$$
\sum^K_{k=1}\sum^I_{i=1}r_{ik}+\upsilon \sum^K_{k=1}\lambda_k=0
$$
Notice that $\sum^K_{k=1}\lambda_k=1$, we can easily solve for $\upsilon$:
$$
\upsilon = -\sum^K_{k=1}\sum^I_{i=1}r_{ik}
$$
Substituting the $\upsilon$ to equation $(*)$ , we can obtain the MLE of $\lambda_k$:
$$
\lambda_k= \frac{\sum^I_{i=1}r_{ik}}{\sum^K_{k=1}\sum^I_{i=1}r_{ik}}
$$


Similarly, we can obtain the MLE of $\Sigma_k$, by calculating the $\frac{\partial L}{\partial\Sigma_k}$ and equate it to zero. Through, observing the target function $L$, we can notice that there are only two terms that is related to $\Sigma_k$, therefore we have:
$$
\frac{\partial L}{\partial \Sigma_k}=\sum^I_{i=1}r_{ik}\cdot \left( \frac{1}{2}\log(|\Sigma_k|)-\frac{1}{2} (x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k) \right)^{'}_{\Sigma_k}
$$


Now we calculate $\frac{d\log |\Sigma_k|}{d\Sigma_k}$ and $\frac{d(x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k)}{d\Sigma_k}$ respectively:
$$
\because d\log |\Sigma_k|= tr(\Sigma_k^{-1}d \Sigma_k) \\
\therefore \frac{d\log |\Sigma_k|}{d\Sigma_k}=\Sigma_k^{-1}
$$
Similarly:
$$
\begin{align}
\because d(x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k)&= d \; tr[(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}]\\
&=tr[(x_i-\mu_k)(x_i-\mu_k)^T \;d \Sigma_k^{-1}]\\
&= tr[-(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}(d\Sigma_k)\Sigma_k^{-1}]\\
&= tr[-\Sigma_k^{-1}(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}d\Sigma_k]\\
\therefore \frac{d(x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k)}{d\Sigma_k}&= -\Sigma_k^{-1}(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}
\end{align}
$$
Now we can explicitly write out $\frac{\partial L}{\partial \Sigma_k} $:
$$
\frac{\partial L}{\partial \Sigma_k}=\sum^I_{i=1}r_{ik}\cdot\left( \frac{1}{2}\Sigma_k^{-1} - \frac{1}{2}\Sigma_k^{-1}(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}\right)
$$
Equate it to zero and solve the equation and we can get the MLE of $\Sigma_k$:
$$
\begin{align}
\frac{\partial L}{\partial \Sigma_k} &=0\\
\sum^I_{i=1}r_{ik}\cdot\left( \frac{1}{2}\Sigma_k^{-1} - \frac{1}{2}\Sigma_k^{-1}(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}\right) &= 0\\
\sum^I_{i=1}r_{ik}\cdot\left( \Sigma_k - (x_i-\mu_k)(x_i-\mu_k)^T\right) &=0\\
\Sigma_k=\frac{\sum^I_{i=1}r_{ik}(x_i-\mu_k)(x_i-\mu_k)^T}{\sum^I_{i=1}r_{ik}}
\end{align}
$$


Lastly, we calculate the MLE of $\mu_k$ :
$$
\begin{align}
\frac{\partial L}{\partial \mu_k}&=\sum^I_{i=1}\left(\frac{1}{2}(x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k)\right)^{'}_{\mu_k}\\
\because d(x_i-\mu_k)^T\Sigma^{-1}_k(x_i-\mu_k)&= d \; tr[(x_i-\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}]\\
&=tr[-(d\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}-(x-\mu_k)(d\mu_k)^T\Sigma^{-1}_k]\\
&=tr[-(d\mu_k)(x_i-\mu_k)^T\Sigma_k^{-1}]+tr[-(x-\mu_k)(d\mu_k)^T\Sigma^{-1}_k]\\
&=tr[-(x_i-\mu_k)^T\Sigma_k^{-1}d\mu_k]+tr[-(\Sigma_k^{-1})^T(d\mu_k)(x-\mu_k)^T]\\
&=tr[-(x_i-\mu_k)^T\Sigma_k^{-1}d\mu_k]+tr[-(x-\mu_k)^T(\Sigma_k^{-1})^Td\mu_k]\\
&=tr[-2(x_i-\mu_k)^T\Sigma_k^{-1}d\mu_k]\\
&=tr[-2\Sigma_k^{-1}(x_i-\mu_k)d\mu_k]\\
\therefore \frac{\partial L}{\partial \mu_k}&=\sum^I_{i=1}r_{ik}\cdot\left(-2\Sigma_k^{-1}(x_i-\mu_k)\right)
\end{align}
$$
Equate the $\frac{\partial L}{\partial \mu_k}$ to zero, and solve for $\mu_k$:
$$
\begin{align}
\frac{\partial L}{\partial \mu_k}&=0\\
\sum^I_{i=1}r_{ik}\cdot\left(-2\Sigma_k^{-1}(x_i-\mu_k)\right)&=0\\
\mu_k &= \frac{\sum^I_{i=1}r_{ik}x_i}{\sum^I_{i=1}r_{ik}}
\end{align}
$$



## Problem 7.4

 Consider modeling some univariate continuous visual data $x \in [0,1]$ using a mixture of beta distributions. Write down an equation for this model. Describe in words what will occur in (i) the E-step and (ii) the M-step.



**Answers:**

Suppose that we are now model the continuous visual data $x$ using a mixture of beta distribution. The model is:
$$
Pr(x|\theta)=\sum^K_{k=1}\lambda_kBeta_{x}[\alpha_k,\beta_k]
$$
where $\alpha_k$ and $\beta_k$ are the parameters of the $k$th beta distribution and $\lambda_{1\cdots K}$ are positive valued weights that sum to one.

To learn the parameters $\theta =\{\alpha_k \beta_k, \lambda_k\}^K_{k=1}$ from training data $\{x_i\}^I_{i=1}$,  we could apply the maximum likelihood approach:
$$
\begin{align}
\hat\theta &= \arg\max_{\theta}\left[\sum^I_{i=1}log[Pr(x_i|\theta)]\right]\\
&=\arg\max_{\theta}\left[\sum^I_{i=1}log[\sum^K_{k=1}\lambda_kBeta_{x_i}[\alpha_k,\beta_k]]\right]
\end{align}
$$
We can apply EM algorithm to finished the estimation. But before staring the EM algorithm, we first define the hidden variables. Similar to MoG, we can also consider the mixture of Beta model as the marginalization of a joint probability distribution between the observed data $x $ and a discrete hidden variable $h$ that takes values $h \in \{1 \cdots K\}$. We can re-defined our model as follow:
$$
\begin{align}
Pr(x|h,\theta)&=Beta_x[\alpha_h,\beta_h]\\
pr(h|\theta) &= Cat_h[\lambda]
\end{align}
$$
where $\lambda =[\lambda_1, \cdots,\lambda_K]$ are parameters of the categorical distribution. Compared to the models at the beginning, we can see that, here what we do is actually extract the $\lambda$ parameters and used it described the hidden variables. These, two descriptions of model are essentially equivalent.

Now we can lean the parameters $\theta =\{\lambda_k,\alpha_k,\beta_k\}^K_{k=1}$ from training data $\{x_i\}^I_{i=1}$. 

First we initialize the parameters $\theta$ randomly and alternately execute the E- and M-steps:

**E-steps:** we obtain the distribution of hidden variables $q_i(h_i)$ by calculating the posterior probability distribution $Pr(h_i|x_i)$:
$$
\begin{align}
\hat q_i(h_i)= Pr(h_i=k|x_i,\theta^{[t]})&=\frac{Pr(x_i|h_i=k,\theta^{[t]})Pr(h_i=k,\theta^{[t]})}{\sum^K_{j=1}Pr(x_i|h_i=j, \theta^{[t]})Pr(h_i=j, \theta^{[t]})}\\
&= \frac{\lambda_kBeta_{x_i}[\alpha_k,\beta_k]}{\sum^K_{j=1}\lambda_jBeta_{x_i}[\alpha_j, \beta_j]}\\
&= r_{ik}
\end{align}
$$
**M-steps:** we can maximize the bound with respect to the parameters $\theta$ , 
$$
\begin{align}
\hat\theta^{t+1} &= \arg\max_{\theta}\left[\sum^I_{i=1}\sum^K_{k=1} \hat q_i(h_i=k) log[Pr(x_i,h_i=k|\theta)]\right]\\
&= \arg\max_{\theta}\left[\sum^I_{i=1}\sum^K_{k=1} r_{ik}\log[\lambda_kBeta_{x_i}[\alpha_k,\beta_k]]\right]
\end{align}
$$


## Problem 7.5

Prove that the student $t$-distribution over $x$ is the marginalization with respect to $h$ of the joint distribution $Pr(x,h)$ between $x$ and a hidden variable $h$ where 
$$
Stud_x[\mu,\sigma^2,v] = \int Norm_x[\mu,\sigma^2/h]Gam_h[v/2,v/2]dh.
$$

Note: The term of normal distribution on the book is wrong. The covariance should be divided by $h$, but not $v$.

**Answers:**

To prove this equation, you must know the form of student distribution, normal distribution , gamma distribution and the the gamma function. We listed as below before we start the deduction.
$$
\begin{align}
Stud_x[\mu,\sigma,v] &=\frac{\Gamma[\frac{v+1}{2}]}{\sqrt{v\pi\sigma^2}\Gamma[\frac{v}{2}]}\left(1+\frac{(x-\mu)^2}{v\sigma^2}\right)^{-\frac{v+1}{2}}\\
Norm_x[\mu,\frac{\sigma^2}{h}]&=\frac{1}{\sqrt{2\pi\sigma^2}} \cdot h^{\frac{1}{2}} \cdot \exp \left[-\frac{1}{2}\frac{(x-\mu)^2}{\sigma^2}h\right]\\
Gam_h[\frac{v}{2},\frac{v}{2}]&=\frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma(\frac{v}{2})}\cdot h^{\frac{v}{2}-1} \cdot \exp \left[ -\frac{1}{2}vh\right]
\end{align}\\
\Gamma[x]= \int t^{x-1}e^{-t}dt
$$
Now lets start the deduction:
$$
\begin{align}
&\int Norm_x[\mu,\frac{\sigma^2}{h}] \cdot Gam_h[\frac{v}{2},\frac{v}{2}] \;dh\\
&=\int \frac{1}{\sqrt{2\pi\sigma^2}} \cdot \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma(\frac{v}{2})} \cdot h^{\frac{v-1}{2}} \cdot \exp \left[ -\frac{h}{2}(\frac{(x-\mu)^2+v\sigma^2}{\sigma^2})\right]\; dh\\
&= \frac{1}{\sqrt{2\pi\sigma^2}} \cdot \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma(\frac{v}{2})}\int h^{\frac{v-1}{2}} \cdot \exp \left[ -\frac{h}{2}(\frac{(x-\mu)^2+v\sigma^2}{\sigma^2})\right]\; dh   \qquad (*)
\end{align}
$$
Let $z=\frac{h}{2}(\frac{(x-\mu)^2+v\sigma^2}{\sigma^2}) $ , then we can know :
$$
\begin{align}
h&=\frac{2\sigma^2}{v\sigma^2+(x-\mu)^2} \cdot z\\
dh&= \left(\frac{v\sigma^2+(x-\mu)^2}{ 2\sigma^2}\right)^{-1} dz

\end{align}
$$
Now we can arrange the equation $(*)$:
$$
\begin{align}
Eq (*) &= \frac{1}{\sqrt{2\pi\sigma^2}} \cdot \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma(\frac{v}{2})} \int \left(\frac{v\sigma^2+(x-\mu)^2}{2\sigma^2}\right)^{-1}\cdot \left(\frac{v\sigma^2+(x-\mu)^2}{2\sigma^2}\right)^{-\frac{v-1}{2}} \cdot z^{\frac{v-1}{2}} \cdot \exp(-z) \;dz\\
&=\frac{1}{\sqrt{2\pi\sigma^2}} \cdot \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma(\frac{v}{2})} \cdot \left(\frac{v\sigma^2+(x-\mu)^2}{2\sigma^2}\right)^{-\frac{v+1}{2}} \int z^{\frac{v+1}{2}-1}\exp{(-z)}\;dz\\
&=\frac{1}{\sqrt{2\pi\sigma^2}} \cdot \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma(\frac{v}{2})} \cdot \left(\frac{v\sigma^2+(x-\mu)^2}{2\sigma^2}\right)^{-\frac{v+1}{2}} \cdot \Gamma[\frac{v+1}{2}]\\
&=\frac{\Gamma[\frac{v+1}{2}]}{\sqrt{2\pi\sigma^2}\Gamma[\frac{v}{2}]} \cdot (\frac{v}{2})^{-\frac{1}{2}} \cdot (\frac{v}{2})^{\frac{1}{2}} \cdot (\frac{v}{2})^{\frac{v}{2}} \cdot \left(\frac{v\sigma^2+(x-\mu)^2}{2\sigma^2}\right)^{-\frac{v+1}{2}}\\
&=\frac{\Gamma[\frac{v+1}{2}]}{\sqrt{v\pi\sigma^2}\Gamma[\frac{v}{2}]} \cdot (\frac{2}{v})^{-\frac{v+1}{2}} \cdot \left(\frac{v\sigma^2+(x-\mu)^2}{2\sigma^2}\right)^{-\frac{v+1}{2}}\\
&=\frac{\Gamma[\frac{v+1}{2}]}{\sqrt{v\pi\sigma^2}\Gamma[\frac{v}{2}]} \cdot \left(1+ \frac{(x-\mu)^2}{v\sigma^2}\right)^{-\frac{v+1}{2}}\\
&= Stud_x[\mu, \sigma^2, v]
\end{align}
$$


## Problem 7.6

Show that the peak of the Gamma distribution $Gam_z[α,β]$ is at
$$
\hat z = \frac{\alpha-1}{\beta}
$$
**Answers:**

Calculate the derivatives of the probability density function respect to $z$:
$$
\begin{align}
\frac{\partial Gam_z[\alpha,\beta]}{\partial z} &= \frac{\beta^{\alpha}}{\Gamma[\alpha]}  \left( \exp[-\beta z](-\beta)z^{\alpha-1} + \exp[-\beta z](\alpha -1)\right)\\
&= \frac{\beta^{\alpha}}{\Gamma[\alpha]} \exp[-\beta z]((-\beta)z^{\alpha-1}+ (\alpha -1)z^{\alpha-2})
\end{align}
$$
equate it to zero and solve for $z$:
$$
\begin{align}
\frac{\beta^{\alpha}}{\Gamma[\alpha]} \exp[-\beta z]((-\beta)z^{\alpha-1}+ (\alpha -1)z^{\alpha-2})=0\\
z= \frac{\alpha-1}{\beta}
\end{align}
$$

## Problem 7.7

 Show that the Gamma distribution is conjugate to the inverse scaling factor of the variance in a normal distribution so that
$$
Norm_{x_i}[\mu,\Sigma/h_i]Gam_{hi}[ν/2,ν/2] = \kappa Gam_{h_i}[\alpha,\beta],
$$
and ﬁnd the constant of proportionality $\kappa$ and the new parameters $\alpha$  and $\beta$.

**Answers: **

The normal distribution is :
$$
Norm_{x_i}[\mu,\Sigma/h_i]=\frac{h_i^{D/2}}{(2\pi)^{D/2}|\Sigma|^{1/2}}\cdot\exp[\frac{1}{2}(x-\mu)^T(\frac{\Sigma}{h_i})^{-1}(x-\mu)]\\
=\frac{h_i^{D/2}}{(2\pi)^{D/2}|\Sigma|^{1/2}}\cdot\exp[\frac{1}{2}(x-\mu)^T\Sigma)^{-1}(x-\mu)h_i]\\
$$
The gamma distribution is :
$$
Gam_{h_i}[\frac{v}{2},\frac{v}{2}]= \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma[\frac{v}{2}]}\exp[-\frac{v}{2}h_i]h_i^{\frac{v}{2}-1}
$$


Thus:
$$
\begin{align}
Norm_{x_i}[\mu,\Sigma/h_i]Gam_{hi}[ν/2,ν/2]&= \frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma[\frac{v}{2}]} \cdot \frac{1}{(2\pi)^{d/2}|\Sigma|^{1/2}} \cdot h^{\frac{v-D}{2}} \cdot \exp[-\frac{h_i}{2}((x-\mu)^T)\Sigma^{-1}(x-\mu)+v]\\
&=\frac{(\frac{v}{2})^{\frac{v}{2}}}{\Gamma[\frac{v}{2}]} \cdot \frac{1}{(2\pi)^{d/2}|\Sigma|^{1/2}} \cdot h^{\frac{v-D}{2}} \cdot \exp[-\frac{h_i}{2}((x-\mu)^T)\Sigma^{-1}(x-\mu)+v]
\end{align}
$$

# Chapter 8 Regression models

## Problem 8.1 
Consider a regression problem where the world stat $w$ is known to be positive. To cope with this we could construct a regression model in which the world state is modeled as a gamma distribution. We could constrain both parameters $\alpha, \beta$ of the gamma distribution to be the same so that $\alpha = \beta$ and make them a function of data $x$ Describe a maximum likelihood approach to fitting this model.

**A:** Because we set the parameters of gamma distribution $\alpha = \beta$, we assume that the these two parameters are linear function of the data $x$, that is $\alpha = \beta = \phi x$. Then we can write out our discriminate model explicitely:

$$
\begin{align}
Pr(W_i|x_i,\phi) &=Gamma_{w_i}[\phi x, phi_x]\\
&= \frac{(\phi x_i)^{\phi x_i}}{\Gamma[\phi x_i]}\cdot exp[-\phi x_i]w^{\phi x_i-1}
\end{align}
$$

We would like to extimate the the parameter $\phi$ with maximum likelihood method, that is

$$
\phi &= \arg\max_{phi} = \prob_{i}^{I} Pr(w_i|x_i,\phi)\\
&=\arg\max \sum_{i}^{I} \log[Pr(w_i|x_i,\phi)]\\
$$

Now we can take the derivatives with respect to $\phi$, equate the resulting expression to zero and solve it. Then we can get the MLE of $\phi$.

## Problem 8.2

Consider a robust regression problem based on the t-distribution rather than normal distribution. Define this model preciesely in mathematical terms and sketch out a maximum likelihood approach to fitting the parameters.

**A:** We build our robust regression model based on t- distribution. Again, we still assum the relationship between the parameters of the problem and the data $x$ to be linear.

$$ 
\begin{align}
Pr(w_i|x_i,\theta) &= Stud_{w_i}[\Phi^Tx_i,\sigma^2,v]\\
&=\frac{\Gamma[frac{v+1}{x}]}{\sqrt{v\pi\sigma^2}\Gamma[\frac{v}{2}]}\left(1+\frac{(x-\Phi^Tx_i)^2}{v\sigma^2}\right)^{-\frac{v+1}{2}}
\end{align}
$$
where $\theta=\{\Phi, \sigma^2, v\}$

Its logrithm form is :

$$
\log(Pr(w_i|x_i,\theta))=\log(\Gamma[\frac{v+1}{2}])-\frac{1}{2}\log(v\pi\sigma^2)+\log[\Gamma(\frac{v}{2})]-\frac{v+1}{2}\log(1+\frac{(w-\Phi^Tx_i)^2}{v\sigma^2})
$$

We can make estimation of the parameter \phi with MLE.

$$
\begin{align}
\hat\theta &= \arg\max_{\theta} \prod_{i=1}^I Pr(w_i|x_i,\theta)\\
&= \arg\max_{\theta} \sum_{i=1}^I \log[\Pr(w_i|x_i,\theta)]
\end{align}
$$

Now we can take derativates with respects to the elements in $\theta$ respectively, equate the final expression to zero, and then solve out the parameters.

## Problem 8.3
Prove that the maximum likelihood solution for the gradient in the linear regression model is

$$
\hat \phi = (XX^T)^{-1}Xw.
$$

**A:** 
According to the text, the target function is 

$$
L=-\frac{I\log[2\pi]}{2}-\frac{I\log[\sigma^2]}{2}-\frac{(w-x^T\phi)^T(w-x^T\phi)}{2\sigma^2}
$$

We calculate the derivatives with respects to $\phi$

$$
\begin{align}
\frac{\partial L}{\partial \phi}&=\left( \frac{(w-x^T\phi)^T(w-x^T\phi)}{\2\sigma^2}\right)^{'}_{\phi}\\
&=\frac{1}{2\sigma^2}\left[\right]^{'}_{\phi}\\
&=\frac{1}{2\sigma^2}\left[w^Tw-2w^Tx^T\phi+\phi^Txx^T\phi\right]^{'}_{\phi}\\
&=\frac{1}{2\sigma^2}\left[0-2xw+2(xx^T)\phi\right]\\
&=\frac{1}{2\sigma^2}\left[-2xw+2(xx^T)\phi\right]\\
\end{align}
$$

Equate the above expression to zero and solve for $\phi$.

$$
\begin{align}
\frac{1}{2\sigma^2}\left[-2xw+2(xx^T)\phi\right]&=0\\
\phi= (xx^T)^{-1}wx
\end{align}
\end{align}
$$

## Problem 8.4
For the Bayesian linear regression model (section 8.2), show that the posterior distributino over the parameters $\phi$ is given by

$$
Pr(\phi|X,w)=Norm_{\phi}\left[\frac{1}{\sigma^2}A^{-1}Xw,A^{-1}\right],
$$

where
$$
A=\frac{1}{\sigma^2}XX^T+\frac{1}{\sigma^2_p}I
$$


