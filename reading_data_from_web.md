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
