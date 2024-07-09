---
title: 'Automated River Substrate Mapping From Sonar Imagery With Machine Learning'
authors:
- Cameron S. Bodine
- Daniel Buscombe
- Toby D. Hocking
date: '2024-07-09'
publishDate: '2024-07-09T10:00:00'
publication_types:
- article-journal
publication: '*JGR: Machine Learning and Computation*'
doi: 10.1029/2024JH000135
abstract: 'Knowledge of the variation and distribution of substrates at large spatial extents in aquatic systems, particularly rivers, is severely lacking, impeding species conservation and ecosystem restoration efforts. Recreation-grade side-scan sonar (SSS) instruments have demonstrated their unparalleled value as a low-cost scientific instrument capable of efficient and rapid imaging of the benthic environment. However, existing methods for generating georeferenced data sets from these instruments, especially substrate maps, remain a barrier of adoption for scientific inquiry due to the high degree of human intervention and required expertise. To address this shortcoming, we introduced PING-Mapper, an open-source and freely available Python-based software for automatically generating geospatial benthic data sets from popular Humminbird® instruments reproducibly. The previously released Version 1.0 of the software provided automated workflows for exporting georeferenced sonar imagery. This study extends functionality with version 2.0 by incorporating semantic segmentation with deep neural networks to reproducibly map substrates at large spatial extents. We present a novel approach for generating label-ready sonar data sets, creating label-image training sets, and model training with transfer learning; all with open-source tools. The six-class substrate model achieves an overall accuracy of 78% and the best performing class achieves 91%. Grouping substrates into three classes further improves the overall accuracy (87%) and best performing class accuracy (94%). Additional workflows enable masking sonar shadows, calculating independent bed picks and correcting attenuation effects in the imagery to improve interpretability. This software provides an improved mechanism for automatically mapping substrate distribution from recreation-grade SSS systems, thereby lowering the barrier for inclusion in wider aquatic research.'

# Summary. An optional shortened abstract.
# summary:

tags:
- side-scan sonar
- Humminbird
- aquatic substrate
- remote sensing
- deep learning
- segmentation
featured: true

links:
- name: URL
  url: https://doi.org/10.1029/2024JH000135
url_pdf: https://agupubs.onlinelibrary.wiley.com/doi/epdf/10.1029/2024JH000135
url_code: 'https://doi.org/10.5281/zenodo.10120054'
# url_dataset: '#'
# url_poster: '#'
url_project: 'https://github.com/CameronBodine/PINGMapper'
# url_slides: ''
# url_source: '#'
# url_video: '#'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: 'Image credit: [**Bodine, Buscombe, Hocking (2024)**](https://doi.org/10.1029/2024JH000135)'
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
- ping-mapper

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
# slides: example
---

<!-- {{% callout note %}}
Create your slides in Markdown - click the *Slides* button to check out the example.
{{% /callout %}}

Add the publication's **full text** or **supplementary notes** here. You can use rich formatting such as including [code, math, and images](https://docs.hugoblox.com/content/writing-markdown-latex/). -->




---
