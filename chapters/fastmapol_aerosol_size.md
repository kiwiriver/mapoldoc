# Aerosol characterization : size distribution and representation

FastMAPOL supports several aerosol representations with increasing complexity:

- **Aer-1:** five predefined log-normal modes
- **Aer-2:** six predefined log-normal modes
- **Aer-3:** aerosol components represented by chemical composition

These aerosol models share a common mathematical framework for size integration, mode summation, optical properties, and bulk aerosol products. This chapter summarizes the formulation used to connect the retrieved state variables to physically meaningful aerosol quantities, including optical depth, effective radius, effective variance, fine-mode fraction, and volume fraction. In this chapter, we focused on the first two models. 

## Overview

The aerosol model formulation includes the following key elements:

- size integration over each particle size distribution
- calculation of effective radius and effective variance
- multi-mode considerations
- derivation of aerosol products such as fine-volume fraction (FVF) and fine-mode fraction (FMF)

## Aerosol density distributions

### Conversion between volume and number density log-normal distributions

Both the number density and the volume density can be represented by log-normal distributions. The two forms are equivalent and can be converted into each other by changing the mean radius and the normalization constant.

For a monomodal system, the volume and number size distributions are written as

$$
\frac{d V(r)}{d \ln r}= \frac{V_{0}}{\sqrt{2 \pi} \sigma_{v}} \exp\left[-\frac{(\ln r-\ln r_{v})^2}{2 \sigma_{v}^2}\right],
$$

$$
\frac{d N(r)}{d \ln r}= \frac{N_{0}}{\sqrt{2 \pi} \sigma_{n}} \exp\left[-\frac{(\ln r-\ln r_{n})^2}{2 \sigma_{n}^2}\right].
$$

Here, $r_v$ and $r_n$ are the mean radii for the volume and number distributions, respectively, and $\sigma_v$ and $\sigma_n$ are the corresponding logarithmic standard deviations. The formulation follows Xu (2016, Appendix A6), with the corrected definition of $\sigma$ as confirmed by Pengwang Zhai through personal communication with Feng Xu.

Since particle volume and number are related by

$$
V=\frac{4\pi}{3}Nr^3,
$$

the corresponding differential distributions satisfy

$$
\frac{d V(r)}{d \ln r}=\frac{4\pi}{3} r^3 \frac{d N(r)}{d \ln r}.
$$

Using this relation, the following identities are obtained:

$$
\sigma_{n}=\sigma_{v}=\sigma,
$$

$$
r_{n}=r_{v} \exp(-3 \sigma^2),
$$

$$
N_{0}=V_{0} \frac{3}{4 \pi r_{v}^3}\exp(4.5 \sigma^2),
$$

$$
N_{0}=V_{0} \frac{3}{4 \pi r_{n}^3}\exp(-4.5 \sigma^2).
$$

These results are consistent with the derivation given by R.G. Grainger. The last two expressions provide an internal consistency check.

## Optical depth

Using the conversion above, the column-integrated number density can be expressed in terms of the column-integrated volume density as

$$
N_{0}=V_{0} \frac{3}{4 \pi r_{v}^3}\exp(4.5 \sigma^2).
$$

The aerosol optical depth is defined as

$$
\tau_a=\sum_{i=1}^{N_c} C_{ext,i} N_i,
$$

where $C_{ext,i}$ is the extinction cross section of component $i$ in units of $\mu m^2$, and $N_i$ is the corresponding column number density in units of $\mu m^{-2}$.

Here, column number density is equivalent to vertically integrated number density. If the local number density is $n$ in units of $\mu m^{-3}$ and the layer thickness is $z$ in units of $\mu m$, then

$$
N = n z.
$$

Using column volume density, the optical depth can be rewritten as

$$
\tau_a = \sum_{i=1}^{N_c} C_{ext,i} N_i,
$$

$$
\tau_a = \sum_{i=1}^{N_c} C_{ext,i} V_i \frac{3}{4 \pi r_{v}^3}\exp(4.5 \sigma^2),
$$

$$
\tau_a = \sum_{i=1}^{N_c} \left[C_{ext,i} \frac{3}{4 \pi r_{v}^3}\exp(4.5 \sigma^2)\right] V_i.
$$

The unit of column volume density is $\mu m^3 \mu m^{-2}$, which reduces to $\mu m$.

If a maximum optical depth $\tau_{max}$ is prescribed, then the corresponding maximum allowed volume density for each component is

$$
V_i=\tau_{max}\left[C_{ext,i} \frac{3}{4 \pi r_{v}^3}\exp(4.5 \sigma^2)\right]^{-1},
$$

or equivalently

$$
V_i=\tau_{max} \exp(4.5 \sigma^2) \frac{4 \pi r_{v}^3}{3 C_{ext,i}}.
$$

A practical bound on $V_i$ can be estimated at a reference wavelength such as $550 \, nm$ by computing extinction cross sections over the expected refractive-index range for each mode. For the lower bound, one may use a real refractive index close to that of water at $550 \, nm$, approximately 1.3.

In general, increasing the imaginary part of the refractive index increases the extinction cross section. For coarse particles, however, the extinction is often much less sensitive to refractive index than to particle size.

## Convention for size distributions

The size distributions may be defined with respect to either $r$ or $\ln r$. These forms are related by

$$
\frac{d N(r)}{d r}=\frac{1}{r}\frac{d N(r)}{d \ln r},
$$

$$
\frac{d V(r)}{d r}=\frac{1}{r}\frac{d V(r)}{d \ln r}.
$$

This distinction is important when comparing formulations from different references or numerical implementations.

## Moments of the particle size distribution

For later use, define the $k$th moment of the number size distribution as

$$
m_k=\int_{0}^{\infty} r^k \frac{d N(r)}{d r} \, d r,
$$

which is equivalently

$$
m_k=\int_{-\infty}^{\infty} r^k \frac{d N(r)}{d \ln r} \, d \ln r.
$$

For a log-normal number distribution,

$$
m_k=N_0 r_n^k \exp\left(\frac{k^2}{2} \sigma^2\right).
$$

The dimension is consistent: $m_k$ has units of $r^k$, while $\sigma$ is dimensionless.

Substituting the number-volume conversion into the moment expression gives

$$
m_k=N_0 r_n^k \exp\left(\frac{k^2}{2} \sigma^2\right),
$$

$$
m_k=V_{0} \frac{3}{4 \pi r_{v}^3}\exp(4.5 \sigma^2)r^k_{v} \exp(-3k \sigma^2) \exp\left(\frac{k^2}{2} \sigma^2\right),
$$

and therefore

$$
m_k=V_{0} \frac{3}{4 \pi} r^{k-3}_v\exp\left[\frac{(k-3)^2\sigma^2}{2} \right].
$$

This result preserves dimensional consistency because $V_0$ carries the dimension of $r^3$.

A useful special case is $k=3$, for which

$$
m_3=\frac{3V_0}{4\pi}.
$$

Thus, the third moment depends directly on the total particle volume.

## Effective radius and effective variance in terms of $dN/d\ln r$

The effective radius, or area-weighted mean radius, is defined as

$$
r_{eff}=\frac{\int_{r_1}^{r_2} r \, r^2 (dN(r)/dr) \, dr}{\int_{r_1}^{r_2} r^2 (dN(r)/dr) \, dr}
=\frac{m_3}{m_2}.
$$

The weighting by particle area reflects the fact that radiative extinction is approximately proportional to particle cross-sectional area.

The effective variance is defined as

$$
\nu_{eff}=\frac{\int_{r_1}^{r_2}(r-r_{eff})^2 r^2 (dN(r)/dr) \, dr}
{r^2_{eff} \int_{r_1}^{r_2}r^2 (dN(r)/dr) \, dr},
$$

which can be written in terms of moments as

$$
\nu_{eff}= \frac{m_4 m_2}{m_3^2}-1.
$$

This expression is equally valid for distributions written in terms of $dN(r)/d\ln r$.

For a log-normal number distribution

$$
\frac{d N(r)}{d \ln r}=\frac{N_0}{\sqrt{2 \pi} \sigma} \exp\left[-\frac{(\ln r-\ln r_n)^2}{2 \sigma^2}\right],
$$

the effective radius and effective variance are

$$
r_{eff}=r_n \exp\left(\frac{5}{2} \sigma^2\right),
$$

$$
\nu_{eff}=\exp (\sigma^2)-1.
$$

Using the relation between number-mean and volume-mean radius,

$$
r_{eff}=r_n \exp(2.5 \sigma^2),
$$

$$
r_{eff}=r_v \exp(-3 \sigma^2) \exp(2.5 \sigma^2),
$$

$$
r_{eff}=r_v \exp(-0.5 \sigma^2).
$$

Thus, the effective radius is always larger than the number-mean radius and smaller than the volume-mean radius.

For small $\sigma^2$, the approximations

$$
\nu_{eff}=\exp (\sigma^2)-1 \approx \sigma^2
$$

and

$$
r_{eff}= r_v \exp(-0.5 \sigma^2) \approx r_v
$$

show that the effective variance is approximately the square of the logarithmic standard deviation, while the effective radius approaches the volume-mean radius.

## Effective radius and variance in terms of $dV/d\ln r$

Because the retrieval state is often expressed in terms of volume density, it is useful to write effective radius and effective variance directly in terms of the volume distribution.

Starting from

$$
\frac{d V(r)}{d \ln r}=\frac{4\pi}{3} r^3 \frac{d N(r)}{d \ln r},
$$

the effective radius becomes

$$
r_{eff}=\frac{\int_{\ln r_1}^{\ln r_2} (dV(r)/d\ln r) \, d\ln r}
{\int_{\ln r_1}^{\ln r_2} r^{-1} (dV(r)/d\ln r) \, d\ln r}.
$$

Define the moments of the volume distribution as

$$
m'_k = \int_{\ln r_1}^{\ln r_2} r^k \left(\frac{dV(r)}{d\ln r}\right) d\ln r.
$$

Then

$$
r_{eff}= \frac{m'_0}{m'_{-1}}.
$$

Similarly, the effective variance becomes

$$
\nu_{eff}=\frac{\int_{\ln r_1}^{\ln r_2}(r-r_{eff})^2 r^{-1} (dV(r)/d\ln r) \, d\ln r}
{r^2_{eff} \int_{\ln r_1}^{\ln r_2} r^{-1} (dV(r)/d\ln r) \, d\ln r},
$$

which simplifies to

$$
\nu_{eff}= \frac{m'_{-1} m'_1}{m'^2_0}-1.
$$

For discrete numerical implementation, the moment may be written schematically as

$$
m'_k \approx \sum_i r(i)^k v(i,j),
$$

where $i$ indexes the size grid and $j$ indexes pixel or retrieval location. The constant increment $d\ln r$ is omitted here for brevity.

For a log-normal volume distribution,

$$
m'_k =V_{0} \frac{3}{4 \pi} r^{k}_v\exp\left[\frac{k^2\sigma^2}{2} \right].
$$

For a multimode distribution, the effective quantities become

$$
r_{eff}=\frac{\sum_i m_{3,i}}{\sum_i m_{2,i}}
$$

and

$$
\nu_{eff}=\frac{\sum_i m_{4,i} \sum_i m_{2,i}}{(\sum_i m_{3,i})^2}-1
$$

when expressed through number-density moments, or equivalently

$$
r_{eff}=\frac{\sum_i m'_{0,i}}{\sum_i m'_{-1,i}},
$$

$$
\nu_{eff}=\frac{\sum_i m'_{1,i} \sum_i m'_{-1,i}}{(\sum_i m'_{0,i})^2}-1
$$

when expressed through volume-density moments.

## Multi-mode distributions

When the aerosol is represented as a sum of log-normal modes,

$$
\frac{d V(r)}{d \ln r}= \sum_i \frac{V_{0,i}}{\sqrt{2 \pi} \sigma_{v,i}} \exp\left[-\frac{(\ln r-\ln r_{v,i})^2}{2 \sigma_{v,i}^2}\right],
$$

$$
\frac{d N(r)}{d \ln r}= \sum_i \frac{N_{0,i}}{\sqrt{2 \pi} \sigma_{n,i}} \exp\left[-\frac{(\ln r-\ln r_{n,i})^2}{2 \sigma_{n,i}^2}\right].
$$

For each mode, the conversion relations are

$$
\sigma_{n,i}=\sigma_{v,i}=\sigma_i,
$$

$$
r_{n,i}=r_{v,i} \exp(-3 \sigma_{i}^2),
$$

$$
N_{0,i}=V_{0,i} \frac{3}{4 \pi r_{v,i}^3}\exp(4.5 \sigma_{i}^2),
$$

$$
N_{0,i}=V_{0,i} \frac{3}{4 \pi r_{n,i}^3}\exp(-4.5 \sigma_{i}^2).
$$

The distinction between number weighting and volume weighting remains important, since the same physical aerosol can have different mean radii depending on which representation is used.

For the volume-mode parameterization adopted from @Dubovik:2011aa and @Xu:2016aa, the nominal mode radii are

$$
0.1,\ 0.1732,\ 0.3,\ 1.0,\ 2.9 \ \mu m
$$

with logarithmic standard deviations

$$
0.35,\ 0.35,\ 0.35,\ 0.5,\ 0.5.
$$

The impact of the number of modes are further discussed by Fu:2020aa. We adopted this approach as default aerosol model (Aer-1). @Gao:2018aa add an additional coarse mode to deal with the larger aerosol size when using the SWIR bands from RSP. The nomial radii and standard deviations are 8.4 $\mu m$ and 0.5, which is called (Aer-2). 

## Averaged extinction cross section

When extinction is averaged over a volume-based aerosol model, number density must first be recovered from the volume density.

For the same total particle volume, a fine mode contains far more particles than a coarse mode. As a result, fine-mode particles can dominate number-weighted optical properties even when their volume contribution is modest.

For two modes, the total extinction can be written as

$$
C_{ext,t}=C_{ext,f} n_f+C_{ext,c} n_c,
$$

or in terms of volume density,

$$
C_{ext,t}= C_{ext,f} \frac{3 V_f}{4\pi r^3_{v,f}}\exp(4.5 \sigma^2_f)+
C_{ext,c} \frac{3 V_c}{4\pi r^3_{v,c}}\exp(4.5 \sigma^2_c).
$$

To compare fine- and coarse-mode contributions, consider the ratio

$$
\frac{C_{ext,f} \frac{3 V_f}{4\pi r^3_{v,f}}\exp(4.5 \sigma^2_f)}
{C_{ext,c} \frac{3 V_c}{4\pi r^3_{v,c}}\exp(4.5 \sigma^2_c)}.
$$

If the logarithmic standard deviations are assumed equal, this ratio simplifies to an expression proportional to

$$
\frac{V_f}{V_c}\frac{Q_{ext,f} r_{v,c}}{Q_{ext,c} r_{v,f}},
$$

showing explicitly how fine-mode dominance arises from both larger number concentration per unit volume and the size dependence of extinction efficiency.

## Effective radius and variance for multimode aerosols

The total number density and total volume density are

$$
N_0=\sum_i N_{0,i},
$$

$$
V_0=\sum_i V_{0,i}.
$$

The effective radius and effective variance of the combined distribution are

$$
r_{eff}=\frac{\sum_i m_{3,i}}{\sum_i m_{2,i}},
$$

$$
\nu_{eff}=\frac{\sum_i m_{4,i} \sum_i m_{2,i}}{(\sum_i m_{3,i})^2}-1.
$$

### Sensitivity to component volume

To propagate retrieval uncertainty from mode volumes to bulk effective properties, derivatives with respect to $V_j$ are required.

For effective radius,

$$
\frac{\partial r_{eff}}{\partial V_{j}} =
\frac{1}{\sum_i  m_{2,i}}\frac{\partial m_{3,j}}{\partial V_j}- 
\frac{\sum_i m_{3,i}}{(\sum_i m_{2,i})^2}\frac{\partial m_{2,j}}{\partial V_j},
$$

which can be written as

$$
\frac{\partial r_{eff}}{\partial V_{j}} =
r_{eff} \left(\frac{m'_{3,j}}{\sum_i  m_{3,i}}-\frac{m'_{2,j}}{\sum_i  m_{2,i}}\right).
$$

For effective variance,

$$
\frac{\partial \nu_{eff}}{\partial V_j}=
\frac{\sum_i m_{2,i}}{(\sum_i m_{3,i})^2}\frac{\partial m_{4,j}}{\partial V_j}+
\frac{\sum_i m_{4,i}}{(\sum_i m_{3,i})^2}\frac{\partial m_{2,j}}{\partial V_j}
-2\frac{\sum_i m_{4,i} \sum_i m_{2,i}}{(\sum_i m_{3,i})^3}\frac{\partial m_{3,j}}{\partial V_j},
$$

or equivalently

$$
\frac{\partial \nu_{eff}}{\partial V_j}=
(\nu_{eff}+1) \left(\frac{m'_{4,j}}{\sum_i  m_{4,i}}+\frac{m'_{2,j}}{\sum_i m_{2,i}} 
-2\frac{m'_{3,j}}{\sum_i  m_{3,i}}\right).
$$

Here the derivative of the moment with respect to component volume is

$$
m'_{k,j}=\frac{\partial m_{k,j}}{\partial V_j}=\frac{m_{k,j}}{V_j},
$$

which gives

$$
m'_{k,j}=\frac{3}{4 \pi}
r^{k-3}_{v,j} \exp\left[\frac{(k-3)^2}{2} \sigma_j^2\right].
$$

For $k=3$, this derivative is constant because $m_3$ depends linearly on volume and not on radius.

## Physical interpretation of low-order moments

For a volume-based log-normal mode, the low-order moments are

$$
m_{2,i}=\frac{3}{4\pi} V_i r_{v,i}^{-1} \exp(\sigma_i^2/2),
$$

$$
m_{3,i}=\frac{3}{4\pi} V_i,
$$

$$
m_{4,i}=\frac{3}{4\pi} V_i r_{v,i} \exp(\sigma_i^2/2).
$$

For a single mode, these lead directly to

$$
r_{eff}=\frac{m_3}{m_2}=r_v \exp(-\sigma^2/2),
$$

$$
\nu_{eff}=\frac{m_4 m_2}{m_3^2}-1=\exp (\sigma^2)-1.
$$

For a multimode aerosol,

$$
\sum_i m_{3,i}=\frac{3}{4\pi}\sum_i V_i=\frac{3}{4\pi} V_0,
$$

so the summed third moment is proportional to the total particle volume.

Within a fine-mode or coarse-mode subset, $m_2$ emphasizes smaller particles, whereas $m_4$ emphasizes larger particles. This makes effective radius and effective variance sensitive to the relative partitioning among submodes, even when the logarithmic standard deviation is fixed within that subset.

## Fine and coarse mode partitions

To discuss fine-mode and coarse-mode aerosol separately, the mode summation may be restricted to selected ranges.

For the Aer-1 and Aer-2 parameterization adopted here:

- **fine mode:** modes 1 to 3, with volume-mean radii from $0.1$ to $0.3 \, \mu m$
- **coarse mode:** modes 4 and above, with volume-mean radii from $1.0$, $2.9, (and 8.4) \mu m$

Although the first three fine modes share the same logarithmic standard deviation, their effective variance can still change because the bulk quantity depends on the relative partitioning among the mode volumes.

## Phase-matrix averaging

The total number density may be represented as a weighted sum of component size distributions,

$$
n(r)=\sum_i c_i n_i(r),
$$

with

$$
\sum_i c_i =1.
$$

The corresponding size-averaged phase matrix is then

$$
P(\Omega)_{avg}=\int d r \, n(r) P(\Omega;r),
$$

which can be expanded as

$$
P(\Omega)_{avg}= \sum_i c_i \int d r \, n_i(r) P(\Omega;r)
$$

and written as

$$
P(\Omega)_{avg}= \sum_i c_i P_i (\Omega).
$$

If $P_i$ denotes the size-averaged phase matrix of component $i$, then the total phase matrix is a weighted sum over components. If normalized phase matrices $\tilde{P}_i$ are used, then the weighting must be consistent with the corresponding scattering cross sections.

In practice, this requires confirming the normalization convention of the scattering subroutine, such as `spher_interface`.

## Additional notes on units

The column number density may also be written as

$$
N_0=n_0 \delta l,
$$

where $\delta l$ is the aerosol layer thickness.

In log-normal expressions, the absolute unit of radius does not matter as long as $r$ and the mean radius use the same unit, since

$$
\ln r-\ln r_m=\ln(r/r_m).
$$

Both the logarithmic variance and the effective variance are dimensionless.

## Optical depth at different wavelengths

If the aerosol loading is parameterized at a reference wavelength, such as $555 \, nm$, then the corresponding number density can be defined as

$$
n_i=\frac{\tau_{555nm}}{C_{ext,555nm}}.
$$

The optical depth at any other wavelength is then

$$
\tau_{\lambda}=n_i C_{ext,\lambda}.
$$

This relation is useful when the state variable is defined through reference-wavelength optical depth while the forward model requires spectrally varying extinction.


## Aerosol products

The aerosol model supports the derivation of bulk products from the retrieved mode volumes and optical properties. These include, for example:

- **FMF:** fine-mode fraction, typically defined from fine-mode optical depth relative to total optical depth at a reference wavelength
- **FVF:** fine-volume fraction, defined from fine-mode particle volume relative to total particle volume
- effective radius and effective variance for fine, coarse, or total aerosol
- spectrally resolved optical depth and related intensive properties

The exact definition of each product depends on the chosen aerosol representation and on whether the partitioning is performed by mode, by size, or by composition.

## Summary

The aerosol model is built on a consistent log-normal framework that links retrieved mode volumes to number density, optical depth, size moments, and bulk aerosol properties. The volume-based representation is particularly convenient for retrieval, while the number-based representation remains useful for interpreting extinction and other radiatively weighted quantities. The relationships summarized here provide the basis for size integration, mode summation, and the calculation of operational aerosol products in FastMAPOL.