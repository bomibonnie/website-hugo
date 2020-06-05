---
title: Summary Statistics
linktitle: 
toc: true
type: docs
date: "2020-06-02T00:00:00+01:00"
draft: false
menu:
  example:
    parent: Stata
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

In this page, I'll share Stata commands for summary statistics.

### Opening data with `use` command

```{stata}
use "your file name.dta", clear
```

[,clear] is telling Stata to clear any other data that you have opened in Stata. 



### Looking at variables



* Histogram is helpful

```{stata}
histogram varname

hist varname, percent bin(15) xtitle("Your Variable")
hist varname, percent width(10) xtitle("Your Variable")
hist varname, freq xtitle("Your Variable")
```

### Measures of central tendancy: mean, median, mode

```{stata}
summarize varname
sum varname, detail

graph box varname

tabulate varname
tab varname, m
```
