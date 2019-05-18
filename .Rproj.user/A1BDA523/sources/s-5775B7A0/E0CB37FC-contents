#' ---
#' title: "Tornado Project"
#' author: ""
#' date: ""
#' output:
#'   html_document:
#'     keep_md: true
#'     theme: simplex
#'     highlight: monochrome
#' ---
#+ init, include=FALSE
knitr::opts_chunk$set(message = FALSE, warning = FALSE, dev="png", collapse=TRUE,
                      fig.retina = 2, fig.width = 10, fig.height = 6)

#+ libs
library(hrbrthemes) # not 100% necessary
library(tidyverse)  # data wrangling & ggplot2

#+ data
tibble(
  lat = scan(here::here("data/lats.txt")),
  lon = scan(here::here("data/lons.txt")),
  trend = scan(here::here("data/trends.txt"))
) -> tornado

summary(tornado)

#' Grid overview

#+ grid-overview
ggplot(tornado, aes(lon, lat)) +
  geom_point(aes(color = trend))

#' Trend overview

#+ trend-overview
ggplot(tornado, aes(trend)) +
  geom_histogram() +
  scale_x_continuous(breaks = seq(-0.5, 0.5, 0.05))

maps::map("state", ".", exact = FALSE, plot = FALSE, fill = TRUE) %>%
  fortify(map_obj) %>%
  as_tibble() -> state_map

xlim <- range(state_map$long)
ylim <- range(state_map$lat)

filter(
  tornado,
  between(lon, -107, xlim[2]), between(lat, ylim[1], ylim[2]), # -107 gets us ~left-edge of TX
  ((trend < -0.07) | (trend > 0.07)) # approximates notebook selection range
) -> tornado

#' Grid overview #2

#+ grid-overview-2
ggplot(tornado, aes(lon, lat)) +
  geom_point(aes(color = trend))

#' Grid overview #2

#+ grid-overview-3
ggplot(tornado, aes(lon, lat)) +
  geom_tile(aes(fill = trend, color = trend))

#+ map-1
ggplot() +
  geom_tile(
    data = tornado,
    aes(lon, lat, fill = trend, color = trend)
  ) +
  geom_map(
    data = state_map, map = state_map,
    aes(long, lat, map_id = region),
    color = "black", size = 0.125, fill = NA
  )


#+ map-final
c(
  "#023858", "#045a8d", "#0570b0", "#3690c0", "#74a9cf",
  "#a6bddb", "#d0d1e6", "#ece7f2", "#fff7fb", "#ffffff",
  "#ffffcc", "#ffeda0", "#fed976", "#feb24c", "#fd8d3c",
  "#fc4e2a", "#e31a1c", "#bd0026", "#800026"
) -> grad_cols

ggplot() +
  geom_tile(
    data = tornado,
    aes(lon, lat, fill = trend, color = trend)
  ) +
  geom_map(
    data = state_map, map = state_map,
    aes(long, lat, map_id = region),
    color = ft_cols$slate, size = 0.125, fill = NA
  ) +
  borders("usa", colour = "black", size = 0.5) +
  scale_colour_gradientn(
    colours = grad_cols,
    labels = c("Fewer", rep("", 4), "More"),
    name = "Change in tornado frequency, 1979-2017"
  ) +
  scale_fill_gradientn(
    colours = grad_cols,
    labels = c("Fewer", rep("", 4), "More"),
    name = "Change in tornado frequency, 1979-2017"
  ) +
  coord_map(
    projection = "polyconic",
    xlim = scales::expand_range(range(tornado$lon), add = 2),
    ylim = scales::expand_range(range(tornado$lat), add = 2)
  ) +
  guides(
    colour = guide_colourbar(
      title.position = "top", title.hjust = 0.5
    ),
    fill = guide_colourbar(
      title.position = "top", title.hjust = 0.5
    )
  ) +
  labs(
    x = NULL, y = NULL
  ) +
  theme_ipsum_rc(grid="") +
  theme(axis.text = element_blank()) +
  theme(legend.position = "top") +
  theme(legend.title = element_text(size = 16, hjust = 0.5)) +
  theme(legend.key.width = unit(4, "lines")) +
  theme(legend.key.height = unit(0.5, "lines"))
