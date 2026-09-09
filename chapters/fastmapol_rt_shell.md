# Radiative Transfer: Spherical Shell Correction {#sec-rt-shell}

**Implementation status**

| Model| DITL | PACE V3 | PACE V4 | Evaluation | Planned |
| --- |:---:|:---:|:---:|:---:|:---:|
| — | x | x | x | — | — |

As discussed in @sec-rt-model and @Gao:2023aa, the FastMAPOL forward-model training data are generated using a PACE-tailored vector radiative transfer model based on the successive orders of scattering method [@Zhai:2022bb]. An improved pseudo-spherical shell (IPSS) correction is applied to account for Earth curvature and improve radiative transfer accuracy, particularly at large solar and viewing zenith angles [@Zhai:2022aa].

Reflectance and degree of linear polarization (DoLP) are simulated at the PACE satellite altitude of approximately 676 km above Earth's surface. Because of Earth curvature, the solar and viewing zenith angles at the satellite altitude differ from those defined at the Earth's surface, as illustrated in @fig-system. The IPSS treatment accounts for this geometric difference when performing the radiative transfer calculations.

![Spherical-shell geometry and transformation between satellite and surface viewing geometries. Adapted from @Gao:2023aa.](../figure/fig_rt_shell.png){#fig-system width=10cm}

In FastMAPOL, the solar and viewing geometries used as inputs to the NN forward models are defined at the Earth's surface, consistent with the geometry convention used in the PACE Level-1C (L1C) products. Thus, all FastMAPOL forward-model inputs and PACE L1C observations use the same geometric reference convention.

In @fig-system, $\theta'_0$ and $\theta'_v$ denote the solar and viewing zenith angles defined at the satellite altitude, while $\theta_0$ and $\theta_v$ denote the corresponding angles at the Earth's surface. The transformations between these geometries follow @Zhai:2022aa. The solar and viewing azimuth angles are treated consistently within the corresponding reference frames but are not shown in the figure.

This treatment allows the radiative transfer calculations to account for the spherical-shell geometry of the Earth–atmosphere system while maintaining surface-referenced geometry as the common interface between the PACE L1C measurements and the FastMAPOL NN forward models.



