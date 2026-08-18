---
title: Visualization
linktitle: 
toc: true
type: docs
date: "2025-09-05T00:00:00+01:00"
draft: false
menu:
  example:
    parent: R
    weight: 1

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

In this page, I'll share R commands for visualizing data. The dataset I use here is a subset of the 2018 CCES Common Content Dataset. You can download the full dataset from the CCES [Dataverse](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi%3A10.7910/DVN/ZSBZ7K).

### Opening data with `import()`

I use the `rio` package, which can read many file formats (`.dta`, `.csv`, `.xlsx`) with a single function.

```{r}
library(rio)
cces <- import("cces18_subset.dta")
```

### Looking at variables

```{r}
head(cces)     # Shows first 6 rows
View(cces)     # Opens data in a spreadsheet view
names(cces)    # Lists all variable names
```

You can rename a variable like this:

```{r}
names(cces)[names(cces) == "faminc_new"] <- "family_income"
```

### Making a new variable

Let's create an `age` variable from `birthyr`.

```{r}
class(cces$birthyr)          # Shows if it's numeric, character, etc.
summary(cces$birthyr)        # Shows min, max, mean, etc.
sum(is.na(cces$birthyr))     # Counts missing values (NA in R)

cces$age <- 2018 - cces$birthyr
summary(cces$age)
```

### Histograms

Histograms are helpful to look at the distribution of a continuous variable. The base R function is `hist()`.

```{r}
hist(cces$age)                                     # Basic histogram
hist(cces$age, main = "Age", xlab = "Age")          # With better labels

# Percentages instead of counts
hist(cces$age, freq = FALSE, main = "Age", xlab = "Age", ylab = "Percent")

# Change the number of bars
hist(cces$age, breaks = 10, main = "Age", xlab = "Age")
hist(cces$age, breaks = 15, main = "Age", xlab = "Age")
```

You can also split the data into groups and compare histograms side by side.

```{r}
males <- cces[cces$gender == 1, ]
females <- cces[cces$gender == 2, ]

hist(males$age, main = "Age - Males", xlab = "Age")
hist(females$age, main = "Age - Females", xlab = "Age")
```

### Working with a coded variable

The `faminc_new` variable is coded, so let's check what values it takes before summarizing it.

```{r}
summary(cces$faminc_new)
table(cces$faminc_new)        # Count how many of each value
sum(is.na(cces$faminc_new))
unique(cces$faminc_new)       # Shows all unique values
```

Note that 97 means "Prefer not to say," so we exclude it before summarizing.

```{r}
valid_income <- cces$faminc_new[cces$faminc_new < 20]
summary(valid_income)
mean(valid_income, na.rm = TRUE)   # na.rm = TRUE ignores missing values

hist(valid_income, main = "Family Income", xlab = "Income Level")
table(valid_income)
```

### Bar plots for categorical variables

For categorical variables like marital status, use a bar plot instead of a histogram.

```{r}
marstat_counts <- table(cces$marstat)
marstat_counts

barplot(marstat_counts, main = "Marital Status", ylab = "Number of People")

# As percentages
marstat_percent <- prop.table(marstat_counts) * 100
barplot(marstat_percent, main = "Marital Status", ylab = "Percent")
```

### Saving a plot

```{r}
png("hist_age.png")   # Start saving
hist(cces$age)          # Make your plot
dev.off()                 # Finish saving
```
