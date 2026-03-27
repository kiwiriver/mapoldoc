# Aerosol characterization: Backscattering fraction and its relation to lidar ratio and single-scattering albedo

## Overview

The aerosol retrieval framework provides extinction and scattering properties together with the angular scattering phase matrix. These quantities determine the strength of the backscattered signal measured by a lidar system. In addition to the lidar ratio and depolarization ratio, another useful diagnostic quantity is the **backscattering fraction**, which quantifies the relative strength of scattering into the exact backscattering direction compared to the total scattering.

This chapter summarizes the mathematical relationship between the **lidar ratio**, the **single-scattering albedo**, and the **backscattering fraction**. The focus is on the forward relationships linking extinction, scattering, and backscatter quantities, and on the interpretation of these quantities for total aerosol as well as for fine and coarse aerosol partitions.

---

## Scattering and extinction quantities

For a given aerosol population, let

$$
C_{ext}
$$

denote the extinction coefficient or extinction cross section, and let

$$
C_{sca}
$$

denote the scattering coefficient or scattering cross section.

The single-scattering albedo is defined as

$$
\omega_0 = \frac{C_{sca}}{C_{ext}}.
$$

For a partitioned aerosol model, the same definition applies separately to the fine mode, coarse mode, and total aerosol:

$$
\omega_{0,f} = \frac{C_{sca,f}}{C_{ext,f}},
$$

$$
\omega_{0,c} = \frac{C_{sca,c}}{C_{ext,c}},
$$

$$
\omega_{0} = \frac{C_{sca}}{C_{ext}}.
$$

The single-scattering albedo describes the relative contribution of scattering to extinction. Although it is not itself a lidar observable, it provides an important link between extinction and backscatter quantities.

---

## Backscatter coefficient

The lidar backscatter coefficient represents the strength of scattering into the exact backscattering direction.

The lidar ratio is defined as

$$
S_a = \frac{C_{ext}}{\beta},
$$

where

- $C_{ext}$ is the aerosol extinction coefficient,
- $\beta$ is the aerosol backscatter coefficient.

Rearranging the definition gives

$$
\beta = \frac{C_{ext}}{S_a}.
$$

---

## Definition of backscattering fraction

The **backscattering fraction** quantifies the fraction of total scattering directed into the exact backscattering direction. It is defined as

$$
\beta_{\mathrm{frac}} = \frac{\beta}{C_{sca}}.
$$

Substituting the expression for the backscatter coefficient,

$$
\beta_{\mathrm{frac}}
=
\frac{C_{ext}/S_a}{C_{sca}}.
$$

---

## Relation between backscattering fraction and single-scattering albedo

Using the definition of single-scattering albedo,

$$
\omega_0 = \frac{C_{sca}}{C_{ext}},
$$

we can write

$$
C_{sca} = \omega_0 C_{ext}.
$$

Substituting this into the expression for backscattering fraction,

$$
\beta_{\mathrm{frac}}
=
\frac{C_{ext}}
{S_a \, \omega_0 C_{ext}}.
$$

Canceling the extinction coefficient yields

$$
\boxed{
\beta_{\mathrm{frac}}
=
\frac{1}{S_a \, \omega_0}
}
$$

This relation provides a direct connection between the lidar ratio, the single-scattering albedo, and the backscattering fraction.

---

## Fine, coarse, and total backscattering fraction

Applying the same definition to each aerosol partition gives

$$
\beta_{\mathrm{frac},f}
=
\frac{1}{S_{a,f} \, \omega_{0,f}},
$$

$$
\beta_{\mathrm{frac},c}
=
\frac{1}{S_{a,c} \, \omega_{0,c}},
$$

$$
\beta_{\mathrm{frac}}
=
\frac{1}{S_a \, \omega_0}.
$$

Therefore, the backscattering fraction for each mode depends directly on the corresponding lidar ratio and single-scattering albedo.

---

## Units and interpretation

The backscattering fraction defined here corresponds to scattering into the exact backscattering direction. Its units are

$$
\mathrm{sr^{-1}},
$$

because the lidar ratio has units of steradians and the single-scattering albedo is dimensionless.

The relation

$$
\beta_{\mathrm{frac}}=\frac{1}{S_a \, \omega_0}
$$

provides a clear physical interpretation:

- Larger lidar ratio corresponds to weaker backscatter relative to extinction.
- Smaller single-scattering albedo (more absorbing aerosol) increases the inferred backscattering fraction.
- Strong forward-scattering particles tend to produce small backscattering fractions and large lidar ratios.

---

## Summary

The backscattering fraction can be derived directly from the lidar ratio and the single-scattering albedo without explicit evaluation of the phase matrix, provided that the backscatter coefficient is defined consistently.

The key relations are:

$$
\beta = \frac{C_{ext}}{S_a},
$$

$$
\omega_0 = \frac{C_{sca}}{C_{ext}},
$$

and

$$
\beta_{\mathrm{frac}}
=
\frac{1}{S_a \, \omega_0}.
$$

These relations establish the mathematical connection between extinction, scattering, and backscatter quantities used in aerosol characterization and lidar diagnostics.