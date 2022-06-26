## Review on probability and statistics

1. Statistics

   Data $\rightarrow$ Model

   Assume random variable: $x_1,x_2,\cdots,x_n$ are independent samples that follow identical distribution, usually represent as $i, i, d$.

   The basic type of $f(X,\theta)$ is known, but the parameters are unknown.

   Let $x=(x_1,x_2,\cdots,x_n)^T$, so what we need to do is to estimate $\theta$ from sample $x$, which is represented as $\hat\theta$. And $\hat\theta=\hat\theta(x)$, since $x$ is random variable, $\hat\theta$ is also random. It is used as an **approximation**, but **not parameters** themselves.

   

   There's no lack of possibility that the result of estimator has no connect with sample.

   Example 1: If $x_1, x_2\stackrel{i,i,d}{\sim} \mathcal{N}(\theta,1)$, which means they generalize Gaussian Distribution, let assume $\hat\theta=x_1-x_2$, since $x_1-x_2\sim \mathcal{N}(0,2)$, it apparently does not reflect anything related with the parameter. This kind of **useless** estimator is called **Ancillary**.

   

   Example 2: If $x_1,\cdots,x_n\stackrel{i,i,d}{\sim}\mathcal{U}(0,\theta)$, we make two estimators: 
   $$
   \hat\theta=max(x_1,x_2,\cdots,x_n)\\
   \hat\theta=min(x_1,x_2,\cdots,x_n)
   $$
   Apparently the $max$ can reflect more information. We can also calculate the **Order Statistic** to prove it.
   $$
   \begin{align}
   F_{\theta_1}(X)&=P(\theta_1\le X)\\&=P(max(x_1,\cdots,x_n)\le X)\\&=P(x_1\le X_1,\cdots,x_1\le X_n)\\&=\prod_{k=1}^{n}P(x_k\le X)\\&=(F(X))^n
   \end{align}
   $$
   $\displaystyle f_{\theta_1}(X)=\frac{\dd}{\dd X}F_{\theta_1}(X)=n(F(X))^{n-1}f(X)$

   Similarly, we can have

   $F_{\theta_2}(x)=1-P(min(x_1,\cdots,x_n)>X)=1-(1-F(X))^n$

   $f_{\theta_2}(X)=n(1-F(X))^{n-1}f(X)$

   Since $\displaystyle f(X)=\frac{1}{\theta}, F(X)=\frac{X}{\theta}$, then $\displaystyle f_{\theta_1}(X)=\frac{n}{\theta}(\frac{X}{\theta})^{n-1},f_{\theta_2}(X)=\frac{n}{\theta}(1-\frac{X}{\theta})^{n-1},X\in [0,\theta]$.

   When $X\rightarrow \theta$, $f_{\theta_1}(X)$ gets nearer to side $\theta$, which is a better estimator. 

   

   So, is there an estimator that can satisfy all of the random variable which need to guess the parameter(s) $\theta$ ? The answer is **YES**.

   

   Sufficient Statistics (R. A. Fisher)

   An estimator/a statistic $T=T(x)$ is sufficient $\iff$ $f(X,\theta\mid T)$ is independent of $\theta$.

   Example: If $x_1,\cdots,x_n\stackrel{i,i,d}{\sim}B(N,\theta)$, $\theta$ is unknown, $T(x_1,\cdots,x_n)=\sum\limits_{k=1}^nx_k$.

   Proof: $\displaystyle f_{x_1,\cdots,x_n}(X_1,\cdots,X_n,\theta)=\prod\limits_{k=1}^n{N \choose X_k}\theta^{X_k}(1-\theta)^{N-X_k}$
   $$
   \begin{align}
   \displaystyle f_{x_1,\cdots,x_n}(X_1,\cdots,X_n,\theta\mid T=t)&=\frac{P(x_1=X_1,\cdots,x_n=X_n,T=t)}{P(T=t)}\\
   &=\frac{\displaystyle\prod_{k=1}^n{N \choose X_k}\theta^{X_k}(1-\theta)^{N-X_k}}{\displaystyle{nN\choose t}\theta^t(1-\theta)^{N-t}}\\
   &=\frac{\displaystyle\left[\prod_{k=1}^n{N \choose X_k}\right]\theta^{\sum\limits_{k=1}^nX_k}(1-\theta)^{nN}(1-\theta)^{-\sum\limits_{k=1}^nX_k}}{\displaystyle{nN\choose t}\theta^t(1-\theta)^{nN}(1-\theta)^{-t}}
   \end{align}
   $$
   Since $t=\sum\limits_{k=1}^nX_k$, $f_{x_1,\cdots,x_n}(X_1,\cdots,X_n,\theta\mid T=t)=\displaystyle \frac{\displaystyle\prod_{k=1}^n{N\choose X_k}}{\displaystyle {nN\choose t}}$, which is not related with $\theta$.

   So, $T=\sum\limits_{k=1}^nX_k$ is sufficient.

   

   Is there anyway we can present sufficient statistics in a pattern? Obviously, **YES**.

   

   Neyman Factorization

   If we can factorize $f(X,\theta)=g(T(X),\theta)h(X)$, then $T$ is sufficient. (It's quite hard to prove, but we can easily handle that.)

   For example, in the former case, we have
   $$
   \displaystyle\prod_{k=1}^n{N \choose X_k}\theta^{X_k}(1-\theta)^{N-X_k}=\displaystyle\left[\prod_{k=1}^n{N \choose X_k}\right]\theta^{\sum\limits_{k=1}^nX_k}(1-\theta)^{nN-\sum\limits_{k=1}^nX_k}
   $$
   Here, $\displaystyle\prod_{k=1}^n{N \choose X_k}\theta^{X_k}(1-\theta)^{N-X_k}$ is related both with $(X,\theta)$. After the factorization, $\displaystyle\prod_{k=1}^n{N \choose X_k}$ is only related with $X$, and $\theta^{\sum\limits_{k=1}^nX_k}(1-\theta)^{nN-\sum\limits_{k=1}^nX_k}$ is the function of $T(X)$. It's easy to have this pattern to prove the factorization to be sufficient, hence it's easy to say the left side is also sufficient.

   

   Example: Let $x_1,\cdots,x_n\stackrel{i,i,d}{\sim}\mathcal{N}(\theta,1)$. Then $f(x,\theta)=\displaystyle\prod_{k=1}^n\frac{1}{\sqrt{2n}}\exp(-\frac{(X_k-\theta)^2}{2})$.

   Follow the Neyman Factorization, we have
   $$
   \begin{align}
   f(x,\theta)&=\displaystyle\left(\frac{1}{\sqrt{2n}}\right)^n\exp\left(-\frac{1}{2}\sum_{k=1}^n(X_k-\theta)^2\right)\\
   &=\displaystyle\left(\frac{1}{\sqrt{2n}}\right)^n\exp\left(\left(\sum_{k=1}^nX_k\right)\theta-\frac{n}{2}\theta^2\right)\exp\left(-\frac{1}{2}\sum_{k=1}^nX_k^2\right)\\
   
   \end{align}
   $$
   Therefore, $T(x_1,\cdots,x_n)=\sum\limits_{k=1}^nx_k$.

   

   It's not enough to have sufficient statistics, we need to have **target**. We should hold **unbiasness**.

   Unbiasness means when $\hat\theta(x_1,\cdots,x_n)\rightarrow\theta$, $E(\hat\theta)=\theta$.

   Example 1: Let $x_1, \cdots, x_n\stackrel{i,i,d}{\sim}N(\theta,1)$. If we do Neyman Factorization, we have $\hat\theta(x_1,x_2)=x_1+x_2$, so $E(\hat\theta)=2\theta$. But $2\theta$ is not an approach of $\theta$. We need to make adjustments. Let $\displaystyle\hat\theta(x_1,x_2)=\frac{x_1+x_2}{2}$ is ok.

   

   Example 2: $x_1,\cdots,x_n\stackrel{i,i,d}{\sim}\theta+\varepsilon_k$. And $\varepsilon_k$ is random variable. $E(\varepsilon_k)=0, Var(\varepsilon_k)=\sigma^2$.

   Usually, we use $\displaystyle\theta=\frac{1}{n}\sum_{k=1}^nx_k$ to overcome the influence of noise $\varepsilon_k$.

   To calculate the noise, we can use $\displaystyle\hat\sigma^2=\frac{1}{n-1}\sum_{k=1}^n\left(x_k-\hat\theta\right)^2$.

   

