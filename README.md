
<!-- README.md is generated from README.Rmd. Please edit that file -->

# pkg-downloads

<!-- badges: start -->

[![render-rmarkdown](https://github.com/kelly-sovacool/pkg-downloads/actions/workflows/render-rmarkdown.yaml/badge.svg)](https://github.com/kelly-sovacool/pkg-downloads/actions/workflows/render-rmarkdown.yaml)
<!-- badges: end -->

[`mikropml`](https://github.com/SchlossLab/mikropml) package download
counts from `cranlogs`

``` r
library(cranlogs)
library(cowplot)
library(glue)
library(magick)
library(rsvg)
library(tidyverse)
```

## Download the downloads

``` r
downloads <- cran_downloads(package = "mikropml",
                            from = "2020-11-23") %>%
    mutate(cum_count = cumsum(count))
write_csv(downloads, here::here('data', 'downloads.csv'))
tail(downloads)
#>            date count  package cum_count
#> 1368 2024-08-21    18 mikropml     19930
#> 1369 2024-08-22    47 mikropml     19977
#> 1370 2024-08-23    15 mikropml     19992
#> 1371 2024-08-24    13 mikropml     20005
#> 1372 2024-08-25     0 mikropml     20005
#> 1373 2024-08-26     0 mikropml     20005
```

## Get the badge

``` r
badge_url <- "https://cranlogs.r-pkg.org/badges/grand-total/mikropml"
badge_img <- magick::image_read_svg(badge_url, width = 1000)
```

## Plot over time

``` r

downloads_plot <- downloads %>% 
    ggplot(aes(date, cum_count)) + 
    geom_line(color = '#c882fc') + 
    scale_x_date(date_labels = "%b %Y") + 
    theme_bw() + 
    labs(x = '', y = 'downloads to date', 
         title = 'mikropml downloads from CRAN',
         caption = glue("last updated: {Sys.Date()}"))

ggdraw() +
    draw_plot(downloads_plot) +
    draw_image(badge_img, 
               x = 0.99, y = 0.99, 
               hjust = 1, vjust = 1, halign = 1, valign = 1,
               width = 0.15)
```

![](figures/plot-downloads-time-1.png)<!-- -->

``` r
ggdraw() +
    draw_plot(downloads_plot)
```

![](figures/plot-downloads-time_no-badge-1.png)<!-- -->
