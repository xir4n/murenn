
## Receptive field MuReNN_AP

#### Definition of the problem

The structure of Murenn_AP:  

$$
x\stackrel{wt}{\longrightarrow}A\stackrel{conv1d}{\longrightarrow}B\stackrel{iwt}{\longrightarrow}y
$$

With: 

+ x: the input 1d signal
+ wt: wavelet transform
+ conv1d: random filters with i.i.d. gaussian distributed weights
+ iwt: inverse wavelet transform

Assume all the operations are continuous. Denote the input signal at time t as $x_{t}$, the center point of the output signal as $y_{0}$, our goal is to calculate the gradient $\frac{\partial y_{0}}{\partial x_{t}}, \forall t$. 

Key points to note: 

1. Since the conv1d filters are random, the result is a random vector. 
2. The system is linear, so the result is independent of the values of $\{x_t\}$.

#### Conclusion

Splitting the system into three operations

1. $x\stackrel{wt}{\longrightarrow}A$:  
   
$$
A_{su}=a(u, s)=\int_{- \infty}^{+\infty}x(t)\overline{\psi_s}(t-u)dt\\
\Rightarrow\frac{\partial a(u, s)}{\partial x(t)}=\overline{\psi_s}(t-u)
$$

2. $A\stackrel{conv1d}{\longrightarrow}B$:
   
$$
B_{su'} = b(u',s)=\int_{- \infty}^{+\infty}a(u,s)\omega_s(u'-u)du \\
\Rightarrow \frac{\partial b(u',s)}{\partial a(u,s)} =\omega_s(u'-u)
$$

3. $B\stackrel{iwt}{\longrightarrow}y$:  
   
$$
y(u'')=\frac{1}{C_{\psi}}\int_0^{s_0}\int_{-\infty}^{+\infty}b(u',s)\psi_s(u''-u')du'\frac{ds}{s^2}\\
y(0)=\frac{1}{C_{\psi}}\int_0^{s_0}\int_{-\infty}^{+\infty}b(u',s)\psi_s(0-u')du'\frac{ds}{s^2}\\
\Rightarrow\frac{\partial y(0)}{\partial b(u',s)}=\frac{\psi_s(-u')}{s^2}
$$

According to the chain rule,  

$$
\begin{align}
\frac{\partial y_0}{\partial x_t}
&=\frac{1}{C_{\psi}}\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}\int_{0}^{+\infty}\frac{1}{s^2}\psi_s(-u')\omega_s(u'-u)\overline{\psi_s}(t-u)dudu'ds\\
\end{align}
$$

By changing the variable: $\tau=u-u'\Rightarrow d\tau=du, t-u=t-\tau-u'$  

$$
\begin{align}
\Rightarrow \frac{\partial y_0}{\partial x_t}
&=\frac{1}{C_{\psi}}\int_{0}^{+\infty}\frac{1}{s^2}\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}\psi_s(-u')\overline{\psi_s}(t-\tau-u') \omega_s(-\tau)du' d\tau ds\\
&=\frac{1}{C_{\psi}}\int_{0}^{+\infty}\frac{1}{s^2}\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty}\psi_s(-u')\psi_s^*(u'+\tau-t) \omega_s(-\tau)du' d\tau ds\\
&=\frac{1}{C_{\psi}}\int_{0}^{+\infty}\frac{1}{s^2}\int_{-\infty}^{+\infty} (\psi_s * \psi_s^* (\tau-t))\omega_s(-\tau) d\tau ds\\
&=\frac{1}{C_{\psi}}\int_{0}^{+\infty}\frac{1}{s^2}\psi_s*\psi_s^* *\omega_s(t)ds\\
\end{align}
$$
