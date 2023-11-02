---
background: /mountain.jpg
layout: cover
hideInToc: true
---

# Gradient Descent


---

# Gradient Descent

* In 1D with learning rate $\rho = 1$, we make a step in the opposite direction of a slope
	* So, if the slope is 0.7, we make a step - 0.7, which (hopefully) takes us to the minimum
	* Once the slope approaches zero, the step sizes approach zero as well
* Higher $\rho$ speeds up convergence, but we risk skipping the minimum
* Works well for convex differentiable loss functions
* In higher dimensions the gradient vector determines the direction and step length

<div class="grid grid-cols-[3fr,2fr]">
<div>
<br>
    <figure>
    <img src="/gradient.png" style="width: 270px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://imaddabbura.github.io/posts/optimization-algorithms/gradient-descent.html">https://imaddabbura.github.io/posts/optimization-algorithms/gradient-descent.html</a>
    </figcaption>
  </figure>
</div>
<div>
    <figure>
    <img src="/fastlr.png" style="width: 200px !important">
    <figcaption style="color:#b3b3b3ff; font-size: 11px;"><br>Image source:
      <a href="https://blog.paperspace.com/intro-to-optimization-in-deep-learning-gradient-descent/">https://blog.paperspace.com/intro-to-optimization-in-deep-learning-gradient-descent</a>
    </figcaption>
  </figure>
</div>
</div>