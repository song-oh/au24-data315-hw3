# Interactive Article: What Do "Prior" and "Posterior" Mean?

This project provides an article where we explore the concepts of **prior** and **posterior** beliefs in Bayesian statistics. The article is created using Svelte and designed for a non-technical audience, with an interactive slideshow structure to explain these ideas step by step.

You can access the article [here](https://6736d946990f560008dc3ee0--bespoke-chebakia-8b4932.netlify.app/).

## Article Structure

The article is divided into three sections:

### Part I: Setting the Prior

In this part, you'll set your initial belief (the **prior**) about the weight of a mystery box using a slider.

**Visualization:**  
- A slider with two handles allows you to adjust the range for your prior belief, which represents different levels of uncertainty in your initial guess.

### Part II: Updating Beliefs

You can click the 'See Data' button to reveal new weight measurements. As you observe these data points, you'll notice that the range of possible values becomes more focused. This is the process of **updating** — where new evidence helps refine our belief.

**Visualization:**  
- A set of individual data points is displayed to show the new information, and they turn into a histogram (using animated transitions implemented in D3) to show you how the weights are distributed around a more specific value.

### Part III: Building the Posterior Belief

Finally, we combine the prior belief and the new data to form the **posterior**. This section visually shows how the prior and the data distribution merge to form the posterior.

**Visualization:**  
- A graph shows the prior distribution, the observed data distribution, and the posterior distribution. The data and posterior curves adjust dynamically as you modify the value of the data’s mean and standard deviation.

