---
title: Describing Variables
linktitle: 
toc: true
type: docs
date: "2025-09-12T00:00:00+01:00"
draft: false
menu:
  example:
    parent: R
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

In this page, I'll share R commands for describing a variable: measures of central tendency, dispersion, and boxplots. I use the same CCES subset as in the [Visualization](../visualization/) tutorial.

### Loading the data

```{r}
library(rio)
cces <- import("cces18_subset.dta")

cces$age <- 2018 - cces$birthyr
summary(cces$age)
```

### Descriptive statistics

R doesn't bundle all descriptive statistics into a single command the way Stata's `summarize, detail` does, so we calculate each one individually.

```{r}
mean_age <- mean(cces$age, na.rm = TRUE)
median_age <- median(cces$age, na.rm = TRUE)
sd_age <- sd(cces$age, na.rm = TRUE)
min_age <- min(cces$age, na.rm = TRUE)
max_age <- max(cces$age, na.rm = TRUE)
range_age <- max_age - min_age
```

### Quartiles and the interquartile range

```{r}
q1 <- quantile(cces$age, 0.25, na.rm = TRUE)
q3 <- quantile(cces$age, 0.75, na.rm = TRUE)
iqr_age <- IQR(cces$age, na.rm = TRUE)
```

### Mode

R has no built-in function for the mode, so we find the most frequent value from a frequency table.

```{r}
mode_age <- as.numeric(names(sort(table(cces$age), decreasing = TRUE))[1])
```

### Boxplots

A boxplot is a quick way to see the median, quartiles, and any outliers at once.

```{r}
boxplot(cces$age,
        main = "Age Distribution - Boxplot",
        ylab = "Age",
        col = "lightgreen")
```

We can annotate the boxplot with the quartile values we calculated above.

```{r}
text(x = 1.3, y = q1, labels = paste("Q1 =", q1), pos = 4)
text(x = 1.3, y = median_age, labels = paste("Median =", median_age), pos = 4)
text(x = 1.3, y = q3, labels = paste("Q3 =", q3), pos = 4)
```
