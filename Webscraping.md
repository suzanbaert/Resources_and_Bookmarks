# Webscraping

## Storing IDs and or passwords outside your scripts/repos

The idea from *Automated data collection in R* (see references) is to store them in your .Rprofile, which gets loaded automatically when you start R.

For ID/password combinations
+ One time: store them in R.profile: `options(WebsiteName = "userid:password")`
+ Afterwards within a URL refer to it as: `getOption("WebsiteName")`

For API's similar
+ One time: store your token in R.profile: `options(APIname = "token")`
+ Afterwards in GET requests: `getOption("APIname")`

<br>

## Parsing your url



When getting: `Warning message:XML content does not seem to be XML`, try reading the source code first, and then parsing:
```
url_source <- readLines(url, encoding = "UTF-8")
url_parsed <- RCurl::htmlParse(url_source, encoding = "UTF-8")
```
Alternative:
```
urlget <- RCurl::getURL(url)
url_parse <- RCurl::htmlParse(urlget, encoding="UTF-8")
```




## Extracting data

Useful functions:
+ `XML::readHTMLTable()` - locates tables and parses them into a list of dataframes
+ `XML::readHTMLList()` - locates lists
+ `XML::getHTMLLinks()` - returns all external links contained in a page. Can be combined with an xpQuery for a more precise XPath.
+ `XML::getHTMLExternalFiles` - returns all links to external files (images e.g). Can be combined with an xpQuery for a more precise XPath.
+ `XML::readHTMLTable(source, elFun =  getHTMLLinks)` - gets the links inside the table. Default elFun is xmlValue
