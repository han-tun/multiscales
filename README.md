
<!-- README.md is generated from README.Rmd. Please edit that file -->

# multiscales

Multivariate scales for ggplot2, written by Claus O. Wilke

## Installation

This package can be installed from github. It requires the development
version of the colorspace package. It also requires ggplot2 3.0.0, which
was released on July 3,
    2018.

    install.packages("colorspace", repos = "http://R-Forge.R-project.org")
    devtools::install_github("clauswilke/multiscales")

This is an experimental package. Use at your own risk. API is not
stable. No user support provided.

## Examples

Visualizing the lead/lag of Clinton vs. Trump in the 2016 presidential
election jointly with the uncertainty of the lead/lag estimates. This
visualization shows that for many states the outcome was difficult to
predict. (Example taken from Correll et al., [Value-Suppressing
Uncertainty
Palettes](https://idl.cs.washington.edu/files/2018-UncertaintyPalettes-CHI.pdf),
CHI 2018.)

``` r
library(ggplot2)
library(multiscales)

colors <- scales::colour_ramp(
  colors = c(red = "#AC202F", purple = "#740280", blue = "#2265A3")
)((0:7)/7)

ggplot(US_polling) + 
  geom_sf(aes(fill = zip(Clinton_lead, moe_normalized)), color = "gray30", size = 0.2) + 
  coord_sf(datum = NA) +
  bivariate_scale("fill",
    pal_vsup(values = colors, max_desat = 0.8, pow_desat = 0.2, max_light = 0.7, pow_light = 1),
    name = c("Clinton lead", "uncertainty"),
    limits = list(c(-40, 40), c(0, 1)),
    breaks = list(c(-40, -20, 0, 20, 40), c(0, 0.25, 0.50, 0.75, 1.)),
    labels = list(waiver(), scales::percent),
    guide = "colourfan"
  ) +
  theme_void() +
  theme(
    legend.key.size = grid::unit(0.8, "cm"),
    legend.title.align = 0.5,
    plot.margin = margin(5.5, 20, 5.5, 5.5)
  )
```

![](man/figures/README-unnamed-chunk-2-1.png)<!-- -->

For comparison, the same plot with a univariate color scale. Not it
appears that in every state we know who is in the lead.

``` r
ggplot(US_polling) + 
  geom_sf(aes(fill = Clinton_lead), color = "gray30", size = 0.2) + 
  coord_sf(datum = NA) +
  scale_fill_gradient2(
    low = "#AC202F", mid = "#740280", high = "#2265A3",
    name = c("Clinton lead", "uncertainty"),
    limits = c(-40, 40),
    breaks = c(-40, -20, 0, 20, 40),
    guide = guide_colorbar(
      direction = "horizontal",
      label.position = "bottom",
      title.position = "top",
      barwidth = grid::unit(4.0, "cm")
    )
  ) +
  theme_void() +
  theme(
    legend.title.align = 0.5,
    legend.key.size = grid::unit(0.8, "cm"),
    plot.margin = margin(5.5, 20, 5.5, 5.5)
  )
```

![](man/figures/README-unnamed-chunk-3-1.png)<!-- -->
