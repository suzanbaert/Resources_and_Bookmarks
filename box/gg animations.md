# animations


### gif from japan data (Datacamp course)
See also [here](http://blog.revolutionanalytics.com/2016/02/japans-ageing-population-animated-with-r.html)

```# Inspect structure of japan
str(japan)

# Finish the code inside saveGIF
saveGIF({

  # Loop through all time points
  for (i in unique(japan$time)) {

    # Subset japan: data
    data <- subset(japan, time == i)

    # Finish the ggplot command
    p <- ggplot(data, aes(x = AGE, y = POP, fill = SEX, width = 1)) +
      coord_flip() +
      geom_bar(data = data[data$SEX == "Female",], stat = "identity") +
      geom_bar(data = data[data$SEX == "Male",], stat = "identity") +
      ggtitle(i)

    print(p)

  }

}, movie.name = "pyramid.gif", interval = 0.1)
```


Via gganimate:

```
# Update the static plot
p <- ggplot(Vocab, aes(x = education, y = vocabulary,
                       color = year, group = year,
                       frame = year, cumulative = TRUE)) +
  stat_smooth(method = "lm", se = FALSE, size = 3)

# Call gg_animate on p
gg_animate(p, interval = 1.0, filename="vocab.gif")
```
