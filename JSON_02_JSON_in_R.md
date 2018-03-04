# JSON writing and API struggles

Notes from my JSON woes...

## Parsing to JSON arrays within R (jsonlite):

JSON arrays: parse from any dataframe.  
Using `pretty = TRUE` gives the indented form.

```
one_car_df <- mtcars[1, 1:3]
toJSON(one_car_df, pretty = TRUE)
```

```
## [
##   {
##     "mpg": 21,
##     "cyl": 6,
##     "disp": 160
##   }
## ]
```

<br>

If the dataframe has more than one row, it will result in a JSON array of objects:

```
two_cars_df <- mtcars[1:2, 1:3]
toJSON(two_cars_df, pretty = TRUE)
```
```
## [
##   {
##     "mpg": 21,
##     "cyl": 6,
##     "disp": 160
##   },
##   {
##     "mpg": 21,
##     "cyl": 6,
##     "disp": 160
##   }
## ]
```

<br><br>

## Parsing to JSON objects within R (jsonlite):

To make JSON objects, you need to start from a list in R.  

```
car_list <- list(name = "Mazda RX4",
                 mpg = 21,
                 cyl = 6)
toJSON(car_list, pretty = TRUE)
```
```
## {
##   "name": ["Mazda RX4"],
##   "mpg": [21],
##   "cyl": [6]
## }
```

If your list elements have length 1, the default option in jsonlite::toJSON is still to turn the value into an array, which is very often not what you might want to do.  
In this case the solution is to add the `auto_unbox = TRUE` argument:

```
toJSON(car_list, auto_unbox = TRUE, pretty = TRUE)
```
```
## {
##   "name": "Mazda RX4",
##   "mpg": 21,
##   "cyl": 6
## }
```

<br><br><hr><br>

## Sending JSON data to API's

Two general options:  
Using the `httr` package, the `POST()` function will by default do the parsing to JSON

```
output <- POST(api_url, body = car_list, encode = "json")
```


Second option:
In more complicated cases, I found it easier to parse myself to JSON, and then post a JSON object/array to the API. In this case, you have to specify that you already have a JSON file by adding `content_type_json()`.

```
output <- POST("api_url, body = car_json, content_type_json())
```
