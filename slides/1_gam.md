---
background: /mountain.jpg
layout: cover
hideInToc: true
---

# Generalized Additive Models

---

# Generalized Additive Models (GAM)

* Consider GAM: <br> $\mathbb{E} [Y_{N \times 1} | X_1, ..., X_p] = \alpha + f_1 (X_1) + ... + f_p (X_p)$
	* $f_j$ are smooth non-parametric functions
		* These are learnt simultaneously via **kernel** or **scatter plot smoothers**
			* The algorithm uses **backfitting** and **Newton-Raphson** algorithm
	* If $f_j (X_j) = \beta_j h_j (\bm{X}_{N \times p})$, i.e. a **basis expansion**, <br>then we can learn $\beta_j$ via **least squares**
		* where $h_j (\bm{X})$ can be $X_j$, $\log X_j$, $X_j^r$, $X_j X_k$, $I(X_j \in [L_j, U_j])$, ...
		* See [Ch.5](https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=158) for details

---

# Link Function

* Let the conditional mean be $\mu(X) := \mathbb{P}[Y = 1 | X]$ in binary classification
* A **link function** relates $\mu(X)$ to an additive function of predictors. Ex:
	* Logistic regression uses **logit link**: $\log \big(\frac{\mu(\bm{X})}{1 - \mu(\bm{X})} \big) = \alpha + \beta_1 X_1 + ... + \beta_p X_p$
	* Generalization of additive terms: $\log \big(\frac{\mu(\bm{X})}{1 - \mu(\bm{X})} \big) = \alpha + f_1 (X_1) + ... + f_p (X_p)$
	* Generalization of a link function: $g(\mu(\bm{X})) = \alpha + f_1 (X_1) + ... + f_p (X_p)$

<div class="grid grid-cols-[3fr,2fr]">
<div>

* Ex. of a link function, $g(\mu)$:
	* $\mu$ is identity link
  	* $\mathrm{logit}(\mu)$ or $\mathrm{probit}(\mu) = \Phi^{-1}(\mu)$
  		* where $\Phi := \mathcal{N} (0, 1)$, $\Phi^{-1}$ is the **quantile** (inverse) function of $\Phi$
	* $\log (\mu)$ log-linear model for Poisson response of counts
</div>
<div>
  <figure>
    <img src="/a-Cumulative-Distribution-Function-of-Probit-and-Logit-Models-b-Probability-Density.png" style="width: 260px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;">Image source:
	  <a href="https://www.researchgate.net/figure/a-Cumulative-Distribution-Function-of-Probit-and-Logit-Models-b-Probability-Density_fig1_265404717">https://www.researchgate.net/figure/a-Cumulative-Distribution-Function-of-Probit-and-Logit-Models-b-Probability-Density_fig1_265404717</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Fitting Additive Models: Smoothing Spline

* Additive model: $Y = \alpha + \sum_{j=1}^p f_j(X_j) + \varepsilon$, $\mathbb{E}\varepsilon = 0$
	* $f_j$ are cubic splines
* We minimize the **Penalized RSS** given a smoothing parameter $\lambda_j \geq 0$:
$$\mathrm{PRSS}(\alpha, f_1, ..., f_p | \bm{X}_{N \times p} ) = \sum_i \bigg[y_i - \alpha - \sum_j f_j (x_{ij}) \bigg]^2 + \sum_j \lambda_j \int f_j^{\prime\prime}(t_j)^2 dt_j$$	
* The solution is equivalent to a cubic spline with knots at each observation $X_i \in \mathbb{R}^p$
* The solution is not unique unless
	* we restrict $\sum\limits_i f_i (x_{ij}) = 0$, $\forall j$, which results in unique $\hat{\alpha} := \bar{y}$
	* $\bm{X}$ has a [full rank](https://en.wikipedia.org/wiki/Rank_(linear_algebra)#:~:text=The%20column%20rank%20of%20A,%3D%20row%20rank%2C%20below.)

---

# Backfitting Algorithm for Additive Models

[Backfitting Algorithm](https://en.wikipedia.org/wiki/Backfitting_algorithm) by Leo Breiman and Jerome Friedman:
<div class="bg-orange-100">

* Initialization: $\hat{\alpha} := \bar{y}$, $\hat{f}_j := 0$, $\forall j$
* Do until convergence:
	* For $j = 1:p$
		* $\hat{f}_j \leftarrow \mathcal{s}_j \bigg[ \big\{y_i - \hat{\alpha} - \sum_{k \neq j} \hat{f}_k (x_{ik}) \big\}_1^N \bigg]$
			* where $\mathcal{s}_j$ is the cubic smoothing spline fitted to $N$ residuals to estimate $f_j$
		* $\hat{f}_j \leftarrow \hat{f}_j - \frac{1}{N} \sum_i \hat{f}_j (x_{ij})$
			* where $\hat{f}_j := [\hat{f}_j (x_{ij})]_{\forall i}$
			* here we center $\hat{f}_j$ so that the function values average to $0$
* Convergence is reached when $\Delta \hat{f}_j < \tau$, $\forall j$ for some threshold $\tau$ <br> We can also exit the loop after a finite # of iterations
</div>