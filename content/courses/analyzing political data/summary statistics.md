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

In this page, I'll share Stata commands for summary statistics. The dataset I use here is Militarized Interstate Disputes (v4.3) dataset from the Correlates of War Project. You can download the dataset [here](https://correlatesofwar.org/data-sets/MIDs).


### Opening data with `use` command

```{stata}
use "MIDA 4.3.dta", clear
```

[,clear] is telling Stata to clear any other data that you have opened in Stata. 

When you open an excel file (.csv or .xlsx), use 'import delimited' instead.

```{stata}
import delimited "MIDA 4.3.csv", clear 
```

### Looking at variables

What variables do we have in the example dataset?

```{stata}
describe
```
When you type 'describe', You can see the names and labels of all variables in the dataset. 


### Measures of central tendancy: mean, median, mode

Let's look at 'outcome' variable in the MIDA dataset. 'outcome' shows the outcome of dispute like below. 

|Value| Description |
| --- | --- |
|1 | Victory for side A |
|2 | Victory for side B |
|3 | Yield by side A  |
|4 | Yield by side B  |
|5 | Stalemate  |
|6 | Compromise  |
|7 | Released  |
|8 | Unclear  |
|9 | Joins ongoing war  |
|-9| Missing  |

Since the value doesn't have specific meaning (nominal), mean and median are not meaningful. Let's find the mode of this variable. 'tabulate' or simply 'tab' is useful to figure out the mode.

```{stata}
tabulate outcome

tab outcome, missing
```
We can see that 5 (Stalemate) is the mode of this variable. 

Let's fidn the mean and median of 'MaxDur' variable. 'MaxDur' is the maximum duration of dispute. First, you can use 'codebook' command. It will show you the range, number of unique values, number of missing values, mean, standard deviation, and percentiles. You can see that there is no missing values in this variable! 

```{stata}
codebook maxdur

summarize maxdur
sum maxdur, detail
```
Second, you can use summarize or simply sum to look at the number of observations, mean, standard deviation, minimum and maximum values. If you add ', detail' or ', d', then you can see more detailed information such as percentiles, variance, skewness, and kurtosis. 

The mean of the maximum duration of disputes is 161.72 and the median is 41. 

If you have missing values such as -9 in the previous example, you might want to exclude those values. Then, use 'if' option.


### Visualization

Let's make histograms and box plots! Histograms are helpful to look at the distribution of variables. The command is 'histogram' or 'hist'. You can add more options to make a prettier histogram.

```{stata}
histogram maxdur

hist maxdur, percent bin(15) xtitle("Maximum Duration of Dispute")
hist maxdur, percent width(10) xtitle("Maximum Duration of Dispute")
hist maxdur, freq xtitle("Maximum Duration of Dispute")

graph box maxdur
```
You can change the number and width of bins (bin and width respectively). The default setting will show you the density. You can change that by adding 'frequency' or 'percent' as an option to the command. 

You can draw a box plot using the command, 'graph box'. Since this variable is right-skewed, we can see many outside values above the box (dots above the upper adjacent line).
