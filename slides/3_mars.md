---
background: /mountain.jpg
layout: cover
hideInToc: true
---

# MARS

---

# Multivariate Adaptive Regression Splines (MARS)

<div class="grid grid-cols-[7fr,1fr]">
<div>
<br>

* Developed by Jerome H. Friedman in 1991 (Stanford)
	* Algorithm is copyrighted, but a similar algorithm called `py-earth` <br> is [implemented in Python](https://github.com/scikit-learn-contrib/py-earth)

</div>
<div>
    <figure>
    <img src="/Friedman.png" style="width: 100px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Jerome H. Friedman
    </figcaption>
  </figure>
</div>
</div>
<br>

<div class="grid grid-cols-[2fr,2fr] gap-7">
<div>
<br>

* Automatic detection of nonlinearities and feature interactions

* Great for high dimensional feature space

* A generalization of stepwise linear regression

* Greedy search for knots and basis functions
</div>
<div>
    <figure>
    <img src="/MARS.png" style="width: 360px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;">Image source:
      <a href="https://bradleyboehmke.github.io/HOML/mars.html">https://bradleyboehmke.github.io/HOML/mars.html</a>
    </figcaption>
  </figure>
</div>
</div>

---

# MARS: Basis Functions

* Basis Functions: $\mathcal{C} := \bigg\{\color{grey}\underbrace{\color{#F7965A} (x-t)_{+} \color{#006}, (t-x)_{+}}_{\mathrm{~reflected~pair~of~hinge}} \color{#006} \bigg\}_{t \in \{x_{ij}\}_{\forall i,j}}$
	* where $x_{+} := \max(0, x)$, i.e. any negative value is zeroed out
	* it’s a set of linear splines
	* for all distinct inputs, we have $\mathcal{C} = 2 N p$

<div class="grid grid-cols-[5fr,3fr] gap-3">
<div>
<br>
<br>

* The model:<br> $f(X) := \beta_0 + \sum\limits_{m=1}^M \beta_m h_m (X)$
	* where $h_m$ is hinge or a product of 2+ hinges from $\mathcal{C}$
	* $\beta_m$ are estimated by minimizing least squares
</div>
<div>
<br>
<br>
    <figure>
    <img src="/ESL_figure_9.9.png" style="width: 400px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; left: 180px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=341">ESL Fig. 9.9</a>
    </figcaption>
  </figure>
</div>
</div>

---

# MARS: Greedy Algorithm

<div class="grid grid-cols-[4fr,3fr]">
<div>

* Init model to contain only a constant basis function (b.f.), i.e. $\mathcal{M} := \{h_0 = 1\}$
* At each step we have a model with $\{h_{\ell}\}$ b.f.
	* Find $h_m \in \mathcal{C}$ s.t.<br> $\hat{\beta}_{M+1} h_{\ell}(X) \cdot (X_j - t)_{+} + \hat{\beta}_{M+2} h_{\ell}(X) \cdot (t - X_j)_{+}$ yields largest drop in train RSS
		* where $\hat{\beta}_{M+1}$ are estimated via least squares
* Continue until $\mathcal{M}$ reached the specified size
* **Backward deletion**:
	* Remove basis functions from $\mathcal{M}$ yielding smallest increase in RSS
		* This should be possible because functions were added in greedy manner
</div>
<div>
    <figure>
    <img src="/ESL_figure_9.10.png" style="width: 450px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px; position: relative; left: 180px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=342">ESL Fig. 9.10</a>
    </figcaption>
  </figure>
</div>
</div>

---
layout: two-cols
---

# MARS: Pros and Cons

#### Cons:
* vs. Trees: MARS is slower
* Confidence intervals are difficult to estimate (as is typical of any nonparametric method)
* vs. Boosted Trees: MARS offers poorer fit, but is faster to train and is more interpretable

::right::

#### Pros:
* vs. Linear Regression:
	* MARS is more flexible
	* Needs minimal data preparation
	* Limited influence from outliers (only on outer lines)
* vs. Linear Splines:
	* MARS automatically finds knots (vs. hand-picked knots in regression splines)
* Simple to understand
* Handles continuous and categorical features
* Tends to offer reasonable bias-variance tradeoff

---

# Hierarchical Mixture of Experts (HME)

<div class="grid grid-cols-[5fr,3fr]">
<div>

* Similar to CART, but 
	* Splits are **probabilistic** (i.e. “soft”) at each **gating network** (=internal node)
		* At each node the branch is picked based on observation values
	* We fit a **regression** (not just constant) at each **expert** (=terminal node)
		* Experts provide an **opinion** (=prediction)
	* Allows **multiway** $K$ splits with the probability<br> of $x$ in $j$th branch as $g_j (x_{p \times 1}, \gamma_{j, p \times 1}) := \frac{e^{\gamma^{\prime}k^x}}{\sum\limits_{k=1}^K e^{\gamma^{\prime}k^x}}$
		* where $\gamma_j$ is unknown, and $j = 1:K$

</div>
<div>
    <figure>
    <img src="/ESL_figure_9.13.png" style="width: 400px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=349">ESL Fig. 9.13</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Note on Missing Data

* Let $\bm{X}_{N \times p}$ be a data matrix with observed entries as $\bm{X}_{\mathrm{obs}}$ and response vector $\bm{y}_N$ 
* Let $\bm{Z} = (\bm{y}, \bm{X})$ and $\bm{Z}_{\mathrm{obs}} := (\bm{y}, \bm{X}_{\mathrm{obs}})$
* $\bm{R} = \{0, 1\}_{N \times p}$ is an indicator matrix with $1$ where $\bm{X}$ as missing value
* Three cases:
	1. Data is **missing completely at random (MCAR)** 
	2. Data is **missing at random (MAR)**
	3. Data is **missing not at random (MNAR)**
	
---

# Note on Missing Data

* Let $\bm{X}_{N \times p}$ be a data matrix with observed entries as $\bm{X}_{\mathrm{obs}}$ and response vector $\bm{y}_N$ 
* Let $\bm{Z} = (\bm{y}, \bm{X})$ and $\bm{Z}_{\mathrm{obs}} := (\bm{y}, \bm{X}_{\mathrm{obs}})$
* $\bm{R} = \{0, 1\}_{N \times p}$ is an indicator matrix with $1$ where $\bm{X}$ as missing value
* Three cases:
	* Data is **missing completely at random (MCAR)** if the distribution of $\bm{R}$ doesn’t depend on the observed or missing data: $\mathbb{P}_{\theta}[\bm{R}|\bm{Z}] = \mathbb{P}_{\theta}\bm{R}$
		* Missingness of values is unrelated to any variable
		* A very strong and unrealistic assumption, but is often used by models

---

# Note on Missing Data

* Let $\bm{X}_{N \times p}$ be a data matrix with observed entries as $\bm{X}_{\mathrm{obs}}$ and response vector $\bm{y}_N$ 
* Let $\bm{Z} = (\bm{y}, \bm{X})$ and $\bm{Z}_{\mathrm{obs}} := (\bm{y}, \bm{X}_{\mathrm{obs}})$
* $\bm{R} = \{0, 1\}_{N \times p}$ is an indicator matrix with $1$ where $\bm{X}$ as missing value
* Three cases:
	* Data is **missing at random (MAR)** if distribution of $\bm{R}$ depends on $\bm{Z}$ only via $\bm{Z}_{\mathrm{obs}}$:<br> $\mathbb{P}_{\theta}[\bm{R}|\bm{Z}] = \mathbb{P}_{\theta}[\bm{R}|\bm{Z}_{\mathrm{obs}}]$
		* Where $\theta$ are some parameters of the distribution
		* Missingness is not at random, but can be fully captured by features with full information

---

# Note on Missing Data

* Let $\bm{X}_{N \times p}$ be a data matrix with observed entries as $\bm{X}_{\mathrm{obs}}$ and response vector $\bm{y}_N$ 
* Let $\bm{Z} = (\bm{y}, \bm{X})$ and $\bm{Z}_{\mathrm{obs}} := (\bm{y}, \bm{X}_{\mathrm{obs}})$
* $\bm{R} = \{0, 1\}_{N \times p}$ is an indicator matrix with $1$ where $\bm{X}$ as missing value
* Three cases:
	* Data is **missing not at random (MNAR)** if it is neither MCAR or MAR. See also [change point detection](https://en.wikipedia.org/wiki/Change_detection).
		* Ex. Thermometer stops recording temperature when it’s overheated