# About This Document {.unnumbered}

This Algorithm Theoretical Basis Document (ATBD) describes the theoretical basis, implementation, data products, and validation of the Fast Multi-Angle Polarimetric Ocean and Land (FastMAPOL) retrieval algorithm developed for the NASA Plankton, Aerosol, Cloud, ocean Ecosystem (PACE) mission.  

FastMAPOL retrieves aerosol, ocean, and land surface properties from multi-angle polarimetric observations acquired by the PACE polarimeters, HARP2 and SPEXone. The algorithm is currently run in production and produce operational aerosol, ocean color and land surface products with public available data through NASA Earth Data Cloud. 

The following chapters provide information on data access through NASA Earthdata and product release notes. These are followed by **FastMAPOL at a Glance**, which provides a high-level overview of the algorithm. The algorithm overview, data product format, and validation results are then summarized in the subsequent chapters. More detailed technical descriptions of the algorithm and its underlying methodology are provided in the second part of this document.

A web version of this document is available at [{{< meta atbd-website.name >}}]({{< meta atbd-website.url >}}) and may provide more timely updates. Both the web content and the link location are subject to change. Please contact the authors for the latest information.

## Document Information {.unnumbered .unlisted}

| | |
|---|---|
| **Mission** | PACE |
| **Point of Contact** | Meng Gao ([meng.gao@nasa.gov](mailto:meng.gao@nasa.gov)) |
| **Keywords** | {{< meta keywords-display >}} |
| **Website** | [{{< meta atbd-website.url >}}]({{< meta atbd-website.url >}}) |
| **Version** | {{< meta version >}} |

## How to Cite {.unnumbered .unlisted}

When using or referring to FastMAPOL ATBD, please cite this document as:

> Gao, M., P. Zhai, K. Aryal, K. Knobelspiesse, and B. Franz. *FastMAPOL Algorithm Theoretical Basis Document: Coupled Aerosol, Ocean, and Land Retrievals from PACE Multi-Angle Polarimetry*. NASA Goddard Space Flight Center, Version {{< meta version >}}, 2026. <{{< meta atbd-website.url >}}>

Please also cite the relevant peer-reviewed publications when using the algorithm or data products.

# Earthdata Access {.unnumbered} 

FastMAPOL Level-2 (L2) and Level-3 mapped (L3M) products are distributed through NASA Earthdata. The most recent production version is listed first below.

## Version 4.0

**SPEXone MAPOL_OCEAN**

- **L2:** [https://doi.org/10.5067/PACE/SPEXONE/L2/MAPOL_OCEAN/4.0](https://doi.org/10.5067/PACE/SPEXONE/L2/MAPOL_OCEAN/4.0)
- **L3M:** [https://doi.org/10.5067/PACE/SPEXONE/L3M/MAPOL_OCEAN/4.0](https://doi.org/10.5067/PACE/SPEXONE/L3M/MAPOL_OCEAN/4.0)

**HARP2 MAPOL_OCEAN**

- **L2:** [https://doi.org/10.5067/PACE/HARP2/L2/MAPOL_OCEAN/4.0](https://doi.org/10.5067/PACE/HARP2/L2/MAPOL_OCEAN/4.0)
- **L3M:** [https://doi.org/10.5067/PACE/HARP2/L3M/MAPOL_OCEAN/4.0](https://doi.org/10.5067/PACE/HARP2/L3M/MAPOL_OCEAN/4.0)

**HARP2 MAPOL_LAND**

- **L2:** [https://doi.org/10.5067/PACE/HARP2/L2/MAPOL_LAND/4.0](https://doi.org/10.5067/PACE/HARP2/L2/MAPOL_LAND/4.0)
- **L3M:** [https://doi.org/10.5067/PACE/HARP2/L3M/MAPOL_LAND/4.0](https://doi.org/10.5067/PACE/HARP2/L3M/MAPOL_LAND/4.0)

## Version 3.0

Version 3.0 products remain available for users who need access to the earlier FastMAPOL production release.

**SPEXone MAPOL_OCEAN**

- **L2:** [https://doi.org/10.5067/PACE/SPEXONE/L2/MAPOL_OCEAN/3.0](https://doi.org/10.5067/PACE/SPEXONE/L2/MAPOL_OCEAN/3.0)
- **L2:** [https://doi.org/10.5067/PACE/SPEXONE/L3M/MAPOL_OCEAN/3.0](https://doi.org/10.5067/PACE/SPEXONE/L3M/MAPOL_OCEAN/3.0)

**HARP2 MAPOL_OCEAN**

- **L2:** [https://doi.org/10.5067/PACE/HARP2/L2/MAPOL_OCEAN/3.0](https://doi.org/10.5067/PACE/HARP2/L2/MAPOL_OCEAN/3.0)
- **L3M:** [https://doi.org/10.5067/PACE/HARP2/L3M/MAPOL_OCEAN/3.0](https://doi.org/10.5067/PACE/HARP2/L3M/MAPOL_OCEAN/3.0)

# Release Notes {.unnumbered}

The FastMAPOL SPEXone release notes are available under [Documents and Resources on the Earthdata SPEXone product pages](https://www.earthdata.nasa.gov/data/catalog/ob-cloud-pace-spexone-l2-mapol-ocean-4.0#documents-and-resources). The FastMAPOL HARP2 release notes are similarly available under [Documents and Resources on the Earthdata HARP2 product pages](https://www.earthdata.nasa.gov/data/catalog/ob-cloud-pace-harp2-l2-mapol-ocean-4.0#documents-and-resources).

The PACE release notes available through the [PACE Data Access page](https://pace.oceansciences.org/access_pace_data.htm) may not always contain the most up-to-date product-specific information.

Because these links are subject to change, the relevant release information is also provided below for reference.

## Version 4.0

### HARP2 Level-2 Products {.unnumbered .unlisted}

- `MAPOL_LAND` (Aerosol and Land Properties, FastMAPOL Algorithm)

- `MAPOL_OCEAN` (Aerosol and Ocean Properties, FastMAPOL Algorithm)

**Changes from Version 3**

- Uses Version 4 L1C data, with the same FastMAPOL retrieval algorithm previously applied to Version 3 data.

- Aerosol retrieval quality is improved for the `MAPOL_OCEAN` data suites. Quality flags 0 and 1 are recommended for scientific applications.

- Quality flag definitions have been updated. Please refer to the new HelpHub notebooks for details:

  [https://nasa.github.io/oceandata-notebooks/sections/cloud-atmosphere.html](https://nasa.github.io/oceandata-notebooks/sections/cloud-atmosphere.html)

- Ocean surface reflectance products `Rrs1` and `Rrs2` have been renamed `Rrs_angular` and `Rrs_nadir`, respectively. These represent angular Rrs before and after BRDF correction.

- New `MAPOL_LAND` data suites have been added. Multi-angle land surface reflectances are reported as rhos_angular and rhos_nadir.

**Known issues**

- A substantial fraction of aerosol retrievals is associated with lower-quality retrieval conditions, including larger $\chi^2$ values and fewer valid viewing angles (`nv_ref` and `nv_dolp`). These retrievals can exhibit larger biases in aerosol optical properties such as AOD. Users are encouraged to apply the recommended quality flags.

- `MAPOL_LAND` is undergoing active testing and improvement. The current version shows significant biases in both fine- and coarse-mode aerosol properties and should therefore be used with caution.


### HARP2 Level-3 Products {.unnumbered .unlisted}

HARP2 Level-2 products are composited into global Level-3 binned and mapped products. Daily, 8-day, and monthly Level-3 mapped products are produced on an equirectangular grid at 0.1-deg and 1-deg resolution. The available list of mapped product types now include:

- `MAPOL_OCEAN`: aerosol and ocean properties from FastMAPOL algorithm
- `MAPOL_LAND`: aerosol and land properties from FastMAPOL algorithm

### SPEXone Level-2 Products {.unnumbered .unlisted}

**Changes from Version 3**

- Uses Version 4 L1C data, with the same FastMAPOL retrieval algorithm previously applied to Version 3 data.

- Retrieval performance is similar to the reported validation results against AERONET and AERONET-OC observations (Gao, M., K. Aryal, P. Zhai, K. Knobelspiesse, B. Franz, B. Cairns, I. Cetinić, G. Fu, O. Hasekamp, A. Ibrahim, A. M. Sayer, and P. J. Werdell. 2026. “Where Sky Meets Sea: Integrated Aerosol and Ocean Color Retrieval from PACE SPEXone Multi-Angle Polarimetry.” *Remote Sensing of Environment*, 345, 115603. [https://doi.org/10.1016/j.rse.2026.115603](https://doi.org/10.1016/j.rse.2026.115603)).

- `Rrs1` and `Rrs2` have been renamed `Rrs_angular` and `Rrs_nadir`, respectively, representing angular Rrs before and after BRDF correction.

**Known issues**

- Ocean Rrs bias is reduced, but still with a positive bias of approximately 0.001 sr⁻¹ in the blue bands.

- Over coastal waters, Rrs and AOD exhibit stronger coupling and higher retrieval uncertainties, as discussed in the above paper. Improvements are planned to better account for complex water optical properties (Aryal, K., M. Gao, P. Zhai, B. A. Franz, K. Knobelspiesse, A. Ibrahim, and J. Werdell. 2026. “Aerosol and Ocean Color Retrievals from the FastMAPOL/Component Algorithm Using NASA's PACE Polarimetric Measurements.” *Optics Express*, 34(16), 29139. [https://doi.org/10.1364/OE.607432](https://doi.org/10.1364/OE.607432)).


### SPEXone Level-3 Products {.unnumbered .unlisted}

SPEXone Level-2 products are composited into global Level-3 binned and mapped products. Daily, 8-day, and monthly Level-3 mapped products are produced on an equirectanular grid at 0.1-deg and 1-deg resolution. The available list of mapped product types now include:

- `MAPOL_OCEAN`: aerosol and ocean properties from FastMAPOL algorithm

## Version 3.0

### SPEXone Level-2 Products {.unnumbered .unlisted}

- `MAPOL_OCEAN` (Aerosol and Ocean Properties from FastMAPOL Algorithm)

This is the first release of Level-2 and Level-3 products from the FastMAPOL algorithm as applied to SPEXone Version 3 Level-1C. The FastMAPOL algorithm generates a large suite of aerosol and ocean properties. A subset is listed here.

**Provisional Products**

- Aerosol Optical Thickness (`aot`)
- Single Scattering Albedo (`ssa`)
- Effective Radius of Fine Mode Aerosols (`reff_fine`)
- Effective Radius of Coarse Mode Aerosols (`reff_fine`)
- Fine Mode Fraction (`fmf`)
- Fine Mode Fraction by Volume (`fvf`)
- Real Index of Refraction (`mr`)
- Imaginary Index of Refraction (`mi`)
- Wind speed (`wind_speed`)
- Chlorophyll a concentration (`chla`)
- Angular Mean Water-Leaving Reflectance before BRDF correction (`Rrs1_mean`)
- Standard Deviation of Rrs1 (`Rrs1_std`)
- Angular Mean Water-Leaving Reflectance after BRDF correction (`Rrs2_mean`)
- Standard Deviation of Rrs2 (`Rrs2_std`)

**Known issues**

- Small negative Rrs offset (~-0.001) is often observed at wavelengths > 600 nm.
- BRDF correction does not fully remove Rrs angular dependence (as seen in Rrs2).
- High Rrs bias at 678 nm likely due to influence of the Oxygen A-band.


### SPEXone Level-3 Products {.unnumbered .unlisted}

SPEXone Level-2 products are composited into global Level-3 binned and mapped products. Daily, 8-day, and monthly Level-3 mapped products are produced on an equirectanular grid at 0.1-deg and 1-deg resolution. The available list of mapped product types now include:

- `MAPOL_OCEAN`: aerosol and ocean properties over ocean from FastMAPOL algorithm


### HARP2 Level-2 Products {.unnumbered .unlisted}

- `MAPOL_OCEAN` (Aerosol and Ocean Properties from FastMAPOL Algorithm)

This is the first release of Level-2 and Level-3 products from the FastMAPOL algorithm as applied to HARP2 Version 3 Level-1C. The FastMAPOL algorithm generates a large suite of aerosol and ocean optical properties. A subset is listed here.

**Provisional Products**

- Aerosol Optical Thickness (`aot`)
- Single Scattering Albedo (`ssa`)
- Effective Radius of Fine Mode Aerosols (`reff_fine`)
- Effective Radius of Coarse Mode Aerosols (`reff_fine`)
- Fine Mode Fraction (`fmf`)
- Fine Mode Fraction by Volume (`fvf`)
- Real Index of Refraction (`mr`)
- Imaginary Index of Refraction (`mi`)
- Wind speed (`wind_speed`)
- Chlorophyll a concentration (`chla`)
- Angular Mean Water-Leaving Reflectance before BRDF correction (`Rrs1_mean`)
- Standard Deviation of Rrs1 (`Rrs1_std`)
- Angular Mean Water-Leaving Reflectance after BRDF correction (`Rrs2_mean`)
- Standard Deviation of Rrs2 (`Rrs2_std`)

**Known issues**

- Aerosol optical depth is biased high (~0.07) as compared with AERONET-OC.
- Remote-sensing reflectance (Rrs) is biased low (-0.001) as compared with AERONET-OC.
- High uncertainty in aerosol microphysical retrievals (mr, mi, sph, alh etc).
- Fewer polarization measurements than radiance measurements are successfully fitted, as indicated by valid angle counts (Nv_ref and Nv_dolp) and angular data mask (mask_ref and mask_dolp).


### HARP2 Level-3 Products {.unnumbered .unlisted}

HARP2 Level-2 products are composited into global Level-3 binned and mapped products. Daily, 8-day, and monthly Level-3 mapped products are produced on an equirectanular grid at 0.1-deg and 1-deg resolution. The available list of mapped product types now include:

- `MAPOL_OCEAN`: aerosol and ocean properties over ocean from FastMAPOL algorithm

# Jupyter Notebook Tutorials {.unnumbered} 

FastMAPOL products (`MAPOL_OCEAN` and `MAPOL_LAND`) can be accessed and explored using NASA's `earthaccess` Python package, which provides tools for discovering and accessing data through NASA Earthdata. The following tutorials from PACE Help Hub demonstrate how to search for, download, analyze, and visualize the data products using Jupyter notebooks.

The atmspheric notebook tutorials are available through the [PACE Help Hub Atmosphere Section](https://nasa.github.io/oceandata-notebooks/sections/cloud-atmosphere.html).

- [HARP2 Ocean Aerosols (FastMAPOL)](https://nasa.github.io/oceandata-notebooks/notebooks/harp2/harp2_l2_fastmapol_aerosol_ocean_product.html) - Working with aerosol data over ocean from the HARP2 polarimeter with the FastMAPOL algorithm
- [HARP2 Land Aerosols (FastMAPOL)](https://nasa.github.io/oceandata-notebooks/notebooks/harp2/harp2_l2_fastmapol_aerosol_land_product.html) - Working with aerosol data over land from the HARP2 polarimeter with the FastMAPOL algorithm
- [SPEXone Aerosols (FastMAPOL)](https://nasa.github.io/oceandata-notebooks/notebooks/spexone/spexone_l2_fastmapol_aerosol_product.html) - Analyzing aerosol retrievals over ocean from SPEXone measurements with the FastMAPOL algorithm

The land notebook tutorials are available through the [PACE Help Hub Land Section](https://nasa.github.io/oceandata-notebooks/sections/land.html).

# Data Visualization with SeaDAS {.unnumbered} 
<!--
::: {layout-ncol=2}

::: {.column width="55%"}
-->

FastMAPOL products can also be accessed, explored, and visualized using NASA's Sea, Earth, Atmosphere Data Analysis System ([SeaDAS](https://www.earthdata.nasa.gov/data/tools/seadas)). SeaDAS provides a comprehensive graphical environment for discovering, processing, displaying, analyzing, and performing quality control of satellite-based Earth science data. The software provides several tools for working with HARP2 and SPEXone FastMAPOL L2 and L3 products. It is especially useful to explore the multi-angle ocean and land surface reflectance. 

**Data Discovery and Processing**

- **OB_CLOUD Data Browser:** Search for data in NASA Earthdata Cloud
- **OB_CLOUD Subset Tool:** Perform spatial and/or variable subsetting (L2 data)

**Visualization and Analysis**

- **Band Display:** Display individual geophysical variables and spectral bands
- **Spectrum View & Animation:** Examine and animate the spectral dependence of retrieved quantities
- **Angular View & Animation:** Examine and animate multi-angle observations and retrieved quantities

<!--
:::

::: {.column width="45%"}

![SeaDAS tools for browsing and subsetting L2 data](figure/seadas_cloud_search.png){width=100%}

:::

:::
-->


