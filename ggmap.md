# ggmap

ggmap::get_map() function accesses Google Maps. 

```
london_map_13 <- get_map(location = "London, England", zoom = 13)
london_ton_13 <- get_map(location = "London, England", zoom = 13, source = "stamen", maptype = "toner"
```

to make the map: ggmap(london_map_13)
