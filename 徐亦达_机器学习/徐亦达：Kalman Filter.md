# 徐亦达：Kalman Filter

## Part 1

1. Review on dynamic model

   一系列的观测值是 series 而不是 set，其与 index 走向有关

   【在 paper 里面，图模型的观测值一般是灰色，而变量一般是白色】

   state-space model 核心思想：各个观测由隐状态得到，所以相互独立。【参考 HMM 隐马尔可夫模型】

   用 state-space model 描述需要三个概率：

   1. 各个隐状态之间的概率 $P(x_t\mid x_{t-1})$，称为 transition probability。
   2. 每个隐状态到观测值的概率 $P(y_t\mid x_t)$，称为 emission probability 或 measurement probability。
   3. 还有一个是初始的隐状态概率 $P(x_1)$，称 initial probability。

   ![image-20220624013605029](https://cdn.jsdelivr.net/gh/MazelTovy/Cloudimg@main/img/image-20220624013605029.png)

## Part 2

1. 三种常用的 dynamic models

   |                                                   | $P(x_t\mid x_{t-1})$                                         | $P(y_t\mid x_t)$                     | $P(x_1)$                                      |
   | ------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------ | --------------------------------------------- |
   | Discrete State DM (也就是 HMM)                    | $A_{x_{t-1},x_t}$<br />![image-20220624014343241](https://cdn.jsdelivr.net/gh/MazelTovy/Cloudimg@main/img/image-20220624014343241.png) | Any                                  | $\pi$                                         |
   | Linear Gaussian DM (包括Kalman Filter)            | $\mathcal{N}\left(Ax_{t-1}+B,Q\right)$                       | $\mathcal{N}\left(Hx_{t}+C,R\right)$ | $\mathcal{N}\left(\mu_0,\varepsilon_0\right)$ |
   | Non-linear Non-Gaussian DM (包括 Particle Filter) | $f(x_{t-1})$                                                 | $g(y_t)$                             | $f_0(x_1)$                                    |

   dynamic model 可以做
   
   1. evaluation
   
      $P(y_1\dots y_t)$
   
      用 backward-and-forward formula 可以大大简化计算
   
   2. parameter learning
   
      $\mathop{\mathrm{argmin}}\limits_{\theta}\log P\left(y_1\dots y_t\mid\theta\right).$ 
   
   3. filtering
   
      很多时候我们需要知道的是当前状态，也就是求 $P(x_t\mid y_1\dots y_t)$
   
1. 用途

   三种 DM 都可以用于这三个用途，对于 discrete state 来说，用于 evaluation 和 parameter learning 较多；对 linear Gaussian 和 non-linear non-Gaussian 来说，用于 filtering 较多。

## Part 3

1. 介绍 linear Gaussian

   转移概率
   $$
   P(x_t\mid x_{t-1})\sim\mathcal{N}(Ax_{t-1}+B,Q)
   $$
   和
   $$
   x_t=Ax_{t-1}+B+w
   $$
   是等价的。其中 $w$ 是噪声，有 $w\sim\mathcal{N}(0,Q)$。

   同样地，measurement probability
   $$
   P(y_t\mid x_{t})\sim\mathcal{N}(Hx_{t}+C,R)
   $$
   也可以写作
   $$
   y_t=Hx_{t}+C+v
   $$
   其中 $v$ 是噪声，有 $v\sim\mathcal{N}(0,R)$。

   所以线性高斯模型的参数包括 $A,B,Q,H,C,R$。

   取得这些参数即可用 linear Gaussian 建立比较完备的概率模型

## Part 4

1. linear Gaussian 参数制定的一个实例

   小车运动

   加速度为 $\ddot{x}=a\sim\mathcal{N}(0,v^2)$

   其状态矢量为 $\displaystyle\bar{x}_t=\begin{bmatrix}x_t\\ \dot{x}_t\end{bmatrix}$ 【位置和速度】

   由经典力学中的运动学方程
   $$
   \left\{
   \begin{aligned}
   x_t&=x_{t-1}+\dot{x}_{t-1}\Delta t+\frac{1}{2}a(\Delta t)^2\\
   \dot{x}_t&=\dot{x}_{t-1}+a\Delta t
   \end{aligned}
   \right.
   $$
   根据 linear Gaussian 列出矩阵乘式
   $$
   \underbrace{\begin{bmatrix}
   x_t\\
   \dot{x}_t
   \end{bmatrix}}_{\bar{x}_t}=
   \underbrace{\begin{bmatrix}
   1 & \Delta t\\
   0 & 1
   \end{bmatrix}}_A
   \underbrace{\begin{bmatrix}
   x_{t-1}\\
   \dot{x}_{t-1}
   \end{bmatrix}}_{\bar{x}_{t-1}}+
   \begin{bmatrix}
   \frac{1}{2}a(\Delta t)^2\\
   a\Delta t
   \end{bmatrix}
   $$
   其协方差矩阵为
   $$
   \begin{aligned}
   \text{Var}(\bar{x}_t)&=
   \text{E}\begin{bmatrix}
   (\bar{x}_t-\mu)\cdot(\bar{x}_t-\mu)^\top
   \end{bmatrix}\\
   &=\text{E}\begin{bmatrix}
   \begin{bmatrix}
   \frac{1}{2}a(\Delta t)^2\\
   a\Delta t
   \end{bmatrix}
   \begin{bmatrix}
   \frac{1}{2}a(\Delta t)^2 & a\Delta t
   \end{bmatrix}
   \end{bmatrix}\\
   &=\text{E}\left\{
   \begin{bmatrix}
   \frac{1}{4}a^2(\Delta t)^4 & \frac{1}{2}a^2(\Delta t)^3\\
   \frac{1}{2}a^2(\Delta t)^3 & a^2(\Delta t)^2
   \end{bmatrix}
   \right\}\\
   &=\underbrace{\text{E}\begin{bmatrix}
   a^2
   \end{bmatrix}}_{v^2}
   \begin{bmatrix}
   \frac{1}{4}(\Delta t)^4 & \frac{1}{2}(\Delta t)^3\\
   \frac{1}{2}(\Delta t)^3 & (\Delta t)^2
   \end{bmatrix}
   \end{aligned}
   $$
   对观测来说，同样也有类似对表达，只是其只能取得其位置，不能取得其速度，速度由均值计算而来，所以有噪声误差
   $$
   y_t=\begin{bmatrix}
   1 & 0
   \end{bmatrix}
   \begin{bmatrix}
   x_t\\
   \dot{x}_t
   \end{bmatrix}
   +u
   $$
   其中噪声 $u\sim\mathcal{N}(0,R)$

## Part 5

1. Kalman Filter

   对 Kalman Filter，有
   $$
   \begin{aligned}
   \overbrace{P(x_t\mid y_1,\dots,y_t)}^{update}&\propto P(x_t,y_1,\dots,y_t)\\
   &\propto P(y_t\mid x_t,y_1,\dots,y_{t-1})\cdot \overbrace{P(x_t\mid y_1,\dots,y_{t-1})}^{prediction}\\
   &=P(y_t\mid x_t)\cdot P(x_t\mid y_1,\dots,y_{t-1})\\
   &=P(y_t\mid x_t)\cdot\int_{x_{t-1}}P(x_t,x_{t-1}\mid y_1,\dots,y_{t-1})\dd x_{t-1}\\
   &=P(y_t\mid x_t)\cdot\int_{x_{t-1}}P(x_t\mid x_{t-1}, y_1,\dots,y_{t-1})\cdot P(x_{t-1}\mid y_1,\dots,y_{t-1})\dd x_{t-1}\\
   &=P(y_t\mid x_t)\cdot\int_{x_{t-1}}P(x_t\mid x_{t-1})\cdot P(x_{t-1}\mid y_1,\dots,y_{t-1})\dd x_{t-1}\\
   \end{aligned}
   $$
   最后被积分的后一个乘数 $P(x_{t-1}\mid y_1,\dots,y_{t-1})\sim\mathcal{N}(\hat{\mu}_{t-1},\hat{\Sigma}_{t-1})$

   因此 prediction $P(x_t\mid y_1,\dots,y_{t-1})\sim\mathcal{N}(\bar{\mu}_t,\bar{\Sigma}_t)$，对应被积分的后一个乘数，其实也就是上一组 update 的结果，这一组 update $P(x_t\mid y_1,\dots,y_t)\sim\mathcal{N}(\hat{\mu}_{t},\hat{\Sigma}_{t})$。

   也就是说，我们可以认为 Kalman Filter 在做的事情是
   $$
   \begin{aligned}
   t=1\rightarrow\ &\text{update}\ P(x_1\mid y_1)\sim\mathcal{N}(\hat{\mu}_1,\hat{\Sigma}_1)\\
   t=2\rightarrow\ &\text{prediction}\ P(x_2\mid y_1)\sim\mathcal{N}(\bar{\mu}_2,\bar{\Sigma}_2)\\
   &\text{update}\ P(x_2\mid y_1,y_2)\sim\mathcal{N}(\hat{\mu}_2,\hat{\Sigma}_2)\\
   \vdots\\
   t\rightarrow\ &\text{prediction}\ P(x_t\mid y_1,\dots,y_{t-1})\sim\mathcal{N}(\bar{\mu}_t,\bar{\Sigma}_t)\\
   &\text{update}\ P(x_t\mid y_1,\dots,y_t)\sim\mathcal{N}(\hat{\mu}_t,\hat{\Sigma}_t)\\
   \end{aligned}
   $$

2. summary

   ![image-20220625001330349](https://cdn.jsdelivr.net/gh/MazelTovy/Cloudimg@main/img/image-20220625001330349.png)

   【感叹：Richard 的板书真是 art 啊】

   小错误：右上正态分布里显然均值参数是 $\mu$ 而不是 $x$。

## Part 6

1. 补充说明

   因为噪声是独立的常数，所以显然有
   $$
   Cov(x_{t-1},w)=0\\
   Cov(x_{t-1},v)=0\\
   Cov(w,v)=0
   $$
   对一般的两个独立分布的高斯分布，其联合性质为
   $$
   \begin{pmatrix}
   u\\
   v
   \end{pmatrix}
   \sim\mathcal{N}
   \left(
   \begin{bmatrix}
   \mu_u\\
   \mu_v
   \end{bmatrix}\cdot
   \begin{bmatrix}
   \Sigma_u & \Sigma^\top_{u,v} \\
   \Sigma_{u,v} & \Sigma_v
   \end{bmatrix}
   \right)
   $$
   其条件概率符合
   $$
   P(u\mid v)\sim\mathcal{N}\left(\mu_u+\Sigma_{uv}\Sigma^{-1}_v(v-\mu_v),\Sigma_u-\Sigma_{uv}\Sigma^{-1}_v\Sigma^\top_{uv} \right)
   $$
   
2. 进一步推导 Kalman Filter

   我们前面 update 有了 $P(x_{t-1}\mid y_1,\dots,y_{t-1})\sim\mathcal{N}(\hat{\mu}_{t-1},\hat{\Sigma}_{t-1})$，其同样可以写成两部分
   $$
   x_{t-1}\mid y_1,\dots,y_{t-1}=\text{E}[x_{t-1}]+\Delta x_{t-1}
   $$
   其中 $\Delta x_{t-1}\sim\mathcal{N}(0,\hat{\Sigma}_{t-1})$。可以继续等价下去
   $$
   \begin{aligned}
   x_{t}\mid y_1,\dots,y_{t-1}&=A\left(x_{t-1}\right)+w\\
   &=A\left(\text{E}[x_{t-1}]+\Delta x_{t-1}\right)+w\\
   &=\underbrace{A\text{E}[x_{t-1}]}_{\text{E}[x_t]}+\underbrace{A\Delta x_{t-1}+w}_{\Delta x_t}\\
   \end{aligned}
   $$
   左边的 $\text{E}$ 是与随机变量无关的常数，右边的增量高斯分布均值为零。对 $y_t$ 同样有
   $$
   \begin{aligned}
   y_{t}\mid y_1,\dots,y_{t-1}&=Hx_{t}+v\\
   &=H\left(A\text{E}[x_{t-1}]+A\Delta x_{t-1}+w\right)+v\\
   &=\underbrace{HA\text{E}[x_{t-1}]}_{\text{E}[y_t]}+\underbrace{HA\Delta x_{t-1}+Hw+v}_{\Delta y_t}\\
   \end{aligned}
   $$
   也就是
   $$
   P\left(x_t\mid y_1,\dots,y_{t-1}\right)\sim\mathcal{N}\left(A\text{E}\left[x_{t-1}\right],\text{E}\left[(\Delta x)(\Delta x)^\top\right]\right)\\
   P\left(y_t\mid y_1,\dots,y_{t-1}\right)\sim\mathcal{N}\left(HA\text{E}[x_{t-1}],\text{E}\left[(\Delta y)(\Delta y)^\top\right]\right)
   $$
   

## Part 7

1. 联合分布
   $$
   P(x_t,y_t\mid y_1,\dots,y_{t-1})\sim\mathcal{N}\left(
   \begin{bmatrix}
   A\text{E}\left[x_{t-1}\right]\\
   HA\text{E}\left[x_{t-1}\right]
   \end{bmatrix},
   \begin{bmatrix}
   \text{E}\left[(\Delta x)(\Delta x)^\top\right]&\text{E}\left[(\Delta x)(\Delta y)^\top\right]\\
   \text{E}\left[(\Delta y)(\Delta x)^\top\right]&\text{E}\left[(\Delta y)(\Delta y)^\top\right]
   \end{bmatrix}
   \right)
   $$
   由 part5 推导的结果，结合 covariance 的性质
   $$
   \begin{aligned}
   \text{E}\left[(\Delta x)(\Delta x)^\top\right]&=\left(A\Delta x_{t-1}+w\right)\left(A\Delta x_{t-1}+w\right)^\top\\
   &=\left(A\Delta x_{t-1}+w\right)\left(\Delta x^\top_{t-1}A^\top+w^\top\right)\\
   &=A(\Delta x_{t-1})(\Delta x_{t-1})^\top A^\top+0+0+ww^\top\\
   &=A\text{E}\left[\Delta x_{t-1}\Delta x_{t-1}^\top\right]A^\top+\text{E}\left[ww^\top\right]\\
   &=A\hat{\Sigma}_{t-1}A^\top+Q
   \end{aligned}
   $$
   同样，其他几项推导类似。

   
