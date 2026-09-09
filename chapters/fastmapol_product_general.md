# Data Products and Format {#sec-data-format}

## Overview

FastMAPOL (Fast Multi-Angle Polarimetric Ocean and Land algorithm) produces Level-2 (L2) geophysical retrievals of aerosol and surface properties from the multi-angle polarimetric measurements of the **PACE polarimeters**, including **HARP2** and **SPEXone**.

The Level-2 product provides pixel-level estimates of aerosol optical properties, aerosol microphysical parameters, and ocean color quantities derived through atmospheric correction. In addition to the primary geophysical retrievals, the product includes diagnostic quantities describing retrieval performance and quality indicators for downstream processing.

The Level-2 product is distributed in **NetCDF format** and organized into hierarchical data groups to separate geolocation, geophysical variables, diagnostics, and sensor parameters.

The FastMAPOL Level-2 product is designed to support:

- aerosol science applications
- ocean color studies
- land surface studies
- atmospheric correction
- algorithm validation
- Level-3 gridded product generation

## File Organization

The FastMAPOL Level-2 product is organized into several top-level groups. Each group contains variables associated with a specific component of the retrieval system.

| Group | Description |
|---|---|
| `geolocation_data` | Pixel geolocation information |
| `geophysical_data` | Primary aerosol, ocean, and land retrieval products |
| `diagnostic_data` | Retrieval diagnostics and performance metrics |
| `sensor_band_parameters` | Instrument spectral and angular parameters |
| `processing_control` | Processing metadata and algorithm configuration |

### Dimensions

The FastMAPOL Level-2 product uses a set of core dimensions describing the spatial, spectral, and angular sampling of the observations and retrieval products.

| Dimension | Description |
|---|---|
| `number_of_lines` | Number of along-track scan lines |
| `pixels_per_line` | Number of cross-track pixels |
| `wavelength` | Spectral wavelengths used in the retrieval products |
| `number_of_views` | Number of viewing angles |
| `intensity_bands_per_view` | Number of intensity bands per viewing angle |
| `polarization_bands_per_view` | Number of polarization bands per viewing angle |


### Instrument Differences

**HARP2**

- ~90 viewing angles  
- 4 spectral bands : 440, 550, 670, 870nm
- wide swath (~2000km)

**SPEXone**

- 5 viewing angles  
- hyperspectral polarimetric measurements 50
- narrow swath (~100km)

A total of 34 specific wavelengths from the hyperspectral measurements for aerosol and surface retrieval.

## Geolocation Data

Group: `/geolocation_data`

| Variable | Dimensions | Description |
|---|---|---|
| `latitude` | (`number_of_lines`, `pixels_per_line`) | Pixel latitude |
| `longitude` | (`number_of_lines`, `pixels_per_line`) | Pixel longitude |

These variables define the geographic location of each retrieval pixel.

## Geophysical Retrieval Products

Group: `/geophysical_data`

The geophysical group contains aerosol optical properties, aerosol microphysical parameters, and ocean color products retrieved by FastMAPOL. Two data suites, MAPOL_OCEAN and MAPOL_LAND, are available for aerosol over ocean and land, respectively.  

### Aerosol Optical Properties 

**Product suites:** `MAPOL_OCEAN` and `MAPOL_LAND`

| Variable | Dimensions | Description |
|---|---|---|
| `aot` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Aerosol optical depth |
| `aot_fine` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Fine-mode aerosol optical depth |
| `aot_coarse` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Coarse-mode aerosol optical depth |
| `fmf` | (`number_of_lines`, `pixels_per_line`) | Fine-mode aerosol optical depth fraction |
| `angstrom_440_870` | (`number_of_lines`, `pixels_per_line`) | Ångström exponent (440–870 nm) |
| `angstrom_440_670` | (`number_of_lines`, `pixels_per_line`) | Ångström exponent (440–670 nm) |
| `ssa` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Aerosol single-scattering albedo |
| `ssa_fine` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Fine-mode single-scattering albedo |
| `ssa_coarse` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Coarse-mode single-scattering albedo |
| `aerosol_lidar_ratio` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Aerosol lidar ratio |
| `aerosol_depol_ratio` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Aerosol depolarization ratio |

These parameters (`aot`,`angstrom`,`fmf`) describe aerosol optical loading and spectral behavior. Note that Ångström exponent (440-870 nm) is only available for HARP2 data since SPEXone do not have 870nm band. Single scattering albedo (`ssa`) describe aerosol absorption property.

Two new variables are added to V4 data including `aerosol_lidar_ratio` and `aerosol_depol_ratio`, for the comparison with lidar products. 

### Aerosol Microphysical Properties

**Product suites:** `MAPOL_OCEAN` and `MAPOL_LAND`

| Variable | Dimensions | Description |
|---|---|---|
| `reff_fine` | (`number_of_lines`, `pixels_per_line`) | Fine-mode effective radius |
| `reff_coarse` | (`number_of_lines`, `pixels_per_line`) | Coarse-mode effective radius |
| `veff_fine` | (`number_of_lines`, `pixels_per_line`) | Fine-mode effective variance |
| `veff_coarse` | (`number_of_lines`, `pixels_per_line`) | Coarse-mode effective variance |
| `mr_fine` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Fine-mode real refractive index |
| `mr_coarse` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Coarse-mode real refractive index |
| `mr` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Mode-averaged real refractive index |
| `mi_fine` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Fine-mode imaginary refractive index |
| `mi_coarse` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Coarse-mode imaginary refractive index |
| `mi` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Mode-averaged imaginary refractive index |
| `sph_fine` | (`number_of_lines`, `pixels_per_line`) | Fine-mode spherical particle fraction |
| `sph_coarse` | (`number_of_lines`, `pixels_per_line`) | Coarse-mode spherical particle fraction |
| `sph` | (`number_of_lines`, `pixels_per_line`) | Volume-averaged spherical particle fraction |
| `vd_fine` | (`number_of_lines`, `pixels_per_line`) | Fine-mode aerosol volume density |
| `vd_coarse` | (`number_of_lines`, `pixels_per_line`) | Coarse-mode aerosol volume density |
| `fvf` | (`number_of_lines`, `pixels_per_line`) | Fine-mode volume fraction |
| `alh` | (`number_of_lines`, `pixels_per_line`) | Aerosol layer height |

These variables constrain aerosol particle size distribution, composition, and shape.

### Ocean Products

**Product suites:** `MAPOL_OCEAN`

| Variable | Dimensions | Description |
|---|---|---|
| `Rrs_angular` | (`number_of_lines`, `pixels_per_line`, `number_of_views`, `intensity_bands_per_view`) | Angular remote sensing reflectance before bidirectional reflectance correction |
| `Rrs_nadir` | (`number_of_lines`, `pixels_per_line`, `number_of_views`, `intensity_bands_per_view`) | Nadir-adjusted remote sensing reflectance after bidirectional reflectance correction |
| `Rrs_angular_mean` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Mean of `Rrs_angular` over viewing angles |
| `Rrs_angular_std` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Standard deviation of `Rrs_angular` over viewing angles |
| `Rrs_nadir_mean` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Mean of `Rrs_nadir` over viewing angles |
| `Rrs_nadir_std` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Standard deviation of `Rrs_nadir` over viewing angles |
| `wind_speed` | (`number_of_lines`, `pixels_per_line`) | Retrieved ocean surface wind speed |
| `chla` | (`number_of_lines`, `pixels_per_line`) | Retrieved chlorophyll-*a* concentration |

The dimensions of the angular reflectance products depend on the PACE polarimeter. Example dimensions are:

- **SPEXone:** `number_of_lines = 395`, `pixels_per_line = 29`, `number_of_views = 5`, and `intensity_bands_per_view = 34`.
- **HARP2:** `number_of_lines = 395`, `pixels_per_line = 519`, `number_of_views = 90`, and `intensity_bands_per_view = 1`.

The remote sensing reflectance represents ocean-leaving reflectance after atmospheric correction. To compare with AERONET OC or other dataset for validation purpose after BRDF correction, variable Rrs_nadir_mean is suggested to use. 

Note that V3 data contains **Rrs1** and **Rrs2**, which are renmaed as **Rrs_angular** and **Rrs_nadir** in current V4 data.

### Land surface product

**Product suites:** `MAPOL_LAND`

| Variable | Dimensions | Description |
|---|---|---|
| `rhos_angular` | (`number_of_lines`, `pixels_per_line`, `number_of_views`, `intensity_bands_per_view`) | Angular land surface reflectance before bidirectional reflectance correction |
| `rhos_nadir` | (`number_of_lines`, `pixels_per_line`, `number_of_views`, `intensity_bands_per_view`) | Nadir-adjusted land surface reflectance after bidirectional reflectance correction |
| `rhos_angular_mean` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Mean of `rhos_angular` over viewing angles |
| `rhos_angular_std` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Standard deviation of `rhos_angular` over viewing angles |
| `rhos_nadir_mean` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Mean of `rhos_nadir` over viewing angles |
| `rhos_nadir_std` | (`number_of_lines`, `pixels_per_line`, `wavelength`) | Standard deviation of `rhos_nadir` over viewing angles |


The land surface reflectance represents reflectance after atmospheric correction. To compare with validation dataset after BRDF correction, variable rhos_nadir_mean is suggested to use. 

## Diagnostic Data

**Product suites:** `MAPOL_OCEAN` and `MAPOL_LAND`

Group: `/diagnostic_data`

These variables describe retrieval convergence and performance.

| Variable | Dimensions | Description |
|---|---|---|
| `chi2` | (`number_of_lines`, `pixels_per_line`) | Retrieval cost function |
| `chi2_first_guess` | (`number_of_lines`, `pixels_per_line`) | Cost function at the initial state |
| `nv_ref` | (`number_of_lines`, `pixels_per_line`) | Number of reflectance measurements used in the retrieval |
| `nv_dolp` | (`number_of_lines`, `pixels_per_line`) | Number of DoLP measurements used in the retrieval |
| `mask_ref` | (`number_of_lines`, `pixels_per_line`, `number_of_views`, `intensity_bands_per_view`) | Reflectance measurement screening mask |
| `mask_dolp` | (`number_of_lines`, `pixels_per_line`, `number_of_views`, `polarization_bands_per_view`) | DoLP measurement screening mask |
| `quality_flag` | (`number_of_lines`, `pixels_per_line`) | Retrieval quality indicator |
| `nfev` | (`number_of_lines`, `pixels_per_line`) | Number of forward-model evaluations |
| `njev` | (`number_of_lines`, `pixels_per_line`) | Number of Jacobian evaluations |
| `timing` | (`number_of_lines`, `pixels_per_line`) | Retrieval runtime |
| `ozone` | (`number_of_lines`, `pixels_per_line`) | Total column ozone |
| `surface_pressure` | (`number_of_lines`, `pixels_per_line`) | Surface pressure |

Further details on `chi2`, `nv_ref`, `nv_dolp`, `mask_ref`, `mask_dolp`, and `quality_flag` are provided below.

The remaining variables are primarily included for diagnostic and debugging purposes:

* `nfev`, `njev`, and `timing` are used to track the inversion process and its computational performance.

* `ozone` and `surface_pressure` are typically obtained from ancillary data but may be updated within the retrieval algorithm to provide values at higher spatial resolution.

### Retrieval Quality Metrics Based on the Cost Function

FastMAPOL retrievals are performed using an optimal estimation framework that minimizes a cost function `chi2`. The variable `chi2` represents the final value of the cost function after the inversion has converged. `chi2_first_guess` represents the $\chi^2$ value obtained from the first-guess retrieval using the smaller neural network in the initial retrieval stage (see @sec-nn-model).

Usually lower values of $\chi^2$ indicate better agreement between the measurements and forward model simulations.

### Retrieval Quality Metrics Based on Fitted Measurements

FastMAPOL employs adaptive data screening to remove measurements at specific viewing angles, spectral bands, or their combinations that cannot be adequately fitted by the forward model (see @sec-data-screening). Such measurements are often associated with clouds or other anomalies.

The total numbers of reflectance and DoLP measurements retained in the retrieval are denoted by `nv_ref` and `nv_dolp`, respectively. These variables also provide useful indicators of the retrieval quality for each pixel. If a large fraction of the measurements is removed, less information is available to constrain the retrieved parameters. Therefore, larger values of `nv_ref` and `nv_dolp` generally indicate that more measurement information is available for the retrieval.

The measurement masks for individual viewing angles and spectral bands are also provided in the diagnostic data group as `mask_ref` and `mask_dolp`. These masks allow users to identify which measurements are retained or excluded at each viewing angle and wavelength. The total numbers of valid measurements indicated by `mask_ref` and `mask_dolp` are consistent with the corresponding values of `nv_ref` and `nv_dolp`, respectively.

### Retrieval Quality Flag

A `quality_flag` is provided to facilitate data filtering and Level-3 processing. The flag is determined from a combination of `chi2`, `nv_ref`, and `nv_dolp`. In general, a value of `0` indicates a smaller `chi2`, corresponding to better agreement between the measurements and forward model simulations, together with larger `nv_ref` and `nv_dolp`, indicating that a larger fraction of the measurements is retained in the retrieval.

Larger `quality_flag` values generally indicate a larger `chi2` and/or smaller numbers of retained measurements, and therefore reduced retrieval quality. This may result from reduced information content or potential contamination by effects such as clouds, land, or other adjacency effects.

| `quality_flag` | Description |
|---|---|
| `0` | Highest retrieval quality |
| `1` | Good retrieval quality |
| `>1` | Reduced retrieval quality |


## Sensor Band Parameters

**Product suites:** `MAPOL_OCEAN` and `MAPOL_LAND`

**Group:** `/sensor_band_parameters`

| Variable | Dimensions | Description |
|---|---|---|
| `wavelength` | (`wavelength`) | Retrieval wavelengths |
| `sensor_view_angle` | (`number_of_views`) | Sensor viewing angles |
| `intensity_wavelength` | (`number_of_views`, `intensity_bands_per_view`) | Intensity field center wavelengths at each view |