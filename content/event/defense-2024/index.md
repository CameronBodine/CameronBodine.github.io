---
title: Automated Mapping of Gulf Sturgeon Spawning Habitat in Coastal Plain Rivers

event: Ph.D. Dissertation Defense
# event_url: https://units.fisheries.org/tn/sdafs2024chattanooga/

location: Northern Arizona University
address:
  # street: 450 Serra Mall
  city: Flagstaff
  region: AZ
  # postcode: '94305'
  country: United States

summary: Public presentation given for Ph.D. Dissertation Defense
abstract: 'Gulf sturgeon are a threatened anadromous fish species with spawning populations found in seven Gulf of Mexico coastal plain rivers (east to west: Suwannee, Apalachicola, Choctawhatchee, Yellow, Escambia/Conecuh, Pascagoula, and Pearl/Bogue Chitto rivers). Relatively little is known about spawning habitats located in the Pearl and Pascagoula River Basins as only one confirmed spawning location has been identified. Characterizing, locating, and quantifying existing suitable spawning habitat is critical to species recovery and restoration efforts. While air and space-borne remote sensing provides invaluable insight for mapping terrestrial habitats, these approaches are limited in their application to locating Gulf Sturgeon spawning habitats due to river stage, turbidity, and canopy cover, requiring direct observation of conditions in the field. Recreation-grade side-scan sonar (SSS) instruments have demonstrated their unparalleled value as a low-cost scientific instrument capable of efficient and rapid imaging of the benthic environment. However, existing methods for generating georeferenced datasets from these instruments, especially substrate maps, remains a barrier of adoption for general scientific inquiry due to the high degree of human-intervention and required expertise. To address this short-coming, I introduce PING-Mapper; an open-source and freely available Python-based software for automatically generating geospatial benthic datasets from popular Humminbird instruments reproducibly. The modular software automatically: 1) decodes sonar recordings; 2) exports ping attributes from every sonar channel; 3) uses sonar sensor depth for water column removal; and 4) exports sonogram tiles and georectified mosaics. I further extend functionality of the software by incorporating semantic segmentation with deep neural networks to reproducibly map substrates at large spatial extents. I present a novel approach for generating label-ready sonar datasets, creating label-image training sets, and model training with transfer learning; all with open-source tools. The substrate models achieve an overall accuracy of 78% on six classes and 87% on three classes combined based on experiments on test datasets not used in model training. Additional workflows enable masking sonar shadows, calculating independent bedpicks, and correcting attenuation effects in the imagery to improve interpretability. I use PING-Mapper to map substrate and bedforms across 1,200 river kilometers (RKM) in the Pearl and Pascagoula River Basins for the first time, with no human intervention. Overall fuzzy map accuracies determined from field verification achieve 68.7 to 69.3%. The maps are synthesized to identify potential Gulf Sturgeon spawning habitat which will aid prioritization and evaluation of critical habitat restoration projects. This novel software provides an improved mechanism for automatically mapping substrate distribution from recreation-grade SSS systems, thereby lowering the barrier for inclusion in wider aquatic research.'

# Talk start and end times.
#   End time can optionally be hidden by prefixing the line with `#`.
date: '2024-03-29'
# date_end: '2023-05-18'
all_day: true

# Schedule page publish date (NOT talk date).
publishDate: '2024-04-04'

authors: 
- Cameron S. Bodine
tags:
- side-scan sonar
- Humminbird
- aquatic substrate
- remote sensing
- Gulf Sturgeon
- deep learning
- machine learning
- segmentation
- mapping
- GIS

# Is this a featured talk? (true/false)
featured: true

image:
  caption: 'Image credit: Bodine. (2023)'
  # focal_point: 'Right'

links:
  # - icon: twitter
  #   icon_pack: fab
  #   name: Follow
  #   url: https://twitter.com/georgecushen
url_code: 'https://github.com/CameronBodine/PINGMapper'
url_pdf: ''
url_slides: ''
url_video: 'https://youtu.be/KboFOv5ySjQ?si=ExbaQUrUvEkIqJ81'

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






