Reading data from web
================

Scrape data from web
--------------------

### First table in [National Survey on Drug Use and Health](http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm) :

Read the html:

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

Extract the table(s):

``` r
tabl_mar =   
  drug_use_html %>% 
  html_nodes(css = "table") %>% 
  first() %>% 
  html_table() %>%  
  slice(-1) %>% #get rid of the first row
  as_tibble()
```

Star War Movie info
-------------------

Data form [IMDB Star War Page](https://www.imdb.com/list/ls070150896/):

``` r
url = "https://www.imdb.com/list/ls070150896/"
swm_html = read_html(url)
```

[SelectorGadget](https://selectorgadget.com/) works here!!!

``` r
title_vec = 
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>%  # SelectorGadget 
  html_text %>% view

gross_rev_vec =  
  swm_html %>% 
  html_nodes(css = ".text-muted .ghost~ .text-muted+ span") %>%  # SelectorGadget 
  html_text %>% view

runtime_vec = 
  swm_html %>%
  html_nodes(".runtime") %>%
  html_text() %>% view

swm_df = 
  tibble(
    title = title_vec,
    gross_rev = gross_rev_vec,
    runtime = runtime_vec) %>% view
```

NYC Water consumption data, using API
-------------------------------------

Get data from [Water Consumption In The New York City](https://data.cityofnewyork.us/Environment/Water-Consumption-In-The-New-York-City/ia2d-e54m)

**Why API instaed of exporting:** Sometimes data may get updating, and to make our work more reproducible.

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
#if JSON
nyc_water = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>% 
  content("text") %>%
  jsonlite::fromJSON() %>%
  as_tibble()
```

[BRFSS](https://chronicdata.cdc.gov/Behavioral-Risk-Factors/Behavioral-Risk-Factors-Selected-Metropolitan-Area/acme-vg9e)
-------------------------------------------------------------------------------------------------------------------------

Same procedure for a different dataset

``` r
brfss_2010 = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv") %>% 
  content("parsed") # only 1000 rows here, we can request more
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   year = col_double(),
    ##   sample_size = col_double(),
    ##   data_value = col_double(),
    ##   confidence_limit_low = col_double(),
    ##   confidence_limit_high = col_double(),
    ##   display_order = col_double(),
    ##   locationid = col_logical()
    ## )

    ## See spec(...) for full column specifications.

``` r
brfss_smart2010 = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv",
      query = list("$limit" = 5000)) %>% 
  content("parsed")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   year = col_double(),
    ##   sample_size = col_double(),
    ##   data_value = col_double(),
    ##   confidence_limit_low = col_double(),
    ##   confidence_limit_high = col_double(),
    ##   display_order = col_double(),
    ##   locationid = col_logical()
    ## )
    ## See spec(...) for full column specifications.
