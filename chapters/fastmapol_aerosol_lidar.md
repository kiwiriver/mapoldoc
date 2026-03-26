# Aerosol characterization: Lidar ratio and depolarization

## Overview

The aerosol retrieval framework provides extinction and scattering properties together with the angular scattering phase matrix. These quantities can be used to derive lidar observables, in particular the **lidar ratio** and the **linear depolarization ratio**.

This chapter summarizes the mathematical connection between the retrieved aerosol optical properties and the lidar-relevant quantities. The focus here is on the forward relationships from extinction and phase matrix elements to the lidar ratio and depolarization, for total aerosol as well as for fine and coarse aerosol partitions.

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

Although the single-scattering albedo is not itself a lidar product, it provides useful context because the backscatter signal depends on the angular redistribution of the scattered power, while extinction controls the attenuation of the lidar beam.

## Phase matrix and backscattering geometry

The scattering properties of the aerosol are described by the phase matrix, written here as

$$
\mathbf{P}(\Theta),
$$

where $\Theta$ is the scattering angle. In the standard convention for randomly oriented particles with mirror symmetry, the phase matrix contains elements such as

$$
P_{11}(\Theta), \qquad P_{22}(\Theta),
$$

together with the other Mueller-matrix elements.

For lidar applications, the relevant direction is the **exact backscattering direction**, corresponding to

$$
\Theta = 180^\circ.
$$

In the numerical implementation, this corresponds to the last angular index of the phase-matrix array. Therefore, the backscatter-related quantities are obtained from the phase matrix evaluated at $\Theta = 180^\circ$.

Let

$$
P_{11}(180^\circ)
$$

and

$$
P_{22}(180^\circ)
$$

denote the first and second diagonal phase-matrix elements at exact backscatter.

## Relation between phase matrix and volume backscatter

The lidar backscatter coefficient is proportional to the phase function evaluated at the backscattering direction.

If the phase matrix is normalized such that

$$
\int_{4\pi} P_{11}(\Theta,\phi)\, d\Omega = 4\pi,
$$

then the backscatter coefficient is

$$
\beta = \frac{C_{sca}}{4\pi} P_{11}(180^\circ).
$$

Equivalently, if the retrieved quantity is an extinction coefficient rather than a cross section, the same relation holds with coefficient units instead of per-particle units. The key point is that the lidar backscatter coefficient is proportional to the product of scattering strength and the phase-matrix element in the exact backscattering direction.

Thus, for fine mode, coarse mode, and total aerosol, one may write

$$
\beta_f = \frac{C_{sca,f}}{4\pi} P_{11,f}(180^\circ),
$$

$$
\beta_c = \frac{C_{sca,c}}{4\pi} P_{11,c}(180^\circ),
$$

$$
\beta = \frac{C_{sca}}{4\pi} P_{11}(180^\circ).
$$

If the retrieval outputs a matrix-valued quantity already scaled by scattering coefficient, then the same formulas apply after confirming the normalization convention of the stored matrix.

## Lidar ratio

The lidar ratio is defined as the ratio of extinction to backscatter:

$$
S_a = \frac{\alpha}{\beta},
$$

where

- $\alpha$ is the aerosol extinction coefficient,
- $\beta$ is the aerosol backscatter coefficient.

Using the backscatter relation above,

$$
\beta = \frac{C_{sca}}{4\pi} P_{11}(180^\circ),
$$

the lidar ratio becomes

$$
S_a
= \frac{C_{ext}}{ \left(C_{sca}/4\pi\right) P_{11}(180^\circ)}
= \frac{4\pi C_{ext}}{C_{sca} P_{11}(180^\circ)}.
$$

Using the single-scattering albedo $\omega_0=C_{sca}/C_{ext}$, this can also be written as

$$
S_a = \frac{4\pi}{\omega_0 P_{11}(180^\circ)}.
$$

This form makes the physical dependence especially clear:

- larger extinction increases the lidar ratio,
- larger backscatter phase function at $180^\circ$ decreases the lidar ratio,
- larger single-scattering albedo decreases the lidar ratio by enhancing scattering relative to extinction.

### Fine, coarse, and total lidar ratio

The same definition applies separately to each aerosol partition:

$$
S_{a,f} = \frac{4\pi C_{ext,f}}{C_{sca,f} P_{11,f}(180^\circ)},
$$

$$
S_{a,c} = \frac{4\pi C_{ext,c}}{C_{sca,c} P_{11,c}(180^\circ)},
$$

$$
S_{a} = \frac{4\pi C_{ext}}{C_{sca} P_{11}(180^\circ)}.
$$

If the stored phase-matrix quantity already includes the scattering scaling, so that the backscatter term is directly represented by a matrix element $M_{11}(180^\circ)$ with dimensions of scattering coefficient, then the lidar ratio reduces to

$$
S_a = \frac{4\pi C_{ext}}{M_{11}(180^\circ)}.
$$

This is the form used in the retrieval post-processing when the matrix quantity `mv` already contains the scattering-weighted phase matrix.

Accordingly, the fine, coarse, and total lidar ratios are written as

$$
S_{a,f} = \frac{4\pi C_{ext,f}}{M_{11,f}(180^\circ)},
$$

$$
S_{a,c} = \frac{4\pi C_{ext,c}}{M_{11,c}(180^\circ)},
$$

$$
S_{a} = \frac{4\pi C_{ext}}{M_{11}(180^\circ)}.
$$

### Interpretation

The lidar ratio becomes large when the extinction is strong but the backscatter is weak. This often occurs for aerosol types that scatter predominantly away from the exact backscattering direction. Numerically, very small values of $P_{11}(180^\circ)$ or $M_{11}(180^\circ)$ can produce very large lidar ratios, so practical implementations may require a small lower bound for numerical stability.

## Linear depolarization ratio

The linear depolarization ratio quantifies the change in polarization state of the backscattered signal. It is commonly used to characterize particle nonsphericity, orientation effects, and mixtures of spherical and nonspherical scatterers.

At exact backscatter, the depolarization ratio can be derived from the diagonal phase-matrix elements. Using the backscatter phase matrix, define

$$
\delta = \frac{1 - P_{22}(180^\circ)/P_{11}(180^\circ)}
{1 + P_{22}(180^\circ)/P_{11}(180^\circ)}.
$$

Equivalently,

$$
\delta = \frac{P_{11}(180^\circ)-P_{22}(180^\circ)}
{P_{11}(180^\circ)+P_{22}(180^\circ)}.
$$

This is the expression used in the retrieval diagnostics.

### Fine, coarse, and total depolarization ratio

Applying the same formula to the fine mode, coarse mode, and total aerosol gives

$$
\delta_f =
\frac{P_{11,f}(180^\circ)-P_{22,f}(180^\circ)}
{P_{11,f}(180^\circ)+P_{22,f}(180^\circ)},
$$

$$
\delta_c =
\frac{P_{11,c}(180^\circ)-P_{22,c}(180^\circ)}
{P_{11,c}(180^\circ)+P_{22,c}(180^\circ)},
$$

$$
\delta =
\frac{P_{11}(180^\circ)-P_{22}(180^\circ)}
{P_{11}(180^\circ)+P_{22}(180^\circ)}.
$$

If the stored matrix quantity is already scaled by scattering amplitude or scattering coefficient, the same ratio applies because the scaling cancels:

$$
\delta =
\frac{M_{11}(180^\circ)-M_{22}(180^\circ)}
{M_{11}(180^\circ)+M_{22}(180^\circ)}.
$$

Thus the depolarization ratio depends only on the relative structure of the backscatter matrix, not on its absolute normalization.

### Interpretation

Several limiting cases are useful:

- If

  $$
  P_{22}(180^\circ)=P_{11}(180^\circ),
  $$

  then

  $$
  \delta = 0,
  $$

  which corresponds to no linear depolarization of the backscattered light.

- If

  $$
  P_{22}(180^\circ)<P_{11}(180^\circ),
  $$

  then

  $$
  \delta > 0,
  $$

  indicating nonzero depolarization.

- Larger deviations between $P_{22}(180^\circ)$ and $P_{11}(180^\circ)$ generally indicate stronger nonsphericity effects.

For spherical particles in many common scattering regimes, the depolarization ratio is very small, whereas irregular particles such as mineral dust can produce significantly larger values.

## Link between retrieval phase matrix and lidar observables

The retrieval provides the aerosol phase function or phase matrix, either for each mode or for the total aerosol mixture. Once the phase matrix is available at the backscattering direction, lidar properties follow directly.

The connection can be summarized as follows.

### Step 1: obtain extinction and scattering quantities

From the retrieval, obtain

$$
C_{ext}, \qquad C_{sca}, \qquad \omega_0=\frac{C_{sca}}{C_{ext}}.
$$

### Step 2: evaluate the phase matrix at backscatter

Extract

$$
P_{11}(180^\circ), \qquad P_{22}(180^\circ).
$$

In discrete form, this is the final angular element of the phase-matrix array.

### Step 3: compute backscatter coefficient

Using the normalization convention,

$$
\beta = \frac{C_{sca}}{4\pi} P_{11}(180^\circ).
$$

Or, if the matrix is already scattering-weighted,

$$
\beta = \frac{M_{11}(180^\circ)}{4\pi}.
$$

### Step 4: compute lidar ratio

$$
S_a = \frac{\alpha}{\beta}
= \frac{4\pi C_{ext}}{C_{sca}P_{11}(180^\circ)}
= \frac{4\pi}{\omega_0 P_{11}(180^\circ)}.
$$

Or in scattering-weighted form,

$$
S_a = \frac{4\pi C_{ext}}{M_{11}(180^\circ)}.
$$

### Step 5: compute depolarization ratio

$$
\delta =
\frac{P_{11}(180^\circ)-P_{22}(180^\circ)}
{P_{11}(180^\circ)+P_{22}(180^\circ)}.
$$

Or equivalently,

$$
\delta =
\frac{1-P_{22}(180^\circ)/P_{11}(180^\circ)}
{1+P_{22}(180^\circ)/P_{11}(180^\circ)}.
$$

## Mixtures of aerosol modes

If the aerosol is represented as a sum of fine and coarse components, then the total backscatter matrix is the sum of the component backscatter matrices:

$$
M_{ij}(180^\circ) = M_{ij,f}(180^\circ) + M_{ij,c}(180^\circ).
$$

Similarly, total extinction is

$$
C_{ext}=C_{ext,f}+C_{ext,c}.
$$

The total lidar ratio is then

$$
S_a = \frac{4\pi (C_{ext,f}+C_{ext,c})}
{M_{11,f}(180^\circ)+M_{11,c}(180^\circ)}.
$$

The total depolarization ratio becomes

$$
\delta =
\frac{\left[M_{11,f}(180^\circ)+M_{11,c}(180^\circ)\right]
-\left[M_{22,f}(180^\circ)+M_{22,c}(180^\circ)\right]}
{\left[M_{11,f}(180^\circ)+M_{11,c}(180^\circ)\right]
+\left[M_{22,f}(180^\circ)+M_{22,c}(180^\circ)\right]}.
$$

Therefore, the lidar properties of a mixture are not simple weighted averages of the fine- and coarse-mode lidar products; they must be recomputed from the summed extinction and summed backscatter matrix elements.

## Practical remarks on normalization

The formulas above depend on the normalization convention of the stored phase matrix.

Two common cases are:

1. **Normalized phase matrix**
   
   $$
   \int_{4\pi} P_{11} d\Omega = 4\pi
   $$
   
   In this case,

   $$
   \beta=\frac{C_{sca}}{4\pi}P_{11}(180^\circ),
   $$
   
   and

   $$
   S_a=\frac{4\pi C_{ext}}{C_{sca}P_{11}(180^\circ)}.
   $$

2. **Scattering-weighted phase matrix**
   
   If the stored matrix already includes the scattering scaling, denoted here by $M_{ij}$, then

   $$
   \beta=\frac{M_{11}(180^\circ)}{4\pi},
   $$
   
   and

   $$
   S_a=\frac{4\pi C_{ext}}{M_{11}(180^\circ)}.
   $$

The depolarization ratio is unaffected by this distinction, because any common multiplicative factor cancels in the ratio.

## Summary

The lidar ratio and linear depolarization ratio can be derived directly from the retrieved extinction and phase matrix at the exact backscattering direction.

The key relations are:

$$
S_a=\frac{\alpha}{\beta},
$$

$$
\beta=\frac{C_{sca}}{4\pi}P_{11}(180^\circ),
$$

$$
S_a=\frac{4\pi C_{ext}}{C_{sca}P_{11}(180^\circ)}
=\frac{4\pi}{\omega_0 P_{11}(180^\circ)},
$$

and

$$
\delta=
\frac{P_{11}(180^\circ)-P_{22}(180^\circ)}
{P_{11}(180^\circ)+P_{22}(180^\circ)}.
$$

When the retrieval outputs a scattering-weighted backscatter matrix, these become

$$
S_a=\frac{4\pi C_{ext}}{M_{11}(180^\circ)},
$$

$$
\delta=
\frac{M_{11}(180^\circ)-M_{22}(180^\circ)}
{M_{11}(180^\circ)+M_{22}(180^\circ)}.
$$

These relations provide the mathematical bridge between the retrieved aerosol phase matrix and lidar-observable quantities for fine mode, coarse mode, and the total aerosol mixture.