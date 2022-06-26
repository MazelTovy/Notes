# Modern Digital Signal Processing

## Introduction

1. Self-introduction

   张灏 13301383501 haozhang@tsinghua.edu.cn Rohn Building 11-105

   

2. Prerequisite

   Calculus

   Linear Algebra

   Probability

   **Statistics is very different from probability theory**

   

3. Textbooks

   1. Fundamental of Statistical Signal Processing, by S.M. Kay

      very easy to read, comprehend, and is recommended

   2. Adaptive Filter Theory, by Haykin

      Since it has published the fifth edition, it's highly recommended

   3. Spectral Analysis of Signal, by Stoica and Moses

   All in all, it's **Linear Estimation + Adaptive Filtering + Power Spectrum Analysis**.

   

4. Mottos

   **No Reading, No Learning.** College learning is different from high school.

   **No Writing, No Reading.** Focus on formulas and notations in one paper or one book, that's the key of scientific research, **effective reading**.

   **No Data, No Truth.** Modern DSP is similar to Machine Learning.

   **No Analytic, No Understanding.** That's the same to reading.

   **No Programming, No Cognition.** MATLAB or Python or R is quite ok.

   

5. Review on probability

   The kernel of probability is uncertainty.

   1. Concepts

      Statistical Experiment $\rightarrow$ Sample Space $\Omega$ $\rightarrow$ Event (Subset of $\Omega$) $\rightarrow$ Probability

      Two form of Sample Space: 

      1. Discrete (Countable)
      2. Continuous ($\mathbb{R}$)

      The standard definition of probability is not mapping all the events to the $[0,1]$ range, since there are some probability can't be presented in $\mathbb{R}$. It should actually be:

      $B\rightarrow [0,1]$ .

      

      So, $P(\emptyset)=0, P(\Omega)=1$, and when $A_i\cap A_j=\emptyset(i\ne j)$, $P(\bigcup\limits_{k=1}^{\infty}A_k)=\sum\limits_{k=1}^{\infty}P(A_k)$ . They're addable if they are countable and have no intersection.

      

      **The most important triple in probability theory: $(\Omega,B,P)$**

      The possibility is prescribed, so statistics is extremely different from probability.

      

   2. Probability vs. Statistic

      The **triple** upon is the model of possibility.

      Probability: Model $\rightarrow$ Decision

      Statistic: Data $\rightarrow$ Model

      If the model can't be generated from data easily, recently we have a new way to skip the model to cope with that, which is called **Big Data** or **Machine Learning**.

      
   
      Example 1: Choose a chord in a circle **randomly**, what's the probability that the length of the chord is greater than the side length of a regular triangle inscribed in the round.![image-20220411120346015](https://cdn.jsdelivr.net/gh/MazelTovy/Cloudimg@main/img/202204111210347.png)
      
      That's a good example to show that probability depend on how we choose sample space. If we predetermine one point of the chord, let the other point move, than the sample space will be its **circumference**, which will result in $\displaystyle\frac{1}{3}$. If we take the whole **area** as sample space, what we need to determine is the middle point of the circle. Since if the middle point is determined, the chord is determine. And from that we see the probability is $\displaystyle\frac{round\ inscribed\ in\ the\ triangle}{round\ outside}$, which is $\displaystyle\frac{1}{4}$. Prof said they are all right answers.
      
      
      
      Conclusion: **Sample space is the very foundation of probability**.
      
      
      
      Example 2: $n$ hats, $n$ persons, one match to one, but worn randomly. How many persons have the right hats?
      
      
      
      First we determine the sample space as a set as below:
      $$
      \Omega=\{n_1\ n_2\cdots n_n, n_2\ n_1\cdots n_n,\cdots,n_n\ n_{n-1}\cdots n_2\ n_1\}
      $$
      There're $n!$ elements in the set. To calculate every $P(N=i)$ is difficult, since one's behavior will influence another.
      
      Assume $A_k\le\Omega$, which represent the set that $k^{th}$ is matched. (That is pretty much like Dynamic Programming.) So:
      $$
      \{N=0\}=\overline{A_1}\cap\overline{A_2}\cap\cdots\cap\overline{A_n}=\overline{A_1\cup A_2\cup\cdots\cup A_n}
      $$
      Therefore the probability is
      $$
      P(N=0)=P(\overline{A_1\cup A_2\cup\cdots\cup A_n})=1-P(A_1\cup A_2\cup\cdots\cup A_n)
      $$
      From the inclusive-exclusive principle, we can deduce
      $$
      \begin{align}
      P(N=0)&=1-\left(\sum\limits_{k=1}^{n}P(A_k)-\sum\limits_{i\ne j}P(A_i\cap A_j)+\sum\limits_{i\ne j\ne k}P(A_i\cap A_j\cap A_k)\cdots\right)\\
      &=1-\left(n\cdot\frac{(n-1)!}{n!}-{n \choose 2}\cdot\frac{(n-2)!}{n!}+{n \choose 3}\cdot\frac{(n-3)!}{n!}\cdots\right)\\
      &=1-\left(1-\frac{1}{2!}+\frac{1}{3!}\cdots\right)
      \end{align}
      $$
      
   3. Review on set
   
      1. Independence
   
         $P(A\cap B)=P(A)\cdot P(B)$
   
         Extension: $\displaystyle P(\bigcup\limits_{k=1}^nA_k)=\prod\limits_{k=1}^nP(A_k)$
   
         An important point: $A$ and $B$ are independent $\ne$ $A\cap B=\emptyset$
   
      2. Conditional
   
         $P(B\mid A)=\displaystyle\frac{P(A\cap B)}{P(B)}$
   
         Conditional probability is the change of sample space.
   
      3. Addition (Total Probability)
   
         $\displaystyle P(A)=\sum\limits_{k=1}^nP(A\mid B_k)P(B_k)$
   
         
   
         Bayes: $\displaystyle P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}$
   
         
   
         Example for Bayes: $2$ urns. one urn: $m$ red, $n$ black; another urn: $n$ red, $m$ black
   
         Unknow which urn. If pick up a red, what's the probability of a second pick in red $P(r_2\mid r_1)$?
   
         Since $P(AB\mid C)=P(A\mid BC)P(B\mid C)$, then
         $$
         \begin{align}
         P(r_2\mid r_1)&=P(r_2\mid U_1\ r_1)P(U_1\mid r_1)+P(r_2\mid U_2\ r_1)P(U_2\mid r_1)\\
         &=P(r_2\ U_1\mid r_1)+P(r_2\ U_2\mid r_1)\\
         &=\frac{m}{m+n}\cdot \frac{P(r_1\mid U_1)P(U_1)}{P(r_1)}+\frac{n}{m+n}\cdot \frac{P(r_1\mid U_2)P(U_2)}{P(r_1)}\\
         &=\frac{m}{m+n}\cdot \frac{\displaystyle\frac{m}{m+n}\cdot\frac{1}{2}}{\displaystyle\frac{1}{2}}+\frac{n}{m+n}\cdot \frac{\displaystyle\frac{n}{m+n}\cdot\frac{1}{2}}{\displaystyle\frac{1}{2}}\\
         &=\frac{m^2+n^2}{(m+n)^2}
         \end{align}
         $$
         If $m\ne n$, we can easily find out that $P(r_2\mid r_1)$ is absolutely greater than $\displaystyle\frac{1}{2}$. This is marvelous. You see, from the Bayes formula, if you have known the ball you picked up is **red**, your **bias** will lead you to believe more that the next one is **red**, too. The more urn is, the more you will **believe** you're holding the urn that has more red.
   
   4. Random Variable
   
      1. Notation
   
         Errr, what is this notation?![image-20220411140353959](https://cdn.jsdelivr.net/gh/MazelTovy/Cloudimg@main/img/202204111403103.png)
   
         Oh, it turns out only $Z$, my misunderstanding : )
      
         Random variable has no connect with **"random"**. It's only a function that reflect sample space to the real number field. $\Omega\rightarrow \mathbb{R}$ It's ensured.
      
         The meaning of random variable is to do quantization on sample space $\Omega$.
      
         
      
      2. Description
      
         Discrete: $P(Z=x_k)=P(\{\omega:Z(\omega)=x_k\}),P(Z\in A)=\sum\limits_{x_k\in A}P(Z=x_k)$
      
         Continuous: since for every single $x$, $P(Z=x)=0$, we only use function $f_Z(X)$, such that$\displaystyle P(Z\in A)=\int_Af_Z(x)dx$.
      
         It's complete and coherent in both discrete cases and continuous cases.
      
         
      
      3. Examples
      
         1. Discrete examples
      
            Bernoulli: $P(Z=0)=p,P(Z=1)=1-p$
      
            
      
            Binomial: $\displaystyle P(Z=k)={n \choose k}p^k(1-p)^{n-k}$ (Model of shooting)
      
            
      
         	Poisson: $\displaystyle P(Z=k)=\frac{\lambda^k}{k!}e^{-\lambda}$
         	
         	​	From Binomial to Poisson: let $p\rightarrow 0,n\rightarrow\infty,np=\lambda$, so 
         	
         	$$
         	\begin{align}
      		\displaystyle {n \choose k}p^k(1-p)^{n-k}&=\frac{n!}{k!(n-k)!}\left(\frac{\lambda}{n}\right)^k\left(1-\frac{\lambda}{n}\right)^{n-k}\\
         	&\stackrel{n\rightarrow\infty}{\longrightarrow}\frac{\lambda^k}{k!}\cdot e^{-\lambda}
         	\end{align}
         	$$
         	
         2. Continuous examples
         
            Uniform: $\displaystyle U[a,b], f_Z(x)=\frac{1}{b-a}I_{[a,b]}(x)$, the $I$ is called **indicator**, which means
            $$
            I_A(x)=
            \left\{
            \begin{aligned}
            1,\quad x\in A\\
            0,\quad x\overline\in A\\
            \end{aligned}
            \right.
            $$
            
            Exponential: $f_Z(x)=\lambda e^{-\lambda x}I_{[0,\infty]}(x)$. It has no memory, because $P(Z>x+y\mid Z>x)=P(Z>y)$
            
            ​	Proof: $\displaystyle\frac{P(Z>x+y,Z>x)}{P(Z>x)}=\frac{P(Z>x+y)}{P(Z>x)}$, and $\displaystyle P(Z>a)=\int_a^\infty f_Z(x)dx=\int_a^\infty \lambda e^{-\lambda x}dx=e^{-\lambda a}$, so$\displaystyle\frac{P(Z>x+y,Z>x)}{P(Z>x)}=\frac{e^{-\lambda(x+y)}}{e^{-\lambda x}}=e^{-\lambda y}$
            
            
            
            Gaussian: $N(\mu,\sigma), \displaystyle f_Z(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$
            
            1. Average
               $$
               \begin{align}
               E(Z)&=\int_{-\infty}^{+\infty}xf_Z(x)dx\\
               &=\sum\limits_kx_kP(Z=x_k)
               \end{align}
               $$
               It has some important features, such as:
            
               
            
               Linear: $\displaystyle E(\sum\limits_{k=1}^nx_k)=\sum\limits_{K=1}^nE(Z_k)$
            
               Expect: $E(N)=\sum\limits_{k=1}^nE(Z_k)$
            
               If $f_Z(x), Y=g(Z)$, There is a **magic** formula:
               $$
               E(G(Z))=\int_{-\infty}^{+\infty}G(x)f_Z(x)dx
               $$
               Example:
            
               Assume $Z\sim U[0,2\pi]$, then $\displaystyle E(sinZ) = \int_0^{2\pi}\frac{1}{2\pi}sin(\theta)d\theta=0$



