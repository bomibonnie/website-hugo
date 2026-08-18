---
title: Central Limit Theorem
linktitle: 
toc: true
type: docs
date: "2025-09-26T00:00:00+01:00"
draft: false
menu:
  example:
    parent: R
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

In this page, I'll share R commands for demonstrating the Central Limit Theorem using a simple in-class dice-rolling exercise. Each student recorded the average of 5 rolls and the average of 10 rolls; the dataset collects those averages across the whole class.

### Loading the data

```{r}
library(rio)
rolls <- import("dierolls.xlsx")

names(rolls)
```

### Comparing the two sampling distributions

The Central Limit Theorem tells us that as the sample size increases (10 rolls vs. 5 rolls), the sampling distribution of the mean becomes more tightly clustered around the true average. We can see this by overlaying the two histograms.

```{r}
# Get histogram data first to define ylim safely
h5 <- hist(rolls$avg_five, breaks = 10, plot = FALSE)
h10 <- hist(rolls$avg_ten, breaks = 10, plot = FALSE)

# Plot first histogram (orange)
plot(h5, col = "orange",
     xlim = range(c(1, 6)),
     ylim = c(0, max(c(h5$counts, h10$counts))),
     xlab = "Average Value",
     main = "Histograms of avg_five and avg_ten")

# Add second histogram (blue)
plot(h10, col = "blue", add = TRUE)

# Legend
legend("topright",
       legend = c("avg_five", "avg_ten"),
       fill = c("orange", "blue"))
```

Notice that the distribution of `avg_ten` is narrower than `avg_five` — averaging over more rolls reduces the variance of the sample mean, exactly as the Central Limit Theorem predicts.

### Saving the plot

```{r}
png("histogram_overlay.png", width = 600, height = 400)
# ... (plotting code above) ...
dev.off()
```
