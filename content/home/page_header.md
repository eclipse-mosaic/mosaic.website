+++
widget = "page_header" # Costum Mosaic feature
headless = true  # This file represents a page section.
active = true  # Activate this widget? true/false
weight = 10  # Order that this section will appear.

logo = "/img/logos/mosaic/EclipseMOSAIC-Logo-RGB-negativ.svg"
logo_title = "Eclipse MOSAIC"
logo_height = "13rem"
#title = "Eclipse MOSAIC"
subtitle = "A Multi-Domain and Multi-Scale Simulation Framework<br>for Connected and Automated Mobility."
alignment = "left" # left or center (default)

[design.background]
  # Background image.
  image = "background-home-02.jpg"
  image_opacity = "1.0"
  
  # Text color (true=light or false=dark).
  text_color_light = false


[[button]]
  title = "Download Now"
  url = "/download/"
  include_latest_release = true
  icon = "fa fa-download"
  background_color = "#000000"

[[button]]
  title = "License"
  url = "/download/#license"
  subtitle = "EPL 2.0"
  icon = "fa fa-coffee"

[[button]]
  title = "Read the Docs"
  url = "/docs/"
  icon = "fa fa-book"

[[button]]
  title = "Tutorials"
  url = "/tutorials/"
  icon = "fa fa-graduation-cap"

[[feature]]
  name = "Co-Simulation Framework"
  description = "Use best in class simulators or couple others using standardized interfaces allowing for quick and easy scenario creation and expansive modeling capabilities!"
#"Eclipse MOSAIC couples best in class simulators together to create complex simulation scenarios quickly and easily. Standardized interfaces allow to couple new simulators without effort to extend the modeling capabilities. " 
  link_title = "Learn More"
  link_url = "/docs/extending_mosaic/core_concepts/"
  link_feature = true
  
[[feature]]
  name = "Multi-Domain / Multi-Scale Models"
  description = "Simulate and combine models of multiple domains, such as traffic and communication, on multiple scales like microscopic-traffic or detailed vehicle simulation !"  

#"The flexibility of Eclipse MOSAIC allows to combine simulation models of multiple domains (e.g. application, traffic, communication) on multiple scales (e.g. microscopic-traffic vs. detailed vehicle simulation)."
  link_title = "Learn More"
  link_url = "#model_collection"
  link_feature = true
  
[[feature]]
  name = "Open for Extensions"
  description = "Eclipse MOSAIC is Open Source under the Eclipse Public License 2.0! Find the source code of the coupling engine, simulators and models, and scenarios on our GitHub page."

#"Eclipse MOSAIC is Open Source under the Eclipse Public License 2.0! You can find the source code of the coupling engine, various simulators and models, and example scenarios on our GitHub page. This makes it very easy to extend the simulation framework by your own models."
  link_title = "Contribute"
  link_url = "/contribution/"
  link_feature = true

+++
