# HARP2 L2 aerosol product validation (Pre-Launch DITL test)

## Overview

Pre-launch Day-in-the-Life (DITL) simulations provide an end-to-end benchmark of HARP2 measurement realism, L2 retrieval performance, and uncertainty characterization. The results below highlight three key aspects: synthetic observations, pixel-wise retrieval performance, and uncertainty analysis. Details are provided in @Gao:2023aa.

## Summary Information

- **FastMAPOL version:** Prelaunch Test  
- **Data period:** March 21, 2022 (spring equinox)  
- **Reference:** Synthetic HARP2  
- **Published:** Dec 07, 2023 (https://doi.org/10.5194/amt-16-5863-2023)
  
## Synthetic HARP2 observations
As shown in @fig-validation_harp2_l1c, A full day of synthetic HARP2 observations is generated using a neural network forward model for both reflectance and DoLP. Aerosol and surface properties are prescribed from reanalysis (MERRA-2). The simulated observations provide a realistic testbed for retrieval evaluation:

- Global multi-angle measurements are generated for both reflectance and polarization.  
- DoLP shows enhanced sensitivity to aerosol and surface properties.  
- Reflectance and DoLP provide complementary information content.  

![Global HARP2 simulation (550 nm): reflectance (a–c) and DoLP (d–f) from 15 orbits on 21 March 2022, showing three along-track viewing angles. Figure is adopted from @Gao:2023aa](../figure/validation_harp2_l1c.png){#fig-validation_harp2_l1c width=80%}

## Geophysical retrievals with pixel-wise uncertainty
Based on the synthetic data, retrieval results as shown in @fig-validation_harp2_l2 demonstrate strong agreement with truth and meaningful uncertainty estimates:

- AOD (550 nm) is retrieved with high accuracy and low spatial error.  
- Multiple geophysical variables are retrieved simultaneously, with ALH shown as an example.  
- Pixel-wise uncertainty reflects scene dependence, with larger values at low AOD and limited angular sampling.  

![Retrieval performance: AOD (550 nm) and ALH showing retrieval, truth, and uncertainty (a–c, d–f). Figure is adopted from @Gao:2023aa](../figure/validation_harp2_l2.png){#fig-validation_harp2_l2 width=80%}

## Uncertainty closure analysis
Theoretical uncertainties are derived from error propagation, while realized uncertainties are computed from retrieval–truth differences. Their comparisons are shown in @fig-validation_harp2_unc, 

- Good agreement for aerosol optical and microphysical properties, especially at moderate to high AOD.  
- Decreasing uncertainty with increasing aerosol loading.  
- Underestimation of uncertainty in low-AOD conditions and for weakly constrained parameters.  

![Theoretical (red) vs. retrieved (blue) uncertainties as a function of AOD for AOD, SSA, mr, reff, veff, wind speed, and Chl a. AOD ranges from 0.01–0.45 (Δ=0.01). Figure is adopted from @Gao:2023aa](../figure/validation_harp2_unc.png){#fig-validation_harp2_unc width=90%}

## Summary

- Synthetic data provide realistic multi-angle polarimetric observations.  
- L2 retrievals achieve high accuracy for key variables (especially AOD).  
- Pixel-level uncertainties are physically meaningful and scene dependent.  
- Uncertainty estimates are generally consistent with actual errors, with known limitations.  

These results define a pre-launch benchmark for HARP2 L2 validation using DITL data.