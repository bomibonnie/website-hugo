---
title: Bivariate Statistics
linktitle: 
toc: true
type: docs
date: "2020-06-02T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Stata
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

Here are Stata commands for bivariate statistics. I use the 2018 CCES Common Content Dataset for this tutorial. You can download the data from the CCES [Dataverse](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910/DVN/ZSBZ7K).

Let's load the data first. 

```{stata}
use "cces18_common_vv.dta"

```
We are going to use "CC18_426_3" as our DV. "CC18_426_3" is based on the question that "State legislatures must make choices when making spending decisions on important state programs. How would you like your legislature to spend money on education?" Of course, this variable is ordinal, but let's assume it is continuous. 

My research question for this practice is this: Is there any gender difference in support for education spending?

Look at our DV.

```{stata}
tab CC18_426_3, m
tab CC18_426_3, nolab
```
In the original variable, "Greatly increase" is labeled 1 and "Greatly decrease" is labeled 5. I'll change the value for "Greatly increase" to 5 and for "Greatly decrease" to 1. To keep the original variable, I make a new variable called 'educ_spending' and recode this new variable. 

```{stata}
gen educ_spending = CC18_426_3 
recode educ_spending(1=5)(2=4)(4=2)(5=1)
```

### Difference of means test

You can use `ttest` command to conduct the difference of means test. 

```{stata}
ttest educ_spending, by(gender)
```

Look at the p-value below the alternative hypothesis. Do we reject the null hypothesis? How can we know that?


### ANOVA

For the ANOVA test, we are going to focus on four race groups: White, Black, Hispanic, and Asian. Let's look at the variable, race. 

Since we don't include other categories (Native American, Mixed, Other, and Middle Eastern) in our analysis, I add 'if' options to the code below. 

```{stata}
tab race

anova educ_spending race if (educ_spending ~=.) & (race<5)
```
Do we reject the null that the means are the same across the four racial groups?


### $\chi^2$ test

Answer the questions below by using the $\chi^2$ test.

Question 1: Gender and Party ID are independent?
Question 2: Education and Family Income are independent?

Step 1. Find relevant variables in the dataset.


```{stata}
codebook gender
codebook pid3

codebook educ
codebook faminc_new
```

Step 2. Look at the variables and recode them if necessary

Check missing values, labels, etc.

```{stata}
tab faminc_new, m
tab faminc_new, nolab

gen pid3_new = pid3
recode pid3_new(4=.)(5=.)
recode pid3_new(4/5=.)
```

Step 3. Conduct the $\chi^2$ test

```{stata}
tab gender pid3_new, chi2

tab educ faminc_new if faminc_new<17, chi2
```

Are they independent? How do we know that?


### Strength of association

One step further. Let's look at the strength of association using Kendall's Tau-b and Cramer's V.

#### Kendall's Tau-b

If your variables of interest are oridnal, you can use Kendall's Tau-b for the strength of association, which bounded between -1 and 1. 

```{stata}
tab educ faminc_new if faminc_new<17, taub
ktau educ faminc_new if faminc_new<17, stats(taub)
```

#### Cramer's V

If your variables of interest are nominal, then you can use Cramer's V for the strength of association, which is calculated using the equation, $\Large v= \sqrt{\frac{\chi^2}{nm}}$. Cramer's V is bounded between 0 and 1.

```{stata}
tab gender pid3_new, V
```