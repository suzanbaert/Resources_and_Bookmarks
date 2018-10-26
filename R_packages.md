# Creating packages


## Creating a package
+ `devtools::create("mypackage")`: creates the backbone of the package including an R folder, description and namespace file


## R code

+ All functions go inside the R folder
+ Save them manually inside the R folder or use `dump("functionname", file = "mypackage/R/functionname.R)`
+ Ensure proper errors and warnings in your functions.

```
if(!is.numeric(x)) {
  stop("enter your error message")
}

if(!is.numeric(y)) {
  warning("enter your warning message")
}

```

## Description/Namespace file

+ Description: Should contain information on version, license and authors, and also contains imports and dependencies
+ No manual edits in Namespace file
+ In depends: just the R version. In Imports: all imported
+ Adding suggested packages (eg for building vignette): `devtools::use_package("package", "suggests")`


## Adding vignettes and data files

+ use `devtools::use_data(df, pkg = "mypackage")` (if inside mypackage project, second argument is unneeded)
+ use `devtools::use_vignette("vignettename", pkg = "mypackage") to add basic skeleton



<br>

## Documentation via Roxygen2

+ Add skeleton via Rstudio
+ All start with `#'`
+ Afterwards build via `devtools::document("mypackage")`
+ Defaults: First line is title, second description and Third Details

```
#' Title goes here
#'
#' Description goes here
#'
#' Details goes here
```


+ All others have to be added via tags: `#'@tag`
+ For examples there are `\dontrun{}`, `\dontshow{}` and `\donttest{}` exceptions

```
#' @param x add information for every parameter used in your function
#' @return add information about the output of your function
#' @import packagename
#' @importFrom package function
#' @export ##if this function is exported
#' @author
#' @notes
#' @examples add code for examples here.
```


For data sets:
```
#' @format description of the structure of the data
#' @source where you got the data from. can be `\url{}`
```

For packages:

Add a `mypackage.R` file:
```
#' Title
#'
#' Description
#'
#' @author Yourname \email{email.mail.com}
#' @docType package
#' @name packagename
"_PACKAGE"
```



Cross references:
```
#' @seealso point to other documentation [functionname()]
#' @family group related functions
```

Formatting:
+ `\link[packagename]{functionname}` : links to function from other package
+ `\link{functionname}` : links to function from same package
+ `\url{www.url.com}` : url
+ `\email{email@mail.com}` : add emailaddress
+ `\code{text}` : console formatting
+ `\itemize{ \item1 \item2 }` : list formatting


<br>

## Testing

+ Testing your package: `devtools::check("mypackage")`: by default will also build your documentation, and run any tests defined in `\tests`
+ Will give errors, warnings and notes
+ Typical errors might include undocumented functions, package dependencies etc

+ To enable continuous integration with Travis: `devtools::use_travis("mypackage")`
+ To add unit tests: `devtools::use_testthat("mypackage")`

### Individual tests
+ `expect_identifical()`: same values, attributes and types
+ `expect_identifical()`: same values and attributes, with optional tolerance argument
+ `expect_equivalent()`: same values


+ `expect_warning()` with optional text argument
+ `expect_error()` with optional text argument

+ `expect_output` for side effects (such as printing)
+ `expect_output_file` for longer side effects. First time run will create a file, next time will compare outputs


### Grouping tests

+ Makes it easier to see where it goes wrong
+ Add context on top of script
+ Output shows a dot for every tests that passes and a number of failures

```
context("text")
test_that("description of the group of tests"){

add multipe lines
of testing
}
```
