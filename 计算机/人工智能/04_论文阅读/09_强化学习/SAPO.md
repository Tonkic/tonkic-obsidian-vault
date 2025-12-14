**$\theta$ (Theta)**:The parameters of the autoregressive language model.

**$\pi_\theta$ (Pi)**:The stochastic policy representing the model over token sequences.

**$\mathcal{D}$ & $q$**:$\mathcal{D}$ is the query set (dataset); $q$ denotes a specific query.

**$y$**:A response sequence consisting of tokens. $|y|$ is the number of tokens (length).

The probability of generating a response $y$ given a query $q$ is factorized as the product of conditional probabilities at each step:
$$
\pi_{\theta}(y \mid q) = \prod_{t=1}^{|y|} \pi_{\theta}(y_t \mid q, y_{<t}) \tag{1}
$$

- **$y_t$**: The token generated at the current time step $t$ 
- **$y_{<t}$**: The history of tokens generated before step $t$ 



