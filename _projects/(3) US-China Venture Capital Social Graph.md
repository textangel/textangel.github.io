---
name: US-China Venture Capital Social Graph
tools: [Python, BeautifulSoup, MongoDB]
image: /assets/images/social-graph.jpg
description: As part of my Master's Thesis, I scraped around 4 years of Chinese venture capital investment data from a public source and compared it to venture capital investment data in the United States. I was interested in whether the rate of co-investment of established early-stage VC firms with less established early stage firms was higher in China or the US (answer, the former). This became quite a heavily involved data procurement, cleaning, management, and analysis project. 
external_url: https://github.com/textangel/usa-china-venture-capital-graph
---

Over the course of two months, ~50,000 datapoints representing VC investment events were scraped from a popular Chinese investment information aggregator using **BeautifulSoup**, cleaned, and loaded into a MongoDB Database. This data was compared with publicly available data from crunchbase.com, representing the state of US VC investment up to 2013.

I performed comparative analysis of the two datasets, primary from a venture capital co-investment perspective. Co-investment graphs were built from events in which two VC firms invested in the same round of a company. Graphs were generated for different years and for different investment rounds. Network analysis was applied to the dataset to determine the prevalence of co-investment amongst and between different VC tiers.
