# NLP - Homework 04

## LSTM prevents the vanishing gradient

Consider a hidden state $h_t$ at time step $t$. If we make things simple and remove biases and inputs, we have

$$
ht=\sigma(wh_{t−1})
$$

then we have

$$
\frac{\partial h_{t'}}{\partial h_{t}}= \omega^{t'-t}\Pi\sigma'(\omega h_{t'-k})
$$

Here we should pay attention to the $\omega^{t'-t}$ part. If the weight is not equal to 1, it will either decay to zero exponentially fast in $t′−t$, or grow exponentially fast.

In LSTMs, you have the cell state $s_t$. In this case, the derivative is

$$
\frac{\partial s_{t'}}{\partial s_{t}}= \Pi\sigma'(v_{t+k})
$$

As you can see, there is no exponentially fast decaying factor involved. Consequently, there is at least one path where the gradient does not vanish.




















