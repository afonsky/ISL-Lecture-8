---
background: /mountain.jpg
layout: cover
hideInToc: true
---

# Trees

---

# Trees

<div class="grid grid-cols-[3fr,2fr]">
<div>

* We split a feature space into **rectangles** $\{R_m\}_{1:M}$ and fit a **constant** $c_m$ on each
* Constructed with **binary recursive splitting**
* A **split** is a feature and its value <br> provides “best” separation of the output
	* “best” means:
		* In regression: 
			* Smallest total error from each sub-branch
		* In classification: 
			* **Purest** classes in each branch
* The model: $\hat{f}(\bm{X}_{1 \times p}) := \sum_m c_m I (\bm{X} \in R_m)$
</div>
<div>
    <figure>
    <img src="/ESL_figure_9.2.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=325">ESL Fig. 9.2</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Growing Regression Trees

* We select $\{R_m\}$ minimizing RSS on each region $R_m$
* The minimizer is $\hat{c}_m := \bar{y} |_{R_m} = \frac{1}{N_m} \sum_{x_i \in R_m} y_i$
	* i.e. average response on $R_m$
	* $N_m := \# \{x_i \in R_m \}$
	* Search for all possible combinations of $\{R_m\}$ is computationally intractable
* Greedy approach:
	* At every tree node we seek a splitting variable $j$ and split point $s$ yielding lowest total RSS:
	$\min_{j, s} \big[\min_{c_1}~ \mathrm{RSS}_{\mathrm{Left}}(j, s)~~+~~ \min_{c_2}~ \mathrm{RSS}_{\mathrm{Right}}(j, s) \big] =$
	$~~~~~~~~\min_{j, s} \big[\min_{c_1}\sum_{x_i \in R_1(j, s)} (y_i - c_1)^2 ~~+~~ \min_{c_2} \sum_{x_i \in R_2(j, s)} (y_i - c_2)^2 \big]$
	* Then repeat the split on each resulting region 
* If unstrained this will overfit the observations

---

# Controlling Tree Complexity

* One approach:
	* Consider threshold $\tau$
	* If $\Delta \mathrm{RSS} > \tau$, continue splitting

<div class="grid grid-cols-[3fr,2fr]">
<div>

* However, $\Delta \mathrm{RSS}$ (or $\Delta \mathrm{MSE}$) can increase <br> and decrease as the tree grows
	* So, we might stop growing a tree prematurely
* Ex.
	* If $\tau_{\mathrm{MSE}} = 6$, then left branch would not be built
	* But, in level, $\mathrm{MSE}$ drops by $30.5$
</div>
<div>
    <figure>
    <img src="/Decision Trees Explained.png" style="width: 420px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://towardsdatascience.com/decision-trees-explained-3ec41632ceb6">https://towardsdatascience.com/decision-trees-explained-3ec41632ceb6</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Cost Complexity Pruning with Cross Validation

* Consider a **full tree** $T_0$ and some subtree $T$ 
* Let $|T| = \#\{\mathrm{terminal~nodes~of~} T\}$
* **Goal**: find minimizer $T_\alpha := \mathrm{argmin}_T \color{grey}\underbrace{\color{#006} \sum\limits_{m=1}^{|T|} \mathrm{RSS}_m + \alpha |T|}_{C_{\alpha}, \mathrm{~cost~complexity~criterion}}$
	* So, we are looking for a subtree with lowest CV-RSS for a fixed $\alpha$
	* Note: self-validation (i.e. in-sample) will overfit to training observations
* Such unique $T_\alpha$ is found via **weakest link pruning**:
	* Sequentially collapse internal nodes producing the smallest per-node increase in $\mathrm{CV-RSS}_m$
	* Learn more in [this source](http://mlwiki.org/index.php/Cost-Complexity_Pruning)

---

# Growing Classification Trees

* Let $\hat{p}_{mk} := \frac{1}{N_m} \sum_{x_i \in R_m} I (y_i = k)$ estimates $\mathbb{P} [Y = k | R_m]$ for $k = 1:K$
	* i.e. fraction of class $k$ observation in $R_m$
	* $R_m$ is classified to the **majority** class, i.e. $k_m := \mathrm{argmax}_k \hat{p}_{mk}$
<div class="grid grid-cols-[3fr,2fr]">
<div>

* **Impurity** measures:
	* Misclassification error<br> $\frac{1}{N_m} \sum\limits_{i \in R_m} I (y_i \neq k(m)) = 1 - \hat{p}_{mk(m)}$
	* Gini index<br> $\sum\limits_{k \neq k^{\prime}} \hat{p}_{mk} \hat{p}_{mk^{\prime}} = \sum\limits_{k=1}^K \hat{p}_{mk} (1 - \hat{p}_{mk})$
	* Cross-entropy or deviance<br> $-\sum\limits_{k=1}^K \hat{p}_{mk} \log \hat{p}_{mk}$
</div>
<div>
<br>
<br>
    <figure>
    <img src="/ESL_figure_9.3.png" style="width: 500px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=328">ESL Fig. 9.3</a>
    </figcaption>
  </figure>
</div>
</div>

---

# Recall: Type I and Type II Errors

* $\alpha$ (**type I error**), the probability of **falsely rejecting** the null hypothesis, $H_0$
	* a.k.a. **significance level**,  $1 - \mathrm{confidence~level}$, or **false positive (FP)**
* $\beta$ (**type II error**) is the probability of **falsely failing to reject** the $H_0$
	* $1 - \beta$ is the [power](https://en.wikipedia.org/wiki/Power_(statistics)) of the test, or **false negative (FN)**
<figure>
<br>
<img src="/Type_I_II_errors.png" style="width: 400px !important">
<figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
  <a href="https://en.wikipedia.org/wiki/Type_I_and_type_II_errors">https://en.wikipedia.org/wiki/Type_I_and_type_II_errors</a>
</figcaption>
</figure>

---

# Ex: COVID classifier

* $H_0$: Joe has no COVID vs. $H_1$ Joe has COVID
	* **True positive (TP)** and **True Negative (TN)**:
		* we correctly classified Joe with COVID or without it
	* **FP**: we falsely marked Joe as having COVID
		* generally, not a problem and we can do a second test
	* **FN**: we missed COVID in Joe and let him go
		* Big problem because Joe can die
* We should choose a **lower** $\beta$ to have a very low risk of **FN**
	* i.e. move vertical line to the left in the image
	* we control this by specifying larger $\alpha$
* This “importance” can be incorporated in **loss matrix**

---

# Loss Matrix with 2 Classes, $L_{2 \times 2}$

* Recall: confusion matrix counts TP, TN, FP, FN after predictions are made
* Loss matrix allows us to assign weights to FP and FN errors
	* We keep TP and TN weights at 0
* Ex. 
	* Say, we value the error of missing COVID in Joe 10x more important than falsely detecting COVID
	* Then corresponding **loss matrix** is $\begin{bmatrix}0 & L_{FP} = 1 \\ L_{FN} = 10 & 0\end{bmatrix}$
		* A loss matrix gives the cost that is incurred for each element of the
confusion matrix
		* Caution: this matrix can be transposed as well

---

# Loss Matrix with $K$ Classes, $L_{K \times K}$

* We specify loss weights in loss matrix as $\begin{bmatrix}0 & \ldots & L_{1K} \\ \vdots & \ddots & \vdots \\ L_{K1} & \ldots & 0\end{bmatrix}$
	* where $L_{k k^{\prime}}$ is the false classification of the $k$th class observation to class $k^{\prime}$
* Weighted Gini index: $\sum\limits_{k \neq k^{\prime}} L_{k k^{\prime}} \hat{p}_{mk} \hat{p}_{m k^{\prime}}$
	* This has no effect in a two-class case because the total weight is constant
		* $L_{01} \hat{p}_{m0} \hat{p}_{m1} + L_{10} \hat{p}_{m1} \hat{p}_{m0} = (L_{01} + L_{10}) \hat{p}_{m1} \hat{p}_{m0}$
* In 2 class case simply weight the observation as
	* from class $0$ with $L_{01}$ 
	* from class $1$ with $L_{10}$

---

# Missing Values with Trees

* In general, we find ways to **impute** NAs with numeric values <br> before applying a tree-based model:
	* Categorical feature:
		* Make a new category NA
			* You might discover association between NAs and $Y$
	* Numerical feature:
		1. Choose a (primary) split $X_j < c$ (feature and split value ) as usual, ignoring NAs
		2. Build a list of surrogate splits
			1. Surrogate split that best mimics $X_j < c$
			2. Surrogate split the is next best in mimicking $X_j < c$
			3. ...
		3. When using the tree, if encountering NA, use the next available surrogate

---

# Multiway Splits

* In a binary tree we build a split $X_j < c$

* In multiway split observations based on  intervals $[c_{i-1}, c_i)$

<div class="grid grid-cols-[3fr,2fr]">
<div>

* Disadvantage:
	* Split is too fast
	* Relatively shallow depth dangers overfitting (difficult to control)
</div>
<div>
<br>
	<figure>
    <img src="/How_is_Splitting_Decided_for_Decision_Trees.png" style="width: 400px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://www.displayr.com/how-is-splitting-decided-for-decision-trees">www.displayr.com/how-is-splitting-decided-for-decision-trees</a>
    </figcaption>
  	</figure>
</div>
</div>

---

# High Variance of (full grown) Trees

* A slightly different training set may result in a very different tree
	* This is due to hierarchical structure
		* A small change in any internal node propagates through its branch
* Ways to avoid:
	* Use more stable split criterion
	* **Averaging**:
		* **Bagging**: bootstrap observations. Then average predictions
		* **Random forest**: bootstrap observations and **sampling features**. Idem
			* In fact, here we want high variability in trees, which **decorrelates** them <br>
			<figure>
		    <img src="/Green_Blue_Tree.png" style="width: 300px !important">
		    <figcaption style="color:#b3b3b3ff; font-size: 11px;">Image source:
		      <a href="https://uhlibraries.pressbooks.pub/buildingskillsfordatascience/chapter/random-forest">https://uhlibraries.pressbooks.pub/buildingskillsfordatascience/chapter/random-forest</a>
		    </figcaption>
		  	</figure>

	* Use shallow trees (say, in boosting)

---

# Trees Lack Smoothness of the Prediction Surface

<div class="grid grid-cols-[4fr,3fr,3fr]">
<div>

* In classification with 0-1 loss
	* This is not a problem

* In **regression setting**:

	* Can be problematic, where the underlying function is smooth
</div>
<div>
<br>
	<figure>
    <img src="/Decision_Trees_regression_1.png" style="width: 250px !important">
    <br>
    <img src="/Decision_Trees_regression_2.png" style="width: 250px !important">
  	</figure>
</div>
<div>
<br>
	<figure>
    <img src="/Decision_Trees_regression_3.png" style="width: 250px !important">
    <br>
    <img src="/Decision_Trees_regression_4.png" style="width: 250px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Images source:
      <a href="https://bradleyboehmke.github.io/HOML/DT.html">https://bradleyboehmke.github.io/HOML/DT.html</a>
    </figcaption>
  	</figure>
</div>
</div>

---

# Patient Rule Induction Method a.k.a Bump Hunting

<!-- * PRIM also looks for a rectangle with high average response -->
<div class="grid grid-cols-[4fr,3fr] gap-1">
<div>

* Top-down approach:
	1. Start with all observations 
	2. Greedy edge (**face**) **compression**:
		* Seek edge that maximizes box’s mean after side is trimmed (**peeled off**) by $\alpha$ fraction
		* Compress edges till box converges to a bump (area with high mean)
	3. Pasting (opposite to compression):
		* Expand any edge that increases the new box’s mean
</div>
<div>
<br>
	<figure>
    <br>
    <img src="/ESL_figure_9.7.png" style="width: 400px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://hastie.su.domains/ElemStatLearn/printings/ESLII_print12.pdf#page=338">ESL Fig. 9.7</a>
    </figcaption>
  	</figure>
</div>
</div>

* The final model has **interpretable** rules: $a_1 \leq X_1 < b_1, a_2 \leq X_2 < b_2, ...$
* PRIM also looks for a rectangle with high average response