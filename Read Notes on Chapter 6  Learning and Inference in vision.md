# Chapter 6 : Learning and Inference in vision



### Q: What is this chapter about?

**A**: This chapter formulate the computer vision problem using probabilistic models. In computer vision usually, we would like to make inference on the world state w according to the observed data $x$. We can use a probability distribution to described the confidence of our inference. 

For example given an image of a face ( the data $x$), we need to determine whether is a face of a male or female (the world state $w$). We calculate the probability $Pr(w|x)$ to describe the confidence of our a judgement given the image of a face. The higher the probability, the more we are confident on our judgement.

To solve computer vision in this way we need three components:

- *a probabilistic model*. Usually it is a distributions with parameters
- *a learning algorithm*.  Estimate the parameters of the model from the data. The estimation is also considered as learning from the data.
- *an inference algorithm*.  When new data come in, we can input the the data to the estimated model to make inference.



### Q:  Do we always calculate the posterior probability $Pr(w|x)$ directly?

**A**: In fact we do not always calculate the $Pr(w|x)$ directly. It depends on the probability models we choose and the situation we applied. There are two types of probability models: discriminative model and generative model.

In discriminative model, we model the contingency of world according to the data. That is we use the $Pr(w|x)$ to model the problem.

In generative model, we model the contingency of data according to the world state. That is we use the $Pr(x|w)$ to model the problem.



### Q: What are the differences between  discriminative models and generative models besides their modeling of the problem?



**A:**  The main differences lies in three perspectives:

1.  The way of making inference .

   In discriminative model, we model the problem with the posterior distribution $Pr(w|x)$directly. After we obtained the estimated model, we just need to input the new data $x^*$ the model and calculate the posterior probability $Pr(w|x^*)$ directly. Then we can judge the world state according to the posterior probability.

   In generative models, we model the problem with the likelihood distribution $Pr(x|w)$ . But we still targeted on the posterior probability $Pr(w|x)$  . Therefore, after we obtained the estimated likelihood, we calculate the posterior distribution using Bayes' rule. Then we can calculate the posterior probability using the new data $x^*$  .



2. The expressiveness of the final model.

   The choice of the probability distribution for modeling depends on the type and characteristics of the data/world state (binary or multi-valued, discrete or continuous). 

   For a same problem,  the final posterior distribution could be quite different between the discriminative model and generative model.  The discriminative model describe the posterior distribution directly while the generative model obtain the posterior distribution using the Bayes' rule. The posterior distribution in generative model could be quite different from that in discriminative model if the data $x$ and world state $w$ have significant different characteristics.

   

3. Generative model enable incorporation with expert knowledge

   In generative model, we can integrate expert knowledge (or additional knowledge for the problem) by defining a prior. The posterior distribution is a compromise between the likelihood and the prior. It is generally more difficult to integrate expert knowledge in discriminative model.





