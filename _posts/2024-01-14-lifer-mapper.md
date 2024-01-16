---
title: 'Mapping Potential Lifers'
date: 2024-01-14
permalink: /posts/2024/01/lifer-mapper/
tags:
  - ebird
  - birding
  - maps
  - R
---

[eBird Status & Trends](https://science.ebird.org/en/status-and-trends) products are a collection of datasets describing the distribution and abundance of bird populations. While these products have been utilized for scientific and conservation purposes, there is also potential to incorporate them into tools designed specifically for recreational birders. Here I leverage Status & Trends data to map the distribution and number of bird species I've yet to see--my potential "lifers." Where should I go if I'd like to see new birds? When should I go there? These are familiar questions to birders, and I wanted to approach answering them in a rigorous and visually-appealing way.

First, I used my personal eBird data download and the [`rebird`](https://docs.ropensci.org/rebird/) package to compile a list of all bird species in the US that I haven't encountered and downloaded the occurrence layers for these species with [`ebirdst`](https://ebird.github.io/ebirdst/). With [`terra`](rspatial.org/terra) I stacked the occurrence layers and counted the number of species with occurrence probabilities greater than 5% in each grid cell and week. The resulting layers provide weekly estimates of my potential lifers at each location/date. I used the [`tidyterra`](https://dieghernan.github.io/tidyterra/) package to plot the individual rasters and the [`magick`](https://docs.ropensci.org/magick/) package to stitch them together, giving me an animated summary of the birds I've yet to see.

![](/images/posts/2024-01-14-lifer-mapper/US_Possible_lifers_annual_lores.gif)

Looking at my map it's clear I need to get to SE Arizona, where there are up to 34 potential lifers in a single 27x27 km grid cell (on August 9th). South Texas is not far behind. It is fun to see the Dakotas light up during the breeding season (I'm missing some prairie specialists like Baird's Sparrow and Sprague's Pipit). The Southeastern plains and pine woodlands warrant attention (Brown-headed Nuthatch, Bachman's Sparrow, Red-cockaded Woodpecker). South Florida too (Florida Scrub Jay, Snail Kite, Short-tailed Hawk, Mottled Duck, etc.).

There are many potential variations of this map. I experimented with a version that simply sums the occurrence probabilities of all species, giving something like an "expected" count of lifers at a given location and date, but this is harder to interpret since the occurrence probabilities estimate the expected likelihood of encountering a species on a 2 km and 1 hour long travelling checklist under optimal conditions, which differ from species to species (and it's not as though birders are limited to a single checklist/outing when visiting a new location). Thus, I opted for the simpler approach--summing the total number of species with a reasonable chance of being encountered. In the future I'd also like to explore weighting the probabilities based on the species range (e.g., a location with 5 potential lifers that range across the whole continent wouldn't have the same visual weight as a location with 5 potential lifers highly localized to that area). By providing an "empty" life list, the code can also generate a dynamic annual map estimating total species richness.

![](/images/posts/2024-01-14-lifer-mapper/US_Species_richness_annual_lores.gif)

R code to genreate lifer maps for yourself is [here](https://github.com/smsfrn/lifR). I've designed the code to work for any individual US state, and it can be easily adapted for other geographies, though runs in other countries would currently be limited by the number of species with available Status & Trends products. **Fair warning**: this analysis requires a machine with substantial working memory. I can run individual states on my laptop, but for the entire US I need to offload the workflow to an HPC system. Fortunately, thanks to `ebirdst` and `terra`, the actual processing time is remarkably fast.
