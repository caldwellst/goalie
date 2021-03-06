
<!-- README.md is generated from README.Rmd. Please edit that file -->

# goalie <a href='https://github.com/gpw13/goalie'><img src='man/figures/logo.png' align="right" height="139" /></a>

<!-- badges: start -->

[![R-CMD-check](https://github.com/gpw13/goalie/workflows/R-CMD-check/badge.svg)](https://github.com/gpw13/goalie/actions)
<!-- badges: end -->

## Overview

goalie is an R package designed to provide a simple interface for
extracting data from the United Nations Statistics Divisions (UNSD)
[Sustainable Development Goals (SDG)
API](https://unstats.un.org/SDGAPI/swagger/). The package allows for
exploration of SDG data contained within, including available
dimensions/attributes and geographic coverage, while providing a simple
tool to extract all data quickly into R.

-   `sdg_overview()`, `sdg_targets()`, and `sdg_indicators()` provide
    data frames covering the [goals, targets, and indicators of the
    SDGs](https://unstats.un.org/sdgs/indicators/Global%20Indicator%20Framework%20after%202020%20review_Eng.pdf).
-   `sdg_dimensions()` and `sdg_attributes()` provides a data frame of
    dimensions and attributes available for a specific SDG or series of
    data.
-   `sdg_series()` provides a list of all data series available in the
    SDG database.
-   `sdg_geoareas()` provides an overview of geographic areas covered by
    data in the SDG database, or that have data for a specific SDG goal,
    target, indicator, or series.
-   `sdg_geoarea_data()` lists the SDG data available for a specific
    geographic area.
-   `sdg_data()` extracts data for a selection of series in the GHO and
    outputs all results in a single data frame.

The interface is designed to be as simple as possible, only requiring
input of the code of an indicator to extract it. However, at request,
more detailed implementation of the SDG API’s systems can be
implemented. Please provide any requests through the [Github issues
page](https://github.com/gpw13/goalie/issues).

goalie can be installed using `remotes::install_github("gpw13/goalie")`

## Usage

To begin, we can use `gho_overview()` to begin to explore all data
available in the GHO.

``` r
library(goalie)

sdg_overview()
#> # A tibble: 14,199 x 12
#>   goal  goal_title   goal_description   target_description target_title   target
#>   <chr> <chr>        <chr>              <chr>              <chr>          <chr> 
#> 1 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> 2 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> 3 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> 4 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> 5 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> # … with 14,194 more rows, and 6 more variables: indicator_description <chr>,
#> #   indicator_tier <chr>, indicator <chr>, series_description <chr>,
#> #   series_release <chr>, series <chr>
```

If we want the data for `SI_POV_DAY1`, we could now just quickly access
the data frame using `sdg_data()`.

``` r
sdg_data("SI_POV_DAY1")
#> # A tibble: 3,033 x 23
#>    Goal Target Indicator SeriesCode  SeriesDescription   GeoAreaCode GeoAreaName
#>   <dbl>  <dbl> <chr>     <chr>       <chr>                     <dbl> <chr>      
#> 1     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 2     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 3     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 4     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 5     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> # … with 3,028 more rows, and 16 more variables: TimePeriod <dbl>, Value <dbl>,
#> #   Time_Detail <dbl>, TimeCoverage <lgl>, UpperBound <lgl>, LowerBound <lgl>,
#> #   BasePeriod <lgl>, Source <chr>, GeoInfoUrl <lgl>, FootNote <chr>,
#> #   Age <lgl>, Location <lgl>, Nature <chr>, Reporting_Type <chr>, Sex <lgl>,
#> #   Units <chr>
```

From here, standard methods of data manipulation (e.g. base R, the
tidyverse) could be used to select variables, filter rows, and explore
the data. However, we can also continue to explore other aspects of the
SDG database. For instance, if we wanted to see the dimensions and
attributes of `SI_POV_DAY1`, we can easily access that.

``` r
sdg_dimensions(series = "SI_POV_DAY1")
#> # A tibble: 142 x 4
#>   id    code  description        sdmx 
#>   <chr> <chr> <chr>              <chr>
#> 1 Age   <1M   under 1 month old  M0   
#> 2 Age   <1Y   under 1 year old   Y0   
#> 3 Age   <5Y   under 5 years old  Y0T4 
#> 4 Age   <15Y  under 15 years old Y0T14
#> 5 Age   <18Y  under 18 years old Y0T17
#> # … with 137 more rows
```

``` r
sdg_attributes(series = "SI_POV_DAY1")
#> # A tibble: 8 x 4
#>   id     code  description            sdmx 
#>   <chr>  <chr> <chr>                  <chr>
#> 1 Nature C     Country data           C    
#> 2 Nature CA    Country adjusted data  CA   
#> 3 Nature E     Estimated data         E    
#> 4 Nature G     Global monitoring data G    
#> 5 Nature M     Modeled data           M    
#> # … with 3 more rows
```

Let’s say we want to get data for a specific country, then we could look
up the M49 code using the table available through the API.

``` r
sdg_geoareas()
#> # A tibble: 390 x 2
#>   geoAreaCode geoAreaName   
#>   <chr>       <chr>         
#> 1 4           Afghanistan   
#> 2 248         Åland Islands 
#> 3 8           Albania       
#> 4 12          Algeria       
#> 5 16          American Samoa
#> # … with 385 more rows
```

We can then even check what data is available for a specific country,
say Angola.

``` r
sdg_geoarea_data(24)
#> # A tibble: 454 x 12
#>   goal  goal_title   goal_description   target_description target_title   target
#>   <chr> <chr>        <chr>              <chr>              <chr>          <chr> 
#> 1 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> 2 1     End poverty… Goal 1 calls for … By 2030, eradicat… By 2030, erad… 1.1   
#> 3 1     End poverty… Goal 1 calls for … By 2030, reduce a… By 2030, redu… 1.2   
#> 4 1     End poverty… Goal 1 calls for … By 2030, reduce a… By 2030, redu… 1.2   
#> 5 1     End poverty… Goal 1 calls for … By 2030, reduce a… By 2030, redu… 1.2   
#> # … with 449 more rows, and 6 more variables: indicator_description <chr>,
#> #   indicator_tier <chr>, indicator <chr>, series_description <chr>,
#> #   series_release <chr>, series <chr>
```

And we can get data from the SDG for multiple series in one call, with
the output data frames already merged together.

``` r
sdg_data(c("SI_POV_DAY1", "SI_POV_EMP1", "SI_POV_NAHC"))
#> # A tibble: 27,283 x 23
#>    Goal Target Indicator SeriesCode  SeriesDescription   GeoAreaCode GeoAreaName
#>   <dbl>  <dbl> <chr>     <chr>       <chr>                     <dbl> <chr>      
#> 1     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 2     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 3     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 4     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> 5     1    1.1 1.1.1     SI_POV_DAY1 Proportion of popu…           1 World      
#> # … with 27,278 more rows, and 16 more variables: TimePeriod <dbl>,
#> #   Value <dbl>, Time_Detail <dbl>, TimeCoverage <lgl>, UpperBound <lgl>,
#> #   LowerBound <lgl>, BasePeriod <lgl>, Source <chr>, GeoInfoUrl <lgl>,
#> #   FootNote <chr>, Age <chr>, Location <chr>, Nature <chr>,
#> #   Reporting_Type <chr>, Sex <chr>, Units <chr>
```

Of course, the reality is that it’s likely easier for us to work outside
the OData filtering framework and directly in R, so here’s a final more
complex example using `dplyr` and `stringr` alongside `goalie` to
automatically download all indicators for Angola with the word “poverty”
in the series description (case insensitive), for the years 1990 to
2005.

``` r
library(dplyr)
library(stringr)

sdg_geoarea_data(24) %>%
  filter(str_detect(str_to_lower(series_description), "poverty")) %>%
  pull(series) %>%
  sdg_data(area_codes = 24, 1990, 2005)
#> # A tibble: 61 x 23
#>    Goal Target Indicator SeriesCode  SeriesDescription   GeoAreaCode GeoAreaName
#>   <dbl> <chr>  <chr>     <chr>       <chr>                     <dbl> <chr>      
#> 1     1 1.1    1.1.1     SI_POV_EMP1 Employed populatio…          24 Angola     
#> 2     1 1.1    1.1.1     SI_POV_EMP1 Employed populatio…          24 Angola     
#> 3     1 1.1    1.1.1     SI_POV_EMP1 Employed populatio…          24 Angola     
#> 4     1 1.1    1.1.1     SI_POV_EMP1 Employed populatio…          24 Angola     
#> 5     1 1.1    1.1.1     SI_POV_EMP1 Employed populatio…          24 Angola     
#> # … with 56 more rows, and 16 more variables: TimePeriod <dbl>, Value <dbl>,
#> #   Time_Detail <dbl>, TimeCoverage <lgl>, UpperBound <lgl>, LowerBound <lgl>,
#> #   BasePeriod <lgl>, Source <chr>, GeoInfoUrl <lgl>, FootNote <chr>,
#> #   Age <chr>, Location <lgl>, Nature <chr>, Reporting_Type <chr>, Sex <chr>,
#> #   Units <chr>
```

And once we have that data, we can then filter, explore, and analyze the
data with our standard R workflow, or even export the downloaded data to
Excel or other analytical tools for further use.
