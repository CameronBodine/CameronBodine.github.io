---
title: Reproducible Substrate Mapping with Recreation-grade Sonar Systems

event: American Fisheries Society Annual Meeting
# event_url: https://example.org

# location: Hugo Blox Builder HQ
address:
  # street: 450 Serra Mall
  city: Grand Rapids
  region: MI
  # postcode: '94305'
  country: United States

summary: Presentation given at 2023 American Fisheries Society Annual Meeting
abstract: 'Predictive understanding of the variation and distribution of substrates at large spatial extents in aquatic systems is severely lacking. This hampers efforts to numerically predict the occurrence and distribution of specific benthic habitats important to aquatic species, which must be observed in the field. Existing survey methods are limited in scale, require heavy and technically sophisticated survey equipment, or are prohibitively expensive for surveying and mapping. Recreation-grade side scan sonar (SSS) instruments, or fishfinders, have demonstrated their unparalleled value in a lightweight and easily-to-deploy system to image benthic habitats efficiently at the landscape-level. Existing methods for generating geospatial datasets from these sonar systems require a high-level of interaction from the user and are primarily closed-source, limiting opportunities for community-driven enhancements. We introduce PING-Mapper, an open-source and freely available Python-based software for generating geospatial benthic datasets from recreation-grade SSS systems. PING-Mapper is an end-to-end framework for surveying and mapping aquatic systems at large spatial extents reproducibly, with minimal intervention from the user. Version 1.0 of the software (Summer 2022) decodes sonar recordings from any existing Humminbird® side imaging system, export plots of sonar intensities and sensor-derived bedpicks and generates georeferenced mosaics of geometrically corrected sonar imagery. Version 2.0 of the software, to be released Summer 2023, extends PING-Mapper functionality by incorporating deep neural network models that automatically locate and mask sonar shadows, calculate independent bedpicks from both side scan channels, and classify substrates at the pixel level. The widespread availability of substrate information in aquatic systems will inform fish sampling efforts, habitat suitability models, and planning and monitoring habitat restoration.'

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: '2023-08-21'
# date_end: '2030-06-01T15:00:00Z'
all_day: false

# Schedule page publish date (NOT talk date).
publishDate: '2023-11-21'

authors: 
- Cameron S. Bodine
- Daniel Buscombe
- Adam J. Kaeser
tags:
- side-scan sonar
- Humminbird
- aquatic substrate
- remote sensing

# Is this a featured talk? (true/false)
featured: false

image:
  caption: 'Image credit: Bodine et al. (2023)'
  # focal_point: 'Right'

links:
  # - icon: twitter
  #   icon_pack: fab
  #   name: Follow
  #   url: https://twitter.com/georgecushen
url_code: ''
url_pdf: ''
url_slides: ''
url_video: 'https://youtu.be/AlebxkKn83c?si=7Z_xedpzoOFw7l0Z'

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






