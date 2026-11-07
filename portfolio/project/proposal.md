---
id: litvis

narrative-schemas:
  - ../../lectures/narrative-schemas/proposal.yml
---

@import "../../lectures/css/datavis.less"

# Project Proposal

{(whoami|}
Eren Erguvan
eren.erguvan@city.ac.uk
{|whoami)}

{(task|}

You should complete this datavis project proposal by **Sunday 8th November, 5pm UK time** ensuring you have committed and pushed it to GitHub. Successful submissions by that time will be awarded up to **5 marks** towards your coursework assessment total.

{|task)}

{(questions|}

**Project Topic: Changes in London's Public Transport Usage and Passenger Demand**

1. How has passenger demand across London's major public transport modes (Underground, buses, DLR and Overground) changed since 2010?

2. How have different public transport modes recovered following the COVID-19 pandemic, and which have experienced the greatest changes compared with pre-pandemic usage?

3. How do Underground and bus passenger numbers change during selected periods of industrial action compared with similar non-strike periods?

4. Which London Underground stations experience the highest and lowest passenger demand, and what differences can be identified between central London and outer London stations?

5. How do passenger demand patterns at Oakwood and Angel Underground stations differ, particularly in terms of changes over time, weekday and weekend usage, and peak travel periods?

{|questions)}

{(datasources|}

I will use publicly available datasets from Transport for London (TfL) and the London Datastore:

1. **[Public Transport Journeys by Type of Transport](https://data.london.gov.uk/dataset/public-transport-journeys-by-type-of-transport-ep8ow)** – Contains passenger journey figures for buses, Underground, DLR, Overground and other transport modes from April 2010 onwards. This will be used to investigate long-term trends and post-pandemic recovery (RQ1 and RQ2).

2. **[TfL Network Demand Data](https://tfl.gov.uk/info-for/open-data-users/our-open-data)** – Provides daily Tube and bus journey counts and station-level entry/exit data from 2019 onwards. This will support comparisons during selected strike periods and changes in station demand (RQ3 and RQ5).

3. **[TfL Annual Counts and NUMBAT](https://crowding.data.tfl.gov.uk/)** – Provides estimates of Underground station usage, including entry/exit figures and time-of-day patterns. This will allow comparisons of busy and quieter stations, as well as differences between Oakwood and Angel (RQ4 and RQ5).

4. **[TfL Freedom of Information – Industrial Action Information](https://tfl.gov.uk/corporate/transparency/freedom-of-information/foi-request-detail?referenceId=FOI-2633-2526)** – Supporting information for identifying selected periods of industrial action to compare against daily passenger demand (RQ3).

{|datasources)}

{(justification|}

I chose this topic because London's public transport network is something I use very regularly, and I am interested in understanding how passenger demand has changed over time. I particularly want to investigate the differences between transport modes, how they have recovered following the COVID-19 pandemic, and how disruptions such as industrial action relate to passenger usage. Comparing Oakwood, my local Underground station, with Angel, which is near my university, also provides a more personal perspective on the differences between outer and inner London travel patterns.

Data visualisation is well suited to this project because it allows users to identify long-term trends, compare different transport modes, and explore variations in station usage that may be difficult to recognise from raw data alone. Using time-series charts, bar charts and potentially geographical visualisations would help communicate these patterns and answer the research questions.

The TfL and London Datastore datasets were chosen because they are publicly accessible, cover different aspects of London's transport network, and contain quantitative and time-based data suitable for analysis using Elm, Vega-Lite and Tidy. However, differences in reporting periods, data coverage and estimation methods will need to be considered to avoid misleading comparisons, particularly when investigating industrial action.

{|justification)}
