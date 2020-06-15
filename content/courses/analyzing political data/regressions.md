---
title: Regressions
linktitle: 
toc: true
type: docs
date: "2020-06-02T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Stata
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

For this correlation and regression practice, I use a county-level voter turnout dataset. You can get this dataset by [request](mailto:bomi-lee-1@uiowa.edu).

Load the dataset first.

```{stata}
use "county_turnout.dta"
```
The research question for this tutorial is: Are counties with more millennials likely to have lower turnout?

Let's make a scatter plot.

```{stata}
scatter perc_turnout perc_mille
```
* Optional: 1) make it prettier using `grstyle`, 2) add the best fitted link to the scatter plot.


```{stata}
grstyle init
grstyle set plain

scatter perc_turnout perc_mille

twoway (scatter perc_turnout perc_mille)(lfit perc_turnout perc_mille)
```

Is there a linear relationship? 

### Correlation 

Let's look at the correlation between thses two variables.

```{stata}
pwcorr perc_turnout perc_mille

pwcorr perc_turnout perc_mille, sig
```
Interpret the result. The linear relationship between voter turnout and the percentage of millennials is statistically significant?


### Regressions

Let's start with a simple regression. Our DV is voter turnout and our IV is the percentage of millennials. 

```{stata}
reg perc_turnout perc_mille
```
What happens if we add a control variable, percent_college to the regression model? The effect of the percentage of millennials is still significant? 

```{stata}
reg perc_turnout perc_mille perc_college
```

### Visualization using `margins`

Let's visualize the predicted voter turnout. Look at our IV (perc_mille) and choose the range for the plot. I choose the range from 10 to 40 with 5 percent gap. 

```{stata}
sum perc_mille, d
margins, at(perc_mille=(10(5)40)) atmeans

marginsplot, xtitle("Percentage of Millennials") ytitle("Voter Turnout") ///
	ti(Effect of Millennials on Voter Turnout) 
```
