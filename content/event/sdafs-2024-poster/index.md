---
title: Efficient and Reproducible Substrate Mapping with Recreation-grade Sonar Systems [Poster Session]

event: Southern Division American Fisheries Society Annual Meeting
event_url: https://units.fisheries.org/tn/sdafs2024chattanooga/

location: University of Colorado
address:
  # street: 450 Serra Mall
  city: Chattanooga
  region: TN
  # postcode: '94305'
  country: United States

summary: Poster presented at Southern Division American Fisheries Society Annual Meeting.
abstract: 'Knowledge of the variation and distribution of substrates at large spatial extents in aquatic systems is severely lacking, impeding species conservation and habitat restoration efforts. The introduction of recreation-grade side scan sonar (SSS) instruments, or fishfinders, in the 2000’s has provided researchers with an instrument that is easy-to-deploy and operate, enabling rapid surveys of these environments. However, existing methods for processing sonar mosaics and generating substrate maps requires a high degree of human-intervention and expertise, which limits the accessibility, efficiency, and reproducibility of these approaches. An open-source and automated tool for generating geospatial datasets from recreation-grade sonar instruments is needed to help increase our understanding of aquatic habitats at the site and landscape-level. We introduce PING-Mapper, an open-source and freely available Python-based software for generating geospatial benthic datasets from recreation-grade SSS systems. PING-Mapper is an end-to-end framework for surveying and mapping aquatic systems at large spatial extents reproducibly, with minimal intervention from the user. Version 1.0 of the software (Summer 2022) decodes sonar recordings from any Humminbird® side imaging system, export plots of sonar intensities and sensor-derived bedpicks and generates georeferenced mosaics of geometrically corrected sonar imagery. Version 2.0 of the software, released Fall 2023, extends PING-Mapper functionality by incorporating deep neural network models that automatically locate and mask sonar shadows, calculate independent bedpicks from both side scan channels, and classify substrates at the pixel level. An additional workflow enables normalization of sonar intensity values, or backscatter, effectively correcting attenuation effects and improving overall contrast of the sonar mosaics. This software provides the aquatic research community with an efficient means of surveying aquatic systems and generating substrate maps which will inform fish sampling efforts, habitat suitability models, and planning and monitoring habitat restoration.'

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: '2024-02-03'
# date_end: '2023-05-18'
all_day: true

# Schedule page publish date (NOT talk date).
publishDate: '2023-11-22'

authors: 
- Cameron S. Bodine
- Daniel Buscombe
tags:
- side-scan sonar
- Humminbird
- aquatic substrate
- remote sensing

# Is this a featured talk? (true/false)
featured: true

image:
  caption: 'Image credit: Bodine & Buscombe. (2024)'
  # focal_point: 'Right'

links:
  # - icon: twitter
  #   icon_pack: fab
  #   name: Follow
  #   url: https://twitter.com/georgecushen
url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''

# Markdown Slides (optional).
#   Associate this talk with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
# slides: example

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects:
  - ping-mapper

---

<!-- {{% callout note %}}
Click on the **Slides** button above to view the built-in slides feature.
{{% /callout %}}

Slides can be added in a few ways:

- **Create** slides using Hugo Blox Builder's [_Slides_](https://docs.hugoblox.com/reference/content-types/) feature and link using `slides` parameter in the front matter of the talk file
- **Upload** an existing slide deck to `static/` and link using `url_slides` parameter in the front matter of the talk file
- **Embed** your slides (e.g. Google Slides) or presentation video on this page using [shortcodes](https://docs.hugoblox.com/reference/markdown/).

Further event details, including [page elements](https://docs.hugoblox.com/reference/markdown/) such as image galleries, can be added to the body of this page. -->






