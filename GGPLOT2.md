# ggplot2

A list of things I always forget how to do:


## Fixing the order of bars

### Normal Plots
For normal plots or facetted plots with no words reappearing:  
Col_x is the column on the x-axis that needs re-ordering. Col_y is the column based on which I'm re-ordering.

```
df %>%
  mutate(col_x = reorder(col_x, col_y)) %>%
  ggplot(__________)
```


### Facet wrap plots
Discovered two nifty functions from [David Robinson's personal package](https://github.com/dgrtwo/drlib) to make ordered faceted plots with words that appear multiple times.  

The two functions:
```
library(tidytext)
library(ggplot2)

#functions from David Robinson
reorder_within <- function(x, by, within, fun = mean, sep = "___", ...) {
  new_x <- paste(x, within, sep = sep)
  stats::reorder(new_x, by, FUN = fun)
}

scale_x_reordered <- function(..., sep = "___") {
  reg <- paste0(sep, ".+$")
  ggplot2::scale_x_discrete(labels = function(x) gsub(reg, "", x), ...)
}
```

Example of code where I've used it:
```
df %>%
  group_by(episode) %>%
  top_n(10) %>%
  ungroup() %>%
  mutate(word = reorder_within(word, tf_idf, episode)) %>%
  ggplot(aes(word, tf_idf, fill = episode)) +
  geom_col(alpha = 0.8, show.legend = FALSE) +
  facet_wrap(~ episode, scales = "free_y", ncol = 3) +
  scale_x_reordered() +
  coord_flip()
```
