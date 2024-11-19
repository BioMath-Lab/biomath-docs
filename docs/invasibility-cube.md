# Invasibility cube
![example workflow](https://github.com/github/docs/actions/workflows/main.yml/badge.svg)

<!-- {:.alert .alert-warning}
This page is not yet complete. -->

## Specification for invasibility cubes and their production
This document presents the specification for "invasibility cubes”, to visualise invasion fitness and invasibility. It provides a workflow to visualise invasion fitness, using species occurrence data, Invasive Alien Species (IAS) lists, and species traits to calculate trait centrality, visualise trait dispersion, estimate interaction strength, and assess invasibility.

## Task 4.3: Network invasibility cube [Lead: SUN]

Develop a workflow to visualise invasion fitness, using species occurrence data, IAS lists, and species traits to calculate trait centrality, visualize trait dispersion, estimate interaction strength, and assess invasibility. Outputs include interaction strength matrices and community dynamics details under biological invasions.

- Dec 2024: M15 – Code design: Visualise trait dispersion to assess invasibility
- May 2025: M16 – Code test: Visualise trait dispersion to assess invasibility

## Introduction..
Develop a workflow to visualise invasion fitness, using species occurrence data, Alien Invasive Species (AIS) lists, and species traits to calculate trait centrality, visualise trait dispersion, estimate interaction strength, and assess invasibility. Outputs include interaction strength matrices and community dynamics details under biological invasions.

## Objectives  
This workflow supports the work package deliverables for "Visualising Invasion Fitness and Invasibility". The workflow is designed to compute and visualise the invasion fitness and invasibility of ecological interaction networks. By leveraging species occurrence data, lists of invasive alien species (IAS), and species traits, the workflow enables the calculation of community invasibility and the identification of maximum-invasiveness traits (MITs). The workflow delivers outputs such as interaction strength matrices, community trait profiles, and regional maps of invasibility, thus providing crucial insights into ecological network stability and community openness. Below is an expanded description of the workflow used to achieve these objectives:


## Methods  

### Data Access and Preparation  
This section focuses on automating the retrieval and pre-processing of core data, including species occurrence and environmental variables. These data form the basis for further ecological analysis and model building. 
Objective: Automate access and preparation of species, trait and environment data to support downstream invasibility assessments.

#### Species Occurrence Records  
Data will be obtained from i) local sources; ii) Global Biodiversity Information Facility (GBIF); iii) [species occurrence cubes from B3](https://docs.b-cubed.eu/occurrence-cube/specification/).

Automate access and preprocessing of species occurrence data from sources such as local databases, the Global Biodiversity Information Facility (GBIF), and species occurrence cubes. This involves assembling data on species distributions across specified taxonomic groups and regions, resulting in matrices that quantify species co-occurrence within locations.

`get_species`: Fetches and formats species occurrence data from various sources (e.g., local databases, GBIF), creating presence-absence or abundance matrices.  

---

**Table x: Expected structure of species occurrence records in short format data frame.**

| site_id | x (longitude) | y (latitude) | sp_name (species) | pa (presence/absence) | abund (abundance) |
|---------|---------------|--------------|-------------------|-----------------------|-------------------|
| 1.0     | 1.0           | 1.0          | Species A         | 1                     | 5                 |
| 2.0     | 1.0           | 1.0          | Species B         | 0                     | 0                 |
| 3.0     | 1.0           | 1.0          | Species C         | 1                     | 2                 |

---

**Table x: Expected structure of species occurrence records in long format data frame.**

| site_id | x (longitude) | y (latitude) | sp_1 (pa/abund) | sp_2 (pa/abund) | sp_3 (pa/abund) | sp_4 (pa/abund) |
|---------|---------------|--------------|-----------------|-----------------|-----------------|-----------------|
| Row 1   | 1.0           | 1.0          | 1.0             | 1.0             | 1.0             | 1.0             |
| Row 2   | 1.0           | 1.0          | 1.0             | 1.0             | 1.0             | 1.0             |
| Row 3   | 1.0           | 1.0          | 1.0             | 1.0             | 1.0             | 1.0             |

#### Species Traits  
Here the [TRY](https://www.try-db.org/TryWeb/Home.php) database is used to link plant traits back to species records, enabling robust trait-based analyses of biodiversity. Managed by Future Earth and the Max Planck Institute, TRY is a global repository of standardized plant trait data. Version 6 includes over one million records for 2661 traits across 305,000 taxa. It integrates data from 700 datasets, providing global coverage. Open access (CC BY license) ensures transparency, facilitating research on biodiversity, ecosystem functions, and supporting dynamic global models of ecosystem responses to environmental change.

`dataGEN`: Leverages the TRY database to link plant traits back to species records.


#### Environmental Data  
Environmental data are sourced from i) local sources, ii) [WorldClim](https://ouicodedata.com/posts/worldclim-with-r/) using `geodata`, iii) [CHELSA](https://onlinelibrary.wiley.com/doi/full/10.1111/jvs.13215?msockid=110b07b8df4767290225134fde4766f6) using [`climenv`](https://chelsa-climate.org/), and iv) additional [biodiversity data](https://mapme-initiative.github.io/mapme.biodiversity/index.html) using `mapme.biodiversity`.
Automate access and preprocessing of environmental data using diverse sources (e.g., local records, WorldClim, CHELSA, GEE) to compile georeferenced environmental layers (e.g., climate, soil, topography) that are essential for understanding ecological drivers of species distributions.

`get_enviro`: Retrieves georeferenced environmental data (e.g., climate, soil) and clips it to the study area extent, making it compatible with species data.


#### Data Formatting  
Organizes the prepared data into structured data frames for easy access during analysis.

`format_df`: Organizes the prepared data into structured data frames for easy access during analysis:

- **site_xy**: Holds spatial coordinates of sampled sites.
- **site_sp**: Site-by-species matrix for biodiversity assessments.
- **site_env**: Site-specific environmental data provides contextual information about the conditions at each study location.
- **sp_trait**: Species-specific trait data essential for understanding how species traits influence interactions.

---

**Table x: Output structure of `site_xy`, which holds spatial coordinates of sampled sites.**

| site_id | x (longitude) | y (latitude) |
|---------|---------------|--------------|
| Site_1  | 31.50         | -24.20       |
| Site_2  | 31.51         | -24.21       |
| Site_3  | 31.52         | -24.22       |

---

**Table x: Output structure of `site_sp`, the site-by-species matrix used for biodiversity assessments.**

| site_id | sp_1 (pa/abund) | sp_2 (pa/abund) | sp_3 (pa/abund) | sp_4 (pa/abund) | sp_... (pa/abund) |
|---------|-----------------|-----------------|-----------------|-----------------|-------------------|
| Site_1  | 1.0             | 1.0             | 1.0             | 1.0             | 1.0               |
| Site_2  | 1.0             | 1.0             | 1.0             | 1.0             | 1.0               |
| Site_3  | 1.0             | 1.0             | 1.0             | 1.0             | 1.0               |

---

**Table x: Output structure of `site_env`, the site-by-environment matrix linking species and environmental data.**

| site_id | enviro_1 (variable) | enviro_2 (variable) | enviro_3 (variable) | enviro_4 (variable) |
|---------|---------------------|---------------------|---------------------|---------------------|
| Site_1  | 1.0                 | 1.0                 | 1.0                 | 1.0                 |
| Site_2  | 1.0                 | 1.0                 | 1.0                 | 1.0                 |
| Site_3  | 1.0                 | 1.0                 | 1.0                 | 1.0                 |

---

**Table x: Output structure of `sp_trait`, the species-by-traits matrix used for understanding how species traits influence interactions.**

| site_id | trait_1 (sp trait) | trait_2 (sp trait) | trait_3 (sp trait) | trait_4 (sp trait) |
|---------|--------------------|--------------------|--------------------|--------------------|
| Site_1  | 1.0                | 1.0                | 1.0                | 1.0                |
| Site_2  | 1.0                | 1.0                | 1.0                | 1.0                |
| Site_3  | 1.0                | 1.0                | 10.0               | 1.0                |

### Community Trait Profiles  
In progress...

### Functional Dissimilarity  
In progress...

### Interaction Strength  
In progress...

### Invasibility Assessment and Prediction  
In progress...

## Results  
In progress...

## Discussion 
In progress

## Conclusion 
In progress

## Data Availability Statement  
In progress

## Acknowledgements
- [National Institute for Theoretical and Computational Sciences (NITheCS)](https://nithecs.ac.za/)

## References
In progress...

## License

This project is licensed under the [MIT License](LICENSE.md) MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

- [National Institute for Theoretical and Computational Sciences (NITheCS)](https://nithecs.ac.za/)
