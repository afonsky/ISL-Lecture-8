---
background: /mountain.jpg
layout: cover
hideInToc: true
---

# Ensemble Models


---

# Ensemble Models

* Heterogeneous estimators:
	* **Stacking** ([Ch.8.8](https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=307)): models are weighted according to their predictive power
* Homogeneous estimators:
	* Committee methods (equi-weighted averaging):
		* **Bagging**: 	strong models trained on bootstrap observations
		* **Random Forest**: strong trees trained on bootstrap observations and sampled features
	* **Boosting**: a sequence of weak models trained on weighted samples
		* Weak tree classifier is a shallow tree or even a **stump** (single split tree)

---

# Boosting with AdaBoost

<div class="grid grid-cols-[7fr,3fr]">
<div>
<br>

* Let $Y = \pm 1$, $G(X)$ is a classifier on $X_{N \times p}$
* Model yields a sequence of weak classifiers, $G_m (X)$
* Prediction:
	* $\hat{Y} | X := \mathrm{sign}(\sum\limits_{m=1}^M \alpha_m G_m(X))$
		* This is an additive expansion of simple basis functions
* Model weights, $\alpha_m$:
	* $\alpha_m$ are learnt and give higher weight to better models
* Observation weights, $w_i$:
	* Each $G_m$ increases weights for misclassifications in $G_{m-1}$
</div>
<div>
    <figure>
    <img src="/ESL_figure_10.1.png" style="width: 400px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=357">ESL Fig. 10.1</a>
    </figcaption>
  </figure>
</div>
</div>

---

# AdaBoost.M1 Algorithm (Freund and Schapire, 1997)
<div class="bg-orange-100">
<v-clicks depth="3">

1. Let weights $w_i := \frac{1}{N}$, $i = 1:N$
2. For $m$ in $1$ to $M$:
	1. Fit classifier $G_m$ using previous weights $w_i$
	2. Compute model error, weighted misclassification, $\mathrm{err}_m := \frac{\sum_i w_i \cdot I (y_i \neq G_m(x_i))}{\sum_i w_i}$
	3. Compute model weight, $\alpha_m := \log \frac{1 - \mathrm{err}_m}{\mathrm{err}_m}$
		* This $\log$ value minimizes the expected exponential loss
	4. Update observation weights, $w_i \leftarrow w_i \cdot e^{\alpha I (y_i \neq G_m(x_i))}$, $\forall i$
		* no update for correct predictions or $\alpha_m = 0 \Leftrightarrow \mathrm{err}_m = 0.5$
		* increase by $e^{\alpha_m} > 1$ for misclassifications with $\alpha_m > 0 ~\Leftrightarrow~ \mathrm{err}_m > 0.5$
		* increase by $e^{\alpha_m} \in [0,1]$ for misclassifications with $\alpha_m < 0 ~\Leftrightarrow~ \mathrm{err}_m < 0.5$
3. Return $G(x) := \mathrm{sign}[\sum_m \alpha_m G_m(x)]$
</v-clicks>
</div>

##### Note: $w_i$ weights (for training observations) are discarded and not used in model inferencing
---

# AdaBoost Ex. on Simulated Data

<div class="grid grid-cols-[2fr,2fr]">
<div>
<br>

* A single stump performs just a bit better than random classifier
* AdaBoost:<br> “best off-the-shelf classifier in the world”
</div>
<div>
    <figure>
    <img src="/ESL_figure_10.2.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=359">ESL Fig. 10.2</a>
    </figcaption>
  </figure>
</div>
</div>