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

Here are Stata commands for bivariate statistics.

Data from CCES [Dataverse](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910/DVN/ZSBZ7K)



```{stata}
use "cces18_subset.dta"

```

We are going to use "CC18_426_3" as our DV. "CC18_426_3" is from the question below.

State legislatures must make choices when making spending decisions on important state programs. How would you like your legislature to spend money on education?

Let's assume this variable is continuous. Is there any gender difference in support for education spending?

Look at the variable.

```{stata}
tab CC18_426_3, m
tab CC18_426_3, nolab
```

Rename and recode the variable

```{stata}
rename CC18_426_3 educ_spending
recode educ_spending(1=5)(2=4)(4=2)(5=1)
```

### Difference of means test

```{stata}
ttest educ_spending, by(gender)
```


How about races? 
We are going to focus on four race groups: White, Black, Hispanic, and Asian.

### ANOVA

```{stata}
tab race

anova educ_spending race if (educ_spending ~=.) & (race<5)
```

### $\chi^2$ test

Question 1: Gender and Party ID are independent?
Question 2: Education and Family Income are independent?

```{stata}
codebook gender
codebook pid3

codebook educ
codebook faminc_new

tab faminc_new, missing
tab faminc_new, nolab


tab gender pid3 if pid3<4, chi2
tab educ faminc_new if faminc_new<17, chi2

gen pid3_new = pid3
recode pid3_new(4=.)(5=.)
recode pid3_new(4/5=.)

tab gender pid3_new, chi2
```

### Strength of association

#### Kendall's Tau-b

```{stata}
tab educ faminc_new if faminc_new<17, taub
ktau educ faminc_new if faminc_new<17, stats(taub)
```

#### Cramer's V

```{stata}
tab gender pid3_new, V
```