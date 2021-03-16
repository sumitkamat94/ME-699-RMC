@def class = "conference"
@def authors = "Marco, A.; Hennig, P.; Bohg, J.; Schaal S.; Trimpe S.;"
@def title = "Automatic LQR tuning based on Gaussian process global optimization"
@def venue = "2016 IEEE International Conference on Robotics and Automation (ICRA)"
@def year = "2016"
@def hasmath = true
@def review = true
@def tags = ["reviews","learning","control","analysis","optimization"]
@def reviewers = ["Sumit Suryakant Kamat"]

\toc
### Overview / Broad Area
Developing an accurate mathematical model of a physical dynamic system can be challenging, time-intensive, and expensive. Furthermore, the mathematical model of the physical dynamic system may not account for parametric uncertanities, non-linearities, unmodeled dynamics, and disturbances due to limitations of the sensors. If a controller is tuned without considering model uncertainities, then the controller might not achieve the required performance. This motivates the tuning of controller gains using experimental data, as it would reduce the effects of the above-mentioned uncertainities. 

However, tuning of controller gains using methods such as grid search is time-intensive and might require multiple experimental. This necessitates the development of efficient algorithms for obtaining controller gains.

This paper proposes a framework for tuning Linear Quadratic Regulator (LQR) gains by combining linear optimal control with Bayesian optimization. 

[//]: # (Bayesian Optimization involves solving optimization problems where the objective function is continuous but does not have a special structure for e.g.; concavity. Furthermore, the objective function does not give gradient information.)

### Notation
* $x_k$: discrete state, $\tilde{x}_k$: approximate discrete state
* $A_{\text{n}}$, $B_{\text{n}}$: Nominal plant parameters
* $u_k$: Control, $w_k$: noise
* $J$: Nominal Quadratic cost function
* $\hat{J}$: Approximate cost function
* $\mathcal{D} \sub \mathbb{R}^D$: Doman representing region around nominal design


### Problem Statement
Consider the non-linear discrete model $x_{k+1}=f(x_k,u_k,w_k)$, where $x_k \in \mathbb{R}^{n_x}$ and $u_k \in \mathbb{R}^{n_u}$ is the control. The non-linear discrete model has an equilibrium at $x_k=0$, $u_k=0$, and $w_k=0$. The non-linear system can be approximated as a linear discrete (nominal) model $\tilde{x}_{k+1}=A_n \tilde{x}_{k} +B_n u_k+w_k$ about the equilibirirum. 

The nominal cost function is given by $J=\lim_{K \to \infty} \frac{1}{K}\mathbb{E} [\sum_{k=0}^{K-1}x_k^TQx_k+u_k^TRu_k]$, where $Q$ and $R$ are positive-definite weigthing matrices. The objective is to determine the optimal control method which would minimize the cost function, while using data efficiently from fewer experiments. 

### Solution 
 
First, Marco et al. determine the state-feedback control for the non-linear system which would minimize the cost function $J$.
* LQR control given by $u_k=Fx_k$ minimizes the nominal cost function $J$; where $F=\text{lqr} (A_{n},B_{n},Q,R)$. 
* The controller gain can be parameterized as $F=\text{lqr} (A_{n},B_{n},W_x(\theta),W_u(\theta))$. It follows that $J=J(\theta)$. 

* The objective is to minimize $J(\theta)$ by varying the parameters $\theta$. In particular, the optimization problem is given by  $\text{arg} \ \text{min} \ J(\theta) \ \text{s.t.} \ \theta \in \mathcal{D}.$ In other words, the search-space for the parameters $\theta$ is restricted to $\mathcal{D}$. 

* However, note that the cost function $J$ cannot be determined from the experimental data as we observe with $J$ that it represents an infinite horizon problem. Hence, consider the approximate cost function $\hat{J}= \frac{1}{K} [\sum_{k=0}^{K-1}x_k^TQx_k+u_k^TRu_k]$.


### Comments
* The paper focuses on a single example of a path-following robot.
* For this method to work, we must know the state space dynamics and also have data that describes measurements obtained in a state. Both assumptions are impractical.
* The paper casts training as a bilinear optimization problem, which are often unreliable to solve. It's not clear if the optimization will succeed in other problems.
* The piecewise approach might be impractical in high dimensional systems

### Recent Papers
*Note: This section makes more sense for papers published 1-2 years ago or earlier, unlike this paper that is still in press.*
* The [BADGER system](https://bair.berkeley.edu/blog/2020/03/12/badgr/) seems to solve this specific problem of navigation from complex sensor data without state estimation using reinforcement learning. There is no stability analysis, however.
