---
layout: post
title: 'Robustness and Adversarial Attack'
date: 2022-12-18 12:12:00-0400
description: linear algebra
tags: robustness, adversarial
category: ML
subcategory: Adversarial 

---


Adversarial Attacks and Training


* Explaining and Harnessing Adversarial Examples (FGSM,  ICLR 2015)
* Towards Deep Learning Models Resistant to Adversarial Attacks  (PGD, ICLR 2018)
* Ensemble Adversarial Training : Attacks and Defenses (ICLR 2018)

---

## FGSM

vulnerable to adversarial examples. That is, these machine
learning models misclassify examples that are only slightly different from correctly classified examples
drawn from the data distribution.

perhaps combined with insufficient model
averaging and insufficient regularization of the purely supervised learning problem. We show that
these speculative hypotheses are unnecessary. Linear behavior in high-dimensional spaces is sufficient
to cause adversarial examples.

an adversarial input 
\begin{equation}
    \hat{x} = x  + \eta, ~~ \lVert \eta \rVert_\infty < \epsilon 
\end{equation}
the dot product between a weight vector w and an adversarial example:
\begin{equation}
    w^\top \hat{x} =  w^\top x + w^\top \eta 
\end{equation}

We can maximize this increase
subject to the max norm constraint on $\eta$ by assigning $\eta = \text{sign}(w)$


"accidental steganography," where a linear model is forced to attend
exclusively to the signal that aligns most closely with its weights, even if multiple signals are present
and other signals have much greater amplitude.

Let $\theta$ be the parameters of a model, $x$ the input to the model, $y$ the targets associated with $x$ (for
machine learning tasks that have targets) and $J(\theta; x; y)$ be the cost used to train the neural network.
We can linearize the cost function around the current value of $\theta$, obtaining an optimal max-norm
constrained pertubation of
\begin{equation}
    \eta = \epsilon \text{sign} (\nabla_x J(\theta, x,y))
\end{equation}
We refer to this as the "fast gradient sign method" 


We find that using $\epsilon = 0.25$, we cause a shallow softmax
classifier to have an error rate of $99.9\%$ with an average confidence of $79.3\%$ on the MNIST

Similarly, using $\epsilon = 0.1$, we obtain an error rate of $87.15\%$ and
an average probability of $96.6\%$ assigned to the incorrect labels when using a convolutional maxout
network on a preprocessed version of the CIFAR-10.

\subsection{Adversarial Training of Linear Models versus Weight Decay}


If we train a single model to recognize labels $y \in \{ -1, 1 \}$ with $P(y=1) = \sigma (w^\top x + b)$
where
$\sigma(z)$ is the logistic sigmoid function, then training consists of gradient descent on 
\begin{equation}
    \mathbb{E}_{x, y} \zeta (-y(w^\top x + b))
\end{equation}
where $\zeta(z) = \log (1+ \exp(z))$ is the softplus function. We can derive a simple analytical form for
training on the worst-case adversarial perturbation of $x$ rather than $x$ itself.

Note that the sign of the gradient is just $-\text{sign}(w)$, and that $w^\top sign(w) = \lVert w \rVert_1$.
The adversarial version of logistic regression is therefore to minimize

\begin{equation}
    \mathbb{E}_{x, y} \zeta (y(\epsilon \lVert w\rVert_1 - w^\top x - b))
\end{equation}
This is somewhat similar to L1 regularization. However, there are some important differences. Most
significantly, the L1 penalty is subtracted off the model’s activation during training, rather than
added to the training cost. This means that the penalty can eventually start to disappear if the model
learns to make confident enough predictions that $\zeta$ saturates. This is not guaranteed to happen—in
the underfitting regime, adversarial training will simply worsen underfitting. We can thus view L1
weight decay as being more “worst case” than adversarial training, because it fails to deactivate in
the case of good margin.

When training maxout networks on MNIST, we
obtained good results using adversarial training with $\epsilon = :25$. When applying L1 weight decay to
the first layer, we found that even a coefficient of .0025 was too large, and caused the model to get
stuck with over $5\%$ error on the training set. Smaller weight decay coefficients permitted successful
training but conferred no regularization benefit.


Szegedy et al. (2014b) showed that by training on a mixture of adversarial and clean examples, a
neural network could be regularized somewhat. Training on adversarial examples is somewhat different
from other data augmentation schemes; usually, one augments the data with transformations
such as translations that are expected to actually occur in the test set. This form of data augmentation
instead uses inputs that are unlikely to occur naturally but that expose flaws in the ways that the
model conceptualizes its decision function.
\begin{equation}
    \hat{J}(\theta, x, y) = \alpha J(\theta, x, y) + (1 - \alpha) J(\theta, x + \epsilon \text{sign}(\nabla_x J(\theta, x,y,))
\end{equation}
In all of our experiments, we used $\alpha = 0:5$. Other values may work better; our initial guess of this
hyperparameter worked well enough that we did not feel the need to explore more. This approach
means that we continually update our supply of adversarial examples, to make them resist the current
version of the model. Using this approach to train a maxout network that was also regularized with
dropout, we were able to reduce the error rate from $0.94\%$ without adversarial training to $0.84\%$
with adversarial training.

---

## PGD 

They also suggest the notion of security against a first-order adversary as a natural and broad security guarantee.

"first-order adversary", i.e., the strongest attack utilizing the local first order information about the network.

\subsection{An Optimization View on Adversarial Robustness}
Empirical risk minimization (ERM) has been tremendously successful as a recipe for finding
classifiers with small population risk. Unfortunately, ERM often does not yield models that are
robust to adversarially crafted examples

The first step towards such a guarantee is to specify an attack model, i.e., a precise definition
of the attacks our models should be resistant to. For each data point x, we introduce a set of
allowed perturbations $S\subset \mathbb{R}^d$ Rd that formalizes the manipulative power of the adversary.

While we focus on robustness against $\ell_\infty$-bounded attacks in this paper, we remark that more
comprehensive notions of perceptual similarity are an important direction for future research.

the definition of population risk $\mathbb{E}_D[L]$ by incorporating the above adversary.
\begin{equation}
    \min_\theta \rho (\theta), ~~~ \text{where}~ \rho(\theta)  = \mathbb{E}_{(x,y) \sim D} \Big[ \max_{\delta \in \mathcal{S}} L(\theta, x+ \delta, y) \Big] 
\end{equation}

Our perspective stems from viewing the saddle point problem as the
composition of an inner maximization problem and an outer minimization problem.

\subsection{A Unified View on Attacks and Defenses}



* How can we produce strong adversarial examples, i.e., adversarial examples that fool a model
with high confidence while requiring only a small perturbation?
* How can we train a model so that there are no adversarial examples, or at least so that an
adversary cannot find them easily?


One can interpret this attack as a simple one-step scheme for maximizing the inner part of the
saddle point formulation. A more powerful adversary is the multi-step variant, which is essentially
projected gradient descent (PGD) on the negative loss function.

\begin{equation}
    x^{t+1} = \Pi_{x+S}(x^t + \alpha \cdot \text{sgn}(\nabla_x L(\theta, x^{t}, y))) 
\end{equation}

\textbf{MNIST}. We run \textbf{40} iterations of projected gradient descent as our adversary, with a step size
of \textbf{0.01} (we choose to take gradient steps in the $\ell_\infty$-norm, i.e. adding the sign of the gradient,
since this makes the choice of the step size simpler). We train and evaluate against perturbations
of size  $\epsilon= 0.3$. We use a network consisting of two convolutional layers with 32 and 64 filters
respectively, each followed by $2 \times 2$ max-pooling, and a fully connected layer of size 1024. When
trained with natural examples, this network reaches $99.2\%$ accuracy on the evaluation set. However,
when evaluating on examples perturbed with FGSM the accuracy drops to $6.4\%$.

\textbf{CIFAR10}. For the CIFAR10 dataset, we use the two architectures described in 4 (the original
ResNet and its 10 $\times$ wider variant). We trained the network against a PGD adversary with $\ell_\infty$
projected gradient descent again, this time using 7 steps of size 2, and a total $\epsilon = 8$. For our hardest
adversary we chose 20 steps with the same settings, since other hyperparameter choices didn’t
offer a significant decrease in accuracy. The results of our experiments appear in Table 2.
The adversarial robustness of our network is significant, given the power of iterative adversaries,
but still far from satisfactory. We believe that these results can be improved by further pushing
along these directions, and training networks of larger capacity.


White-box attacks with PGD using the Carlini-Wagner (CW) loss function [6] (directly optimizing
the difference between correct and incorrect logits). CW finds the adversarial instance by finding the smallest noise $\delta$ added to an image x that will change the classification to a class $t$ formally. 
\begin{equation}
    \text{minimize} {\lVert \delta \rVert_p} ~~ \text{s.t.} C(x+\delta) = t
\end{equation}
where $C(x)$ is the predicted class of $x$.


---

## Ensemble
 

Our results, generic with respect to the application domain, suggest
that adversarial training can be improved by decoupling the generation of adversarial examples
from the model being trained. Our experiments with Ensemble Adversarial Training show that the
robustness attained to attacks from some models transfers to attacks from other models.

Single-Step Least-Likely Class Method (Step-LL). This variant of FGSM introduced by Kurakin
et al. (2017a;b) targets the least-likely class, $y_{LL} = \arg \min {h(x)}$ (logit class):
\begin{equation}
    x_{LL}^{adv} = x - \epsilon \cdot \text{sgn}(\nabla_x L(\theta, x, y_{LL}))
\end{equation}

Ensemble Adversarial Training, augments a
model’s training data with adversarial examples crafted on other \textbf{static pre-trained models}. Intuitively,
as adversarial examples transfer between models, perturbations crafted on an external model
are good approximations for the maximization problem in (1). Moreover, the learned model can
not influence the “strength” of these adversarial examples. As a result, minimizing the training loss
implies increased robustness to black-box attacks from some set of models.

Let $A_i$ be an adversarial distribution obtained by sampling $(x; y_{true})$ from $D$, computing an adversarial
example $x_{adv}$ for some model such that $\lVert x^{adv} - x \rVert_\infty \le \epsilon $ and outputting $(x_{adv}; y_{true})$. In
Ensemble Adversarial Training, the source distributions are $D$ (the clean data) and $A_1, \cdots, A_k$ (the
attacks overs the currently trained model and the static pre-trained models). The target distribution
takes the form of an unseen black-box adversary $A^*$. Standard generalization bounds for Domain
Adaptation.

<Blockquote>

<h3> Theorem [informal] </h3>
Let $h^* \in \mathcal{H}$ be a model learend with Ensemble Adversarial Training and static black=box adversaries $A_1, \cdots, A_k$. Then, if $h^*$ is robust against the black-box adversaries $A_1, \cdots, A_k$ used at training time, then $h^*$  has bounded error on attacks from a future black-box adversary $A^*$, if $A^*$ is not "much stronger", on average, than the static adversaries $A_1, \cdots, A_k$.
\end{theorem}

</Blockquote>