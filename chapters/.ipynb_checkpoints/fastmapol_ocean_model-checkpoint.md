## Ocean Bio-Optical Model

### Overview

FastMAPOL retrieves ocean optical properties by coupling the aerosol retrieval with a forward radiative transfer model of ocean inherent optical properties (IOPs). The ocean optical properties are parameterized using bio-optical models that describe the spectral absorption and scattering of seawater constituents.

Three parameterizations are considered:

| Model | Parameters | Application |
|------|------------|-------------|
| Bio-1 | 1 parameter | Open ocean (Case-1 waters) |
| Bio-2 | 5 parameters | Coastal and optically complex waters |
| Bio-3 | 3 parameters | Optimized coastal parameterization |

These models describe the spectral absorption and scattering properties of four main water constituents:

- pure seawater
- phytoplankton
- non-algal particles (NAP)
- colored dissolved organic matter (CDOM)

The total inherent optical properties are constructed from the contributions of these components.

---

### Bio-1 Optical Model (Open Ocean)

The **Bio-1 model** is a simplified bio-optical parameterization designed for **open ocean waters (Case-1 waters)**. The model is derived from the generalized Bio-2 model by imposing constraints on its parameters \citep{Zhai2015,Zhai2017}.

In this model, the optical properties are parameterized primarily as a function of chlorophyll concentration $[Chl\,a]$.

---

#### Phytoplankton Absorption

The phytoplankton absorption coefficient $a_{ph}$ follows the formulation defined in the generalized model (see Section 6.3).

---

#### Particulate Absorption

For open ocean waters, the contribution of non-algal particles (NAP) is assumed negligible. Therefore the combined detrital and CDOM absorption coefficient $a_{dg}$ depends only on phytoplankton.

The value at 440 nm is parameterized as

$$
a_{dg}(440) = p_2\, a_p(440,[Chl\,a])
$$

where

$$
p_2 = 0.3 + \frac{5.7\,R_2\,a_p(440,[Chl\,a])}{0.02 + a_p(440,[Chl\,a])}.
$$

The parameter $R_2$ is assumed to be

$$
R_2 = 0.5.
$$

The exponential spectral slope is fixed as

$$
S_{dg} = 0.018.
$$

---

#### Particulate Backscattering

The particulate backscattering coefficient $b_{bp}$ is also assumed to depend only on phytoplankton in open waters.

The magnitude at 660 nm is parameterized as

$$
b_{bp}(660) = 0.347\,[Chl\,a]^{0.766}.
$$

The spectral slope of particulate backscattering is

$$
S_{bp} = -0.5\left(\log_{10}[Chl\,a] - 0.3\right),
$$

valid for

$$
0.02 < [Chl\,a] < 2 \; \text{mg m}^{-3}.
$$

Outside this range, the slope is set to zero.

---

#### Backscattering Fraction

The particulate backscattering fraction is parameterized as

$$
B_p = 0.002 + 0.01\left(0.5 - 0.25\log_{10}[Chl\,a]\right).
$$

The parameter $B_p$ is assumed to be **spectrally independent** \citep{Huot2008}.

---

### Bio-2 Optical Model (Five-Parameter Model)

The **Bio-2 model** describes optically complex coastal waters using a five-parameter parameterization.

The ocean is assumed to be homogeneous with a depth of **200 m** and composed of four optical components:

1. pure seawater
2. phytoplankton covariant particles
3. non-algal particles (NAP)
4. colored dissolved organic matter (CDOM)

CDOM contributes only to absorption, while the other components contribute to both absorption and scattering.

The absorption and scattering coefficients of pure seawater, $a_w$ and $b_w$, are taken from laboratory measurements. The backscattering fraction of pure water is assumed to be

$$
B_w = 0.5.
$$

---

#### Phytoplankton Absorption

Phytoplankton absorption is parameterized as

$$
a_{ph}(\lambda) = A_{ph}(\lambda)\,[Chl\,a]^{E_{ph}(\lambda)}.
$$

Here

- $A_{ph}(\lambda)$ and $E_{ph}(\lambda)$ are empirical coefficients
- $[Chl\,a]$ is chlorophyll concentration in mg m$^{-3}$.

---

#### CDOM and Detrital Absorption

The combined absorption coefficient of CDOM and NAP is modeled as

$$
a_{dg}(\lambda) =
a_{dg}(440)\exp\!\left[-S_{dg}(\lambda-440)\right].
$$

The parameter $S_{dg}$ represents the exponential spectral slope.

---

#### Particulate Backscattering

The particulate backscattering coefficient is modeled as

$$
b_{bp}(\lambda) =
b_{bp}(660)\left(\frac{\lambda}{660}\right)^{-S_{bp}}.
$$

The spectral slope $S_{bp}$ controls the wavelength dependence.

---

#### Backscattering Fraction

The particulate backscattering fraction is

$$
B_p(\lambda) =
B_p(660)\left(\frac{\lambda}{660}\right)^{-S_{Bp}}.
$$

The parameter $S_{Bp}$ defines the spectral variation of the backscattering fraction.

---

#### Total Inherent Optical Properties

The total absorption coefficient is

$$
a(\lambda) =
a_w(\lambda) + a_{ph}(\lambda) + a_{dg}(\lambda).
$$

The total backscattering coefficient is

$$
b_b(\lambda) =
0.5\,b_w(\lambda) + b_{bp}(\lambda).
$$

These inherent optical properties are widely used to describe the ocean color spectrum in bio-optical models.

---

### Bio-3 Optical Model (Three-Parameter Model)

The **Bio-3 model** is a reduced-parameter version of the Bio-2 formulation designed to balance physical realism with retrieval stability.

In this model, a subset of the Bio-2 parameters is retrieved while the remaining parameters are constrained using empirical relationships derived from observations.

*(Details of the Bio-3 parameterization will be described soon.)*