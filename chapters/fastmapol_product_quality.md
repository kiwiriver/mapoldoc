# Data Quality Assessment {#sec-data-quality}

As discussed in the previous chapter, FastMAPOL provides several diagnostic variables for evaluating retrieval quality, including the cost function (`chi2`), the numbers of measurements retained in the retrieval (`nv_ref` and `nv_dolp`), measurement-level screening masks (`mask_ref` and `mask_dolp`), and an overall `quality_flag`. These diagnostics characterize different aspects of retrieval performance and allow users to select data according to the requirements of a particular application.

## Quality Metrics Based on the Cost Function

FastMAPOL retrievals are performed using an optimal estimation framework that minimizes the cost function

$$
\chi^2 = \frac{1}{N}\sum \frac{(f-m)^2}{\sigma^2},
$$

where

* $m$ is the measurement,
* $f$ is the forward model simulation,
* $\sigma$ is the measurement uncertainty, and
* $N$ is the number of measurements used in the retrieval.

The variable `chi2` represents the final value of the cost function after the inversion has converged. In general, lower values of `chi2` indicate better agreement between the measurements and forward model simulations. A large `chi2` may indicate inadequate model fitting or measurement contamination associated with clouds, surface adjacency effects, or other anomalies.

## Quality Metrics Based on Retained Measurements

FastMAPOL employs adaptive data screening to remove measurements at specific viewing angles, spectral bands, or their combinations that cannot be adequately fitted by the forward model (see @sec-data-screening). Such measurements are often associated with clouds, land or cloud adjacency effects, or other measurement and modeling anomalies.

The total numbers of reflectance and DoLP measurements retained in the retrieval are denoted by `nv_ref` and `nv_dolp`, respectively. These variables provide additional indicators of retrieval quality and information content. When a large fraction of the available measurements is removed, fewer independent observations remain to constrain the retrieved parameters. Therefore, larger values of `nv_ref` and `nv_dolp` generally indicate that more measurement information is available to constrain the retrieval.

The measurement masks for individual viewing angles and spectral bands are also provided in the diagnostic data group as `mask_ref` and `mask_dolp`. The screening masks are interpreted as follows:

| Value | Meaning |
|---|---|
| `0` | Measurement retained and used in the retrieval |
| `1` or `NaN` | Measurement excluded from the retrieval |

These masks allow users to identify which measurements are retained or excluded at each viewing angle and wavelength. The total numbers of valid measurements indicated by `mask_ref` and `mask_dolp` are consistent with the corresponding values of `nv_ref` and `nv_dolp`, respectively.

## Quality Flag

To facilitate data filtering and Level-3 processing, FastMAPOL provides an overall `quality_flag` derived from a combination of `chi2`, `nv_ref`, and `nv_dolp`. The flag therefore accounts for both the goodness of fit and the amount of measurement information retained in the retrieval.

In general, lower `quality_flag` values correspond to more stringent thresholds on `chi2`, `nv_ref`, and `nv_dolp`, while larger values correspond to progressively relaxed thresholds. The exact definitions depend on the instrument and product version.

| `quality_flag` | General interpretation |
|---|---|
| `0` | Most stringent quality criteria |
| `1` | Less stringent quality criteria |
| `2` | Further relaxed quality criteria |
| `>2` | Progressively relaxed quality criteria |

Because SPEXone and HARP2 have different spectral and angular sampling configurations, instrument-specific thresholds are used to define the quality flags.

### SPEXone

For SPEXone, the quality criteria are based on the following combinations of `chi2`, `nv_ref`, and `nv_dolp`:
  
| `quality_flag` | Criteria |
|---|---|
| `0` | `chi2 <= 1.5`, `nv_ref >= 160`, `nv_dolp >= 160` |
| `1` | `chi2 <= 1.5`, `nv_ref >= 140`, `nv_dolp >= 140`; excluding flag `0` |
| `2` | `chi2 <= 2.0`, `nv_ref >= 140`, `nv_dolp >= 140`; excluding flags `0–1` |
| `3` | `chi2 <= 2.0`, `nv_ref >= 100`, `nv_dolp >= 100`; excluding flags `0–2` |
| `4` | `chi2 <= 3.0`, `nv_ref >= 60`, `nv_dolp >= 60`; excluding flags `0–3` |

The quality flags progressively relaxed criteria associated with larger `chi2` and/or smaller `nv_ref` and `nv_dolp`. 

<!--Only pixels with `chi2<=5`, `nv_ref>=30` and `nv_dolp>=30` will be outputed in the data file. -->

For the current SPEXone FastMAPOL products, there are 34 spectral bands at 5 viewing angles, corresponding to a maximum of 170 reflectance or DoLP measurements. The thresholds of `nv >= 160` and `nv >= 140` therefore correspond to retaining approximately 94% and 82% of the available measurements, respectively.

The SPEXone quality-flag definition remains unchanged between V3 and V4. `quality_flag = 0` applies the most stringent criteria and was used for the primary validation of the SPEXone FastMAPOL product (@Gao:2026aa). However, retrievals with `quality_flag = 1`, or in some applications `quality_flag = 2`, may also be included when greater spatial or temporal coverage is desired. Users may additionally apply their own thresholds directly to `chi2`, `nv_ref`, and `nv_dolp` when a different balance between retrieval quality and coverage is appropriate.

### HARP2

HARP2 has a substantially different angular sampling configuration from SPEXone, and consequently different thresholds are used. The HARP2 quality-flag definition was also updated between V3 and V4 following improvements in Level-1 calibration.

The V4 quality flags are defined as follows:

| `quality_flag` | V4 criteria                                                           |
| -------------- | --------------------------------------------------------------------- |
| `0`            | `chi2 <= 1.5`, `nv_ref >= 70`, `nv_dolp >= 70`                        |
| `1`            | `chi2 <= 1.5`, `nv_ref >= 60`, `nv_dolp >= 60`; excluding flag `0`    |
| `2`            | `chi2 <= 1.5`, `nv_ref >= 40`, `nv_dolp >= 40`; excluding flags `0–1` |
| `3`            | `chi2 <= 2.0`, `nv_ref >= 30`, `nv_dolp >= 30`; excluding flags `0–2` |
| `4`            | `chi2 <= 2.0`, `nv_ref >= 20`, `nv_dolp >= 20`; excluding flags `0–3` |
| `5`            | `chi2 <= 3.0`, `nv_ref >= 10`, `nv_dolp >= 10`; excluding flags `0–4` |

The quality flags progressively relaxed criteria associated with larger `chi2` and/or smaller `nv_ref` and `nv_dolp`. 

<!--Only pixels with `chi2<=5`, `nv_ref>=10` and `nv_dolp>=10` will be outputed in the data file. -->

For HARP2, a total of 90 angular measurements are available for reflectance and polarization. Thus, the V4 thresholds of `nv >= 70` and `nv >= 60` correspond to retaining approximately 78% and 67% of the available measurements, respectively.

In V4, a new, more stringent criterion was introduced as `quality_flag = 0`, and the numerical flag values associated with the existing criteria were shifted relative to V3. The definitions are summarized below:

| Criteria | V3 | V4 |
|---|---:|---:|
| `chi2 < 1.5`, `nv_ref > 70`, and `nv_dolp > 70` | — | `0` |
| `chi2 < 1.5`, `nv_ref > 60`, and `nv_dolp > 60` | `0` | `1` |
| `chi2 < 1.5`, `nv_ref > 40`, and `nv_dolp > 40` | `1` | `2` |
| `chi2 < 2.0`, `nv_ref > 30`, and `nv_dolp > 30` | `2` | `3` |

This table shows how the quality-flag definitions changed between V3 and V4. The same selection criteria used for `quality_flag = 0`, `1`, and `2` in V3 correspond to `quality_flag = 1`, `2`, and `3`, respectively, in V4, while V4 introduces an additional `quality_flag = 0` criterion with `chi2 < 1.5`, `nv_ref > 70`, and `nv_dolp > 70`.

The table describes the definitions of the quality flags and should not be interpreted as a direct comparison of retrieval quality between V3 and V4, because Level-1 calibration and other retrieval inputs also changed between product versions.

Therefore, when combining or comparing HARP2 V3 and V4 data, users should consider the version-specific definitions rather than applying the same numerical `quality_flag` threshold to both datasets.

The numerical definitions of the quality flags may continue to evolve as instrument calibration, retrieval algorithms, and quality assessment procedures improve. Users should therefore refer to the release-specific product documentation when applying quality filtering.

## Balancing Retrieval Quality and Data Coverage

The appropriate quality filtering depends on the application. Restricting an analysis to retrievals that satisfy the most stringent quality criteria can improve confidence in individual pixels but also reduces spatial and temporal coverage. This trade-off can be particularly important near clouds, coastlines, and other heterogeneous scenes, where more measurements may be rejected by the adaptive screening procedure.

For quantitative validation and comparisons with independent datasets, `quality_flag = 0` provides the most stringent selection and is generally preferred when sufficient matchup samples are available. This helps reduce the potential influence of poorly fitted or weakly constrained FastMAPOL retrievals on the comparison.

However, restricting an analysis to `quality_flag = 0` can substantially reduce the number of available matchups and may also change their spatial distribution. Including `quality_flag = 1` can provide a useful balance between retrieval quality and sampling. For applications where spatial coverage is particularly important, retrievals with `quality_flag = 0`, `1`, or `2` may be considered, depending on the required level of accuracy.

A useful general strategy is:

* **`quality_flag = 0`** — most stringent filtering, suitable for validation and quantitative comparisons when sufficient data are available;
* **`quality_flag = 0–1`** — increased sampling and coverage while retaining relatively stringent quality criteria;
* **`quality_flag = 0–2`** — broader coverage for global mapping and applications where increased sampling is important.

These recommendations should not be interpreted as universal thresholds. The appropriate selection depends on the retrieved variable, instrument, product version, scene conditions, and scientific application. For more flexible analyses, users can directly examine `chi2`, `nv_ref`, and `nv_dolp` and define application-specific thresholds rather than relying solely on `quality_flag`.

When comparing FastMAPOL with other datasets, both the product version and the applied quality criteria should be reported. Differences in quality filtering can affect retrieval statistics, the number of available matchups, and their spatial and temporal distribution, and should therefore be considered when interpreting intercomparisons and validation results.

## Quality Selection in Level-3 Processing

FastMAPOL Level-3 products are generated from the Level-2 retrievals using the standard OCSSW Level-3 processing tools, including `l3bin` for spatial and temporal binning and `l3mapgen` for generating mapped Level-3 products.

During Level-3 binning, the `quality_flag` is used to preferentially retain the best available quality class within each Level-3 bin. When multiple Level-2 pixels fall within the same bin, pixels with the lowest available `quality_flag` are selected for the Level-3 statistics. For example:

* if one or more `quality_flag = 0` pixels are available in a bin, these pixels are preferentially used;
* if no `quality_flag = 0` pixels are available, `quality_flag = 1` pixels may be used;
* if neither `quality_flag = 0` nor `quality_flag = 1` pixels are available, the processing can proceed to the next permitted quality level.

Thus, lower-quality retrievals do not necessarily contribute to a Level-3 bin simply because they are present. They provide additional coverage primarily where retrievals satisfying more stringent quality criteria are unavailable.

This quality-prioritized binning provides a practical balance between retrieval quality and spatial coverage. Restricting Level-3 processing exclusively to `quality_flag = 0` would provide the most stringent selection but could leave substantially more bins without valid data. Allowing additional quality levels, such as `quality_flag = 0–1` or `0–2`, can increase Level-3 coverage while still giving priority to the lowest available quality-flag value within each bin.

The general processing sequence is

$$
\text{Level-2 retrievals}
\rightarrow
\texttt{l3bin}
\rightarrow
\text{Level-3 binned data}
\rightarrow
\texttt{l3mapgen}
\rightarrow
\text{Level-3 mapped products}.
$$

Here, `l3bin` performs the binning and quality-based selection of the Level-2 observations, while `l3mapgen` maps the resulting binned data onto the requested geographic grid.

This distinction is important when interpreting Level-3 FastMAPOL products. Allowing multiple `quality_flag` values during Level-3 generation does not imply that all accepted quality classes are weighted equally within a bin. Rather, the quality flag provides a hierarchy that allows the processing to preferentially use retrievals satisfying more stringent criteria while retaining retrievals from additional quality classes to improve coverage where higher-quality retrievals are unavailable.

Consequently, the quality criteria used to generate a Level-3 product should be considered together with its spatial coverage. Users generating their own Level-3 products can adjust the accepted quality range according to their application, with a more restrictive selection emphasizing retrieval quality and a broader selection emphasizing spatial and temporal coverage.

## Uncertainty Assessment

A rigorous approach for assessing retrieval uncertainty is to propagate measurement uncertainties through the retrieval algorithm to estimate uncertainties in the retrieved parameters (see @sec-uq-theory and @sec-uq-correlation). Such uncertainty products have been computed and released for the pre-launch Day-in-the-Life (DITL) test dataset (see @sec-harp2-ditl).

Due to the additional computational cost of uncertainty propagation, pixel-level retrieval uncertainty products are not included in the current operational FastMAPOL products. Instead, the adaptive data screening and quality assessment described above provide practical measures for identifying retrievals that are less consistent with the forward model or have reduced measurement information content. In particular, adaptive data screening removes measurements that cannot be adequately represented by the forward model, thereby improving the consistency between the measurements used in the inversion and the forward model simulations. This screening is an important step in ensuring that subsequent error propagation provides an appropriate basis for retrieval uncertainty quantification.

The quality metrics and flags discussed above, however, should not be interpreted as quantitative estimates of retrieval uncertainty. Detailed pixel-level uncertainty products based on error propagation are planned for future FastMAPOL data releases.