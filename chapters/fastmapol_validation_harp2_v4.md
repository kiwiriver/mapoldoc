# HARP2 L2 Validation (V4)

This page summarizes validation results for aerosol and surface products retrieved from the PACE multi-angle polarimeters using the **FastMAPOL** algorithm, with emphasis on **HARP2** aerosol optical properties, ocean remote-sensing reflectance ($R_{rs}$) and land surface reflectance ($\rho_s$).

FastMAPOL performs coupled aerosol–surface retrievals using an optimal-estimation framework accelerated by deep neural network forward models. The validation efforts on HARP2 is ongoing, this page provide the validation plan following the work by @Gao:2026aa. More results will be updated on the [PACE FastMAPOL validation page](https://pace.oceansciences.org/pace_fastmapol.htm).

## Summary Information

- **FastMAPOL version:** v4.0  
- **Data period:** 2024/03–2025/12  
- **Reference:** AERONET v3, Level 1.5   

## Retrieval Products to be evaluated (ongoing)

**Aerosol**
- Total, fine-mode, and coarse-mode aerosol optical depth (AOD)
- Total, fine-mode, and coarse-mode single-scattering albedo (SSA)

**Ocean**
- Ocean remote-sensing reflectance ($R_{rs}$)

**LAND**
- Land surface reflectance ($\rho_s$)
  
## Validation Strategy

Validation is organized into three levels similar to SPEXone (@Gao:2026aa):

1. **Global Level-3 assessment**  
   Evaluate large-scale spatial and seasonal patterns as shown below.

2. **Global aerosol validation**  
   Compare retrievals with AERONET observations

3. **Joint aerosol–ocean validation**  
   Evaluate aerosol and $R_{rs}$ simultaneously using AERONET-OC matchups, study the mutual dependencies on the retrieval uncertainties. 

The objective is to assess both the physical realism of the retrieved fields and the consistency of aerosol and ocean products.


## Global Level-3 Products

**Aerosol Products (556 nm)**

Global distributions of total, fine-mode, and coarse-mode AOD and SSA at 556 nm:

![Global aerosol products](../figure/fig_harp2_l3_aod.png)

**LAND Products**

Global distributions of $\rho_{s}$ in RGB (667, 556, 443 nm):

![Global land products (unpublished).](../figure/fig_harp2_l3_rhos.png)

The retrieved $\rho_s$ contains both spectral and angular dimensions. The results shown here represent the angular mean after BRDF correction (`rhos_nadir_mean`).

**Key result:**  
The global distributions show realistic spatial and seasonal patterns, indicating effective separation of atmospheric and surface retrieval components. Over ocean, AOD shows good consistency with AERONET when using the best-quality retrievals (quality flags 0 and 1). Over land, however, retrieved AOD is generally higher than AERONET observations. For more details, please refer to the Version 4 release notes.