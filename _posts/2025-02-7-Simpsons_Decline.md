---
layout: post
title: "Tidy Tuesday: 2025-01-04, Simpsons Graphs Pt. 1"
categories: [Portfolio, TidyTuesday, Visualizations]
tags: [Data, Visualization, TidyTuesday]
date: 2025-02-07 14:01:30 -0500
---

Another entry for [TidyTuesday](https://github.com/rfordatascience/tidytuesday/tree/main). I'm a little late to the party this time. School has been rough.

![Simpsons Visualization](/assets/img/simpsons_decline.png){: .normal }

As you can see, as time passed by, *The Simpsons* got worse and worse throughout the 2010s (according to IMDB data). 

This is absolutely not my best work. I do, however, plan to explore this dataset a lot more. You'll see a lot more Simpsons content from this point forward. 

Here's the code I used to produce the graph:

```
library(tidyverse)
library(tidytuesdayR)
library(extrafont)

#font_import()
#loadfonts(device = "win")

tuesdata <- tidytuesdayR::tt_load(2025, week = 5)
simpsons_episodes <- tuesdata$simpsons_episodes

#mean = 6.787838
#mean(simpsons_episodes$imdb_rating, na.rm = TRUE) 

simpsons_episodes <- simpsons_episodes |>
  mutate(
    Rating = ifelse(imdb_rating > 6.787838, "Above (or Equal To) Average", "Below Average"),
  ) |>
  na.omit() |>
  filter(season < 28)

simpsons_episodes |>
  ggplot(aes(x = season, fill = Rating)) +
  geom_bar(position = "fill") +
  coord_flip() +
  labs(y = "Proportion of Episodes per Season",
       title = "Decline of Simpsons Episode Quality Throughout the 2010s, by Season",
       x = "Season No.",
       fill = "IMDB Rating",
       subtitle = "Average IMDB Rating: ~6.79 Stars"
       ) +
  scale_x_continuous(breaks=seq(20,27,1)) + 
  scale_fill_manual(values=c("#00A9FF", 
                             "#FF5600")) +
  theme_bw() +
  theme(text=element_text(family="Noto Serif"),
        axis.title.x = element_text(margin = margin(t = 10)),
        axis.title.y = element_text(margin = margin(r = 7))
  )
```

**References**

1. Data Science Learning Community (2024). Tidy Tuesday: A weekly social data project. https://tidytues.day.