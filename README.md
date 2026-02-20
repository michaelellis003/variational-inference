# Variational Inference

Coordinate Ascent Variational Inference (CAVI) for a univariate normal model with unknown mean and variance, implemented in R. Based on the framework from Blei, Kucukelbir & McAuliffe (2017).

## Model

Bayesian inference for the conjugate normal model:

```
y_i   ~ N(mu, sigma^2)       i = 1, ..., N
mu    ~ N(mu_0, sigma_0^2)
sigma^2 ~ InvGamma(alpha_0, beta_0)
```

The mean-field variational family factorizes the posterior as:

```
q(mu, sigma^2) = q(mu) * q(sigma^2)
```

where `q(mu)` is Normal and `q(sigma^2)` is Inverse-Gamma, with parameters updated via coordinate ascent to maximize the Evidence Lower Bound (ELBO).

## Dependencies

```r
install.packages(c("ggplot2", "patchwork", "dplyr"))
```

## Usage

```r
source("univariate-normal.R")

# Simulate data
X <- rnorm(20, mean = 100, sd = 15)

# Run CAVI
output <- vi(X, max_iter = 10)

# Approximate posterior parameters
output$mu_q_mu         # posterior mean of mu
output$sigmasq_q_mu    # posterior variance of mu
output$A_q_sigmasq     # shape of q(sigma^2)
output$B_q_sigmasq     # rate of q(sigma^2)
output$elbos           # ELBO trace
```

See [`main.R`](main.R) for a complete example with ELBO convergence and approximate posterior density plots.

## Mathematical Derivation

The full derivation of the optimal variational densities and ELBO is in [`Markdown/variational-inference.pdf`](Markdown/variational-inference.pdf).

## References

- Blei, D. M., Kucukelbir, A., & McAuliffe, J. D. (2017). Variational inference: A review for statisticians. *Journal of the American Statistical Association*, 112(518), 859-877.
- Ormerod, J. T., & Wand, M. P. (2010). Explaining variational approximations. *The American Statistician*, 64(2), 140-153.
