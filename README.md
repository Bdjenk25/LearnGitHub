# LearnGitHub
#### STAPI

Everyone can code because we are part of the matrix

Use the Star Trek API (STAPI) to obtain information on the infamous character, Q. Specifically, retrieve data on his appearances and the stardates when he shows up. The first API call does a lightweight, unobtrusive check to see how many pages of potential search results exist for characters in the database. There are a lot of characters. The second call grabs only page two results. The third call uses the universal/unique ID `uid` to retrieve data on Q. Think of these three successive uses of `stapi` as safe mode, search mode and extraction mode.

``` r
library(rtrek)
library(dplyr)
stapi("character", page_count = TRUE)
#> Total pages to retrieve all results: 64

stapi("character", page = 2)
#> # A tibble: 100 x 24
#>    uid   name  gender yearOfBirth monthOfBirth dayOfBirth placeOfBirth
#>    <chr> <chr> <chr>        <int> <lgl>        <lgl>      <chr>       
#>  1 CHMA~ Stev~ <NA>            NA NA           NA         <NA>        
#>  2 CHMA~ Yegg~ M               NA NA           NA         <NA>        
#>  3 CHMA~ Arex  M               NA NA           NA         <NA>        
#>  4 CHMA~ Jose~ M               NA NA           NA         <NA>        
#>  5 CHMA~ J. Z~ <NA>            NA NA           NA         <NA>        
#>  6 CHMA~ Doyle M               NA NA           NA         <NA>        
#>  7 CHMA~ Butl~ M               NA NA           NA         <NA>        
#>  8 CHMA~ Lito  M               NA NA           NA         <NA>        
#>  9 CHMA~ B. M~ <NA>            NA NA           NA         <NA>        
#> 10 CHMA~ Anna~ <NA>            NA NA           NA         <NA>        
#> # ... with 90 more rows, and 17 more variables: yearOfDeath <int>,
#> #   monthOfDeath <lgl>, dayOfDeath <lgl>, placeOfDeath <lgl>,
#> #   height <int>, weight <int>, deceased <lgl>, bloodType <lgl>,
#> #   maritalStatus <chr>, serialNumber <chr>, hologramActivationDate <lgl>,
#> #   hologramStatus <lgl>, hologramDateStatus <lgl>, hologram <lgl>,
#> #   fictionalCharacter <lgl>, mirror <lgl>, alternateReality <lgl>

Q <- "CHMA0000025118"  #unique ID
Q <- stapi("character", uid = Q)
Q$episodes %>% select(uid, title, stardateFrom, stardateTo)
#>              uid                 title stardateFrom stardateTo
#> 1 EPMA0000001458    All Good Things...      47988.0    47988.0
#> 2 EPMA0000001329                 Q Who      42761.3    42761.3
#> 3 EPMA0000001377                  Qpid      44741.9    44741.9
#> 4 EPMA0000000483 Encounter at Farpoint      41153.7    41153.7
#> 5 EPMA0000000651              Tapestry           NA         NA
#> 6 EPMA0000000845                Q-Less      46531.2    46531.2
#> 7 EPMA0000162588            Death Wish           NA         NA
#> 8 EPMA0000001413                True Q      46192.3    46192.3
#> 9 EPMA0000001510    The Q and the Grey      50384.2    50392.7
```

### Memory Alpha

Obtain content and metadata from the article about Spock on Memory Alpha:

``` r
x <- ma_article("Spock")
x
#> # A tibble: 1 x 4
#>   title content           metadata          categories       
#>   <chr> <list>            <list>            <list>           
#> 1 Spock <S3: xml_nodeset> <tibble [1 x 17]> <tibble [14 x 2]>
x$metadata[[1]]$Born
#> [1] "January 6, 2230 (stardate 2230.06)|ShiKahr, Vulcan"
```

### Memory Beta

Spock was born in 2230. Obtain a subset of the Star Trek universe historical timeline for that year:

``` r
mb_timeline(2230)
#> 2230
#> $events
#> # A tibble: 5 x 4
#>   period id            date  notes                                         
#>   <chr>  <chr>         <chr> <chr>                                         
#> 1 2230   Events        <NA>  Argelius II  and Betelgeuse become members of~
#> 2 2230   Births_and_D~ <NA>  Spock is born deep within a cave in Vulcan's ~
#> 3 2230   Births_and_D~ <NA>  George Samuel Kirk, Jr. is born.[5]           
#> 4 2230   Births_and_D~ <NA>  David Rabin is born.[6]                       
#> 5 2230   Births_and_D~ <NA>  Roy John Moss is born.[7]                     
#> 
#> $stories
#> # A tibble: 5 x 11
#>   title title_url colleciton collection_url section context series date 
#>   <chr> <chr>     <chr>      <chr>          <chr>   <chr>   <chr>  <chr>
#> 1 Burn~ Burning_~ <NA>       <NA>           Chapte~ <NA>    The O~ 2230 
#> 2 Star~ Star_Tre~ <NA>       <NA>           Chapte~ <NA>    The O~ 2230 
#> 3 IDW ~ IDW_Star~ Star Trek~ Star_Trek_(ID~ 2230 f~ <NA>    The O~ 2230 
#> 4 Star~ Star_Tre~ <NA>       <NA>           Chapte~ <NA>    The O~ 2230 
#> 5 Sarek Sarek_(n~ <NA>       <NA>           Chapte~ <NA>    The O~ 12 N~
#> # ... with 3 more variables: media <chr>, notes <chr>, image_url <chr>
```

Live long and prosper.

See the [introduction vignette](https://leonawicz.github.io/rtrek/articles/rtrek.html) for more details and examples.

Reference
---------

[Complete package reference and function documentation](https://leonawicz.github.io/rtrek/)

------------------------------------------------------------------------

Please note that the `rtrek` project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By contributing to this project, you agree to abide by its terms.