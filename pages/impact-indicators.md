# Impact indicators
<!-- ![example workflow](https://github.com/github/docs/actions/workflows/main.yml/badge.svg) -->

<!-- {:.alert .alert-warning}
This page is not yet complete. -->

## Specification for calculation of impact indicators of Alien Taxa
This document demonstrate the computation of impact indicator of alien species using the `impact_indicator`. The `impact_indicator` feeds in species occurrence cube from the `b3gbi::process_cube` using `taxaFun` and processed Environmental Impact Classification of Alien Taxa (EICAT) impact score of species using `impact_cat`. The code in on GitHub repository https://github.com/mmyahaya/impact.indicator.

## Task 5.3: Indicators on impacts of alien taxa [Lead: SUN]

Integration of impacts with occurrence cubes and other projected cubes can provide estimates of current and potential future impacts of biological invasions. A standardised classification system has been developed for impacts of alien taxa (Blackburn et al. 2014, IUCN 2020) and data on impacts are publicly available in databases such as GRIIS. This task will integrate these data with biodiversity data cubes to demonstrate impact for specific case systems and study taxa, feeding into national indicators, impact of specific species, cumulative impact of several species on specific sites (Wilson et al. 2018), and impact risk (McGeoch et al. 2021). We can also provide estimates of invasion debt and forecast potential impacts spatially and temporally (Rouget et al.2016). This task will show how species traits can be incorporated into cubes to develop indicators.

- Nov 2023: M24 – Development of impact indicator workflow
- Oct 2024: M25 - Design code to calculate the IAS impact indicator
- Apr 2025: D5.3 – Indicators on impacts of alien taxa

## Code to calculate the impact indicator

The impact of invasive alien species is recognised as one of the leading drivers of biodiversity loss, significantly disrupting native ecosystems. This repository provides several types of impact indicator by integrating GBIF occurrences cube and the Environmental Impact Classification for Alien Taxa (EICAT) scoring system. Alien species causes impact at various magnitude, through different mechanisms and regions. This repository is a step towards automated impact indicator to track impact of alien species.

## Authors

  - Mukhtar Yahaya
  - Sabrina Kumschick
  - Sandra MacFadyen
  - Pietro Landi

See also the list of [contributors](pages/working.md) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

- [National Institute for Theoretical and Computational Sciences (NITheCS)](https://nithecs.ac.za/)
