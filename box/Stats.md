# Stats



## ANOVA and post-hoc testing

Base R's ANOVA test.
Alternative to get a more SAS/SPSS looking readout is using `agricolae::HSD.test()`.


### one way

```
anova <- aov(y ~ x, data = df)
summary(anova)
pairwise.t.test(data$y, data$x, p.adj = "bonf")
```

### two or more way anova

```
anova <- aov(y ~ x1 + x2, data = df)
summary(anova)
TukeyHSD(anova, "df$x1", conf.level = 0.95)
```

<hr>
