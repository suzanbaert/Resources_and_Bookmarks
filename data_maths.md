### Adding a column which specifies the quantile data is in:

Using the Empirical Cumulative Distribution Function:
var_p will return the

```
distr_var <- ecdf(df$var)
df$var_p <- distr_var(df$var)
```

[Source](https://stats.stackexchange.com/questions/50080/estimate-quantile-of-value-in-a-vector)
