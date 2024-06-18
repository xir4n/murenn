Assumption: 

1. $\omega$ a conv1d filter with support $[-L,L]$​ and weights i.i.d normal variables: 

$$
\omega\sim \mathcal{N}(0,\sigma^2\text{I}_{2L+1})
$$

2. To have $\rVert\omega\lVert^2=1$, $\sigma^2$and $L$ should satisfy:
   $$
   \sigma^2(2L+1)=1
   $$

Conv1D's case:
$$
\begin{aligned}
\displaystyle\mathbb{E}[\sum_{t=-L}^Lt\omega(t)^2]
&=\sum_{t=-L}^Lt\mathbb{E}[\omega(t)^2]\\
&=\sum_{t=-L}^Lt\sigma^2\\
&=0

\end{aligned}
$$

$$
\begin{aligned}
\displaystyle\mathbb{E}[\sum_{t=-L}^Lt^2\omega(t)^2]
&=\sum_{t=-L}^Lt^2\mathbb{E}[\omega(t)^2]\\
&=\sigma^2\sum_{t=-L}^Lt^2\\
&=\sigma^2\frac{L(L+1)(2L+1)}{3}\\
&=\frac{L(L+1)}{3}
\end{aligned}
$$

Dilated Conv1D's case: 

The filter $\omega_d$ is defined by:

$$
\omega_d(n)=\displaystyle\sum_{t=-L}^{L}\omega(t)\delta(n-2^jt)
$$

Obviously  $\mathbb{E}[\displaystyle \sum_{t=-L}^{L}t\omega_d(t)^2]=0$, the time spread of the filter: 

$$
\mathbb{E}[\displaystyle\sum_{t=-2^jL}^{2^jL}t^2\omega_d(t)^2]
$$

By using the change of variable $t=2^j\tau$, we have :  

$$
\begin{aligned}
\mathbb{E}[\displaystyle\sum_{t=-2^jL}^{2^jL}t^2\omega_d(t)^2] 
&= \mathbb{E}[\displaystyle\sum_{\tau=-L}^{L}(2^j\tau)^2\omega_d(2^j\tau)^2]\\
&= \mathbb{E}[\displaystyle\sum_{\tau=-L}^{L}(2^j\tau)^2\omega(\tau)^2]\\
&= \mathbb{E}[\omega(\tau)^2]\displaystyle\sum_{\tau=-L}^{L}(2^j\tau)^2\\
&= 4^j\sigma^2\frac{L(L+1)(2L+1)}{3}\\
&= 4^j\frac{L(L+1)}{3}\\
\end{aligned}
$$

All Pass Dilated Conv1D's case: 

Assumption: 

1. $\omega_j$ the Conv1D Gaussian filter of level j
2. $J(2L+1)\sigma^2=1$​

Definition of the filter:

$$
\omega_{ad}(n)=\displaystyle\sum_{j=1}^{J}\sum_{t=-L}^L\omega_j(t)
\delta(n-2^jt)
$$

Time spread of the filter:  

$$
\begin{aligned}
&\mathbb{E}(\displaystyle\sum_{n=-2^{J}L}^{2^{J}L}n^2\omega_{ad}(n)^2)\\
&=\displaystyle\sum_{n=-2^JL}^{2^JL}n^2\mathbb{E}[\displaystyle\sum_{j=1}^J\sum_{j'=1}^J\sum_{t=-L}^L\sum_{t'=-L}^L\omega_j(t)\omega_{j'}(t')
\delta(n-2^jt)\delta(n-2^{j'}t')]\\
&=\displaystyle\sum_{n=-2^jL}^{2^jL}n^2\sum_{j=1}^J\sum_{t=-L}^L\sigma^2\delta(n-2^{j}t)\\
&=\displaystyle\sum_{j=1}^J\sum_{t=-L}^L(2^jt)^2\sigma^2\\
&=\sigma^2\displaystyle\sum_{j=1}^J4^j\sum_{t=-L}^Lt^2\\
&=\frac{4(4^J-1)L(L+1)}{9J}\\
\end{aligned}
$$

Comparison with Conv1d : 

The support of the Conv1D filter has to be $[-2^JL,2^JL]$ to have the same receptive field with $\omega_d$​​, the time spread becomes:  

$$
\displaystyle\mathbb{E}[\sum_{t=-2^JL}^{2^JL}t^2\omega(t)^2] = 4^J\frac{L(L+2^{-J})}{3}
$$

In this case,  

$$
\begin{aligned}
\frac{TL(\omega_{ad})}{TL(\omega)}
&=\frac{4(4^J-1)L(L+1)}{3J4^J(L(L+2^{-J}L))}\\
&\approx\frac{4}{3J}
\end{aligned}
$$



