# NA


### Are there any NA?

+ `anyNA()`: are there any NA in the dataframe
+ `sapply(df, anyNA)`: checks the whole df

+ `colSums(!is.na(df))`: lists number of not NA per variable in df
+ `purrr::map_df(~is:na(.))`: similar via purrr


### Kick them out

+ `filter(df, !is.na(var))
