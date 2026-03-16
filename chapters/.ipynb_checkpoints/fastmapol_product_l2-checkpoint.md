## Data Products and Format

### Overview

FastMAPOL (Fast Multi-Angle Polarimetric Ocean and Land algorithm) produces Level-2 (L2) geophysical retrievals of aerosol and surface properties from the multi-angle polarimetric measurements of the **PACE polarimeters**, including **HARP2** and **SPEXone**.

The Level-2 product provides pixel-level estimates of aerosol optical properties, aerosol microphysical parameters, and ocean color quantities derived through atmospheric correction. In addition to the primary geophysical retrievals, the product includes diagnostic quantities describing retrieval performance and quality indicators for downstream processing.

The Level-2 product is distributed in **NetCDF format** and organized into hierarchical data groups to separate geolocation, geophysical variables, diagnostics, and sensor parameters.

The FastMAPOL Level-2 product is designed to support:

- aerosol science applications
- ocean color retrievals
- atmospheric correction
- algorithm validation
- Level-3 gridded product generation

---

### File Organization

The FastMAPOL Level-2 file is organized into several top-level groups. Each group contains variables associated with a specific component of the retrieval system.


| Group | Description |
|------|-------------|
| geolocation_data | Pixel geolocation information |
| geophysical_data | Primary aerosol and ocean retrieval products |
| diagnostic_data | Retrieval diagnostics and performance metrics |
| sensor_band_parameters | Instrument spectral and angular parameters |
| processing_control | Metadata and algorithm configuration |

---

#### Dimensions

The FastMAPOL Level-2 product uses a set of core dimensions describing spatial, spectral, and angular sampling.

| Dimension | Description |
|-----------|-------------|
| number_of_lines | Number of along-track scan lines |
| pixels_per_line | Number of cross-track pixels |
| wavelength | Spectral bands used in retrieval |
| number_of_views | Viewing angles used in the retrieval |
| intensity_bands_per_view | Intensity channels per view |
| polarization_bands_per_view | Polarization channels per view |

#### Instrument Differences

**HARP2**

- ~90 viewing angles  
- 4 spectral bands : 440, 550, 670, 870nm
- wide swath (~2000km)

**SPEXone**

- 5 viewing angles  
- hyperspectral polarimetric measurements 50
- narrow swath (~100km)

A total of 34 specific wavelengths from the hyperspectral measurements for aerosol and surface retrieval.

---

### Geolocation Data

Group: `/geolocation_data`

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| latitude | (number_of_lines, pixels_per_line) | Pixel latitude |
| longitude | (number_of_lines, pixels_per_line) | Pixel longitude |

These variables define the geographic location of each retrieval pixel.

---

### Geophysical Retrieval Products

Group: `/geophysical_data`

The geophysical group contains aerosol optical properties, aerosol microphysical parameters, and ocean color products retrieved by FastMAPOL.

---

#### Aerosol Optical Properties

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| aot | (number_of_lines, pixels_per_line, wavelength) | Aerosol optical depth |
| aot_fine | (number_of_lines, pixels_per_line, wavelength) | Fine-mode optical depth |
| aot_coarse | (number_of_lines, pixels_per_line, wavelength) | Coarse-mode optical depth |
| ssa | (number_of_lines, pixels_per_line, wavelength) | Aerosol single scattering albedo |
| ssa_fine | (number_of_lines, pixels_per_line, wavelength) | Fine-mode SSA |
| ssa_coarse | (number_of_lines, pixels_per_line, wavelength) | Coarse-mode SSA |
| angstrom_440_870 | (number_of_lines, pixels_per_line) | Ångström exponent (440-870 nm) |
| angstrom_440_670 | (number_of_lines, pixels_per_line) | Ångström exponent (440-670 nm) |
| fmf | (number_of_lines, pixels_per_line) | Fine-mode optical depth fraction |

These parameters describe aerosol optical loading and spectral behavior.

---

#### Aerosol Microphysical Properties

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| reff_fine | (number_of_lines, pixels_per_line) | Fine-mode effective radius |
| reff_coarse | (number_of_lines, pixels_per_line) | Coarse-mode effective radius |
| veff_fine | (number_of_lines, pixels_per_line) | Fine-mode effective variance |
| veff_coarse | (number_of_lines, pixels_per_line) | Coarse-mode effective variance |
| mr | (number_of_lines, pixels_per_line, wavelength) | Real refractive index |
| mi | (number_of_lines, pixels_per_line, wavelength) | Imaginary refractive index |
| sph | (number_of_lines, pixels_per_line) | Spherical particle fraction |
| vd_fine | (number_of_lines, pixels_per_line) | Fine-mode volume density |
| vd_coarse | (number_of_lines, pixels_per_line) | Coarse-mode volume density |
| fvf | (number_of_lines, pixels_per_line) | Fine-mode volume fraction |
| alh | (number_of_lines, pixels_per_line) | Aerosol layer height |

These variables constrain aerosol particle size distribution, composition, and shape.

---

#### Ocean Color Products

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| Rrs1 | (number_of_lines, pixels_per_line, wavelength, number_of_views) | Remote sensing reflectance before BRDF correction |
| Rrs2 | (number_of_lines, pixels_per_line, wavelength, number_of_views) | Remote sensing reflectance after BRDF correction |
| Rrs1_mean | (number_of_lines, pixels_per_line, wavelength) | Angular mean of Rrs1 |
| Rrs1_std | (number_of_lines, pixels_per_line, wavelength) | Angular standard deviation of Rrs1 |
| Rrs2_mean | (number_of_lines, pixels_per_line, wavelength) | Angular mean of Rrs2 |
| Rrs2_std | (number_of_lines, pixels_per_line, wavelength) | Angular standard deviation of Rrs2 |

The remote sensing reflectance represents ocean-leaving reflectance after atmospheric correction.

Because the full angular arrays can be large, the Level-2 product typically includes the **angular mean and standard deviation**.

---

#### Additional Surface Variables

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| wind_speed | (number_of_lines, pixels_per_line) | Surface wind speed |
| chla | (number_of_lines, pixels_per_line) | Chlorophyll-a concentration |

---

### Diagnostic Data

Group: `/diagnostic_data`

These variables describe retrieval convergence and performance.

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| chi2 | (number_of_lines, pixels_per_line) | Retrieval cost function |
| chi2_first_guess | (number_of_lines, pixels_per_line) | Cost function at first guess |
| nv_ref | (number_of_lines, pixels_per_line) | Number of reflectance measurements used |
| nv_dolp | (number_of_lines, pixels_per_line) | Number of polarization measurements used |
| nfev | (number_of_lines, pixels_per_line) | Number of forward model evaluations |
| njev | (number_of_lines, pixels_per_line) | Number of Jacobian evaluations |
| timing | (number_of_lines, pixels_per_line) | Retrieval runtime |
| quality_flag | (number_of_lines, pixels_per_line) | Retrieval quality indicator |
| ozone | (number_of_lines, pixels_per_line) | Total column ozone |
| surface_pressure | (number_of_lines, pixels_per_line) | Surface pressure |

---

#### Retrieval Quality Metrics

FastMAPOL retrievals are performed using an optimal estimation framework that minimizes the cost function

\[
\chi^2 = \frac{1}{N}\sum \frac{(f-m)^2}{\sigma^2}
\]

where

- \(m\) is the measurement  
- \(f\) is the forward model simulation  
- \(\sigma\) is measurement uncertainty  
- \(N\) is the number of measurements used in the retrieval.

Lower values of \( \chi^2 \) indicate better agreement between measurements and forward model simulations.

---

#### Retrieval Quality Flag

A quality flag is provided to facilitate filtering and Level-3 processing.

| quality_flag | Description |
|--------------|-------------|
| 0 | highest retrieval quality |
| 1 | good retrieval quality |
| >1 | reduced quality |

The quality flag is determined from combinations of:

- cost function value
- number of reflectance measurements
- number of polarization measurements.

---

#### Measurement Screening Masks

FastMAPOL performs adaptive measurement screening during the inversion.

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| mask_ref | (number_of_lines, pixels_per_line, number_of_views, intensity_bands_per_view) | Reflectance measurement mask |
| mask_dolp | (number_of_lines, pixels_per_line, number_of_views, polarization_bands_per_view) | Polarization measurement mask |

Mask interpretation:

| Value | Meaning |
|------|---------|
| 0 | measurement used in retrieval |
| 1 or NaN | measurement excluded from retrieval |

These masks allow users to evaluate measurement quality at each viewing angle.

---

### Sensor Band Parameters

Group: `/sensor_band_parameters`

| Variable | Dimensions | Description |
|----------|-------------|-------------|
| wavelength | (wavelength) | Retrieval wavelengths |
| sensor_view_angle | (number_of_views) | Viewing angle geometry |

---
