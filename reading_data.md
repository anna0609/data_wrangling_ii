Reading Data
================
Minjie Bao
2020-10-24

## Scrape a table

I want the first table from [this
page](http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm)

reading the html

``` r
url ="http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm" 

drug_use_html = read_html(url)
```

extract the table(s); focus on the first one

``` r
tabl_marj = 
drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>% 
  html_table() %>% 
  slice(-1) %>% 
  as_tibble()
```

## star wars movie info

I want the data from[here](https://www.imdb.com/list/ls070150896/).

``` r
url = "https://www.imdb.com/list/ls070150896/"

sw_html = read_html(url)
```

Grab elements that I want.

``` r
title_vec = 
  sw_html %>% 
  html_nodes(css = ".lister-item-header a") %>% 
html_text()

gross_rev_vec = 
  sw_html %>% 
  html_nodes(css = ".text-muted .ghost~ .text-muted+ span") %>% 
html_text()

runtime_vec = 
    sw_html %>% 
  html_nodes(css = ".runtime") %>% 
html_text()

swm_df = tibble(
  title = title_vec,
  gross_rev = gross_rev_vec,
  runtime = runtime_vec
)
```

## Get some water data

This is coming from API

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   year = col_double(),
    ##   new_york_city_population = col_double(),
    ##   nyc_consumption_million_gallons_per_day = col_double(),
    ##   per_capita_gallons_per_person_per_day = col_double()
    ## )

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>% 
  content("text") %>% 
  jsonlite::fromJSON() %>% 
  as_tibble()
```
