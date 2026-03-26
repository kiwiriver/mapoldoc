# Land Surface Reflectance Model (Ross–Li BRDF and Polarized BPDF)

This document describes the mathematical formulation of the land surface reflectance model implemented in the **PACE simulator**. 
**Need proof read to be consistent with the code and formulation**

The model combines:
- Ross–Li kernel-driven BRDF  
- Polarized bidirectional reflectance distribution function (BPDF)  
- Fresnel reflection physics  

The output is a **4×4 Mueller matrix** describing polarized surface reflection.

## Surface Reflectance Model

The polarized land surface reflectance is expressed as

$$
\mathbf{R}(\theta_s,\theta_v,\phi) =
|\cos\theta_s|
\left[
f_{iso}
+
f_{vol} K_{vol}(\theta_s,\theta_v,\phi)
+
f_{geo} K_{geo}(\theta_s,\theta_v,\phi)
\right]\mathbf{I}
+
B_{pol}\mathbf{K}_{pol}
$$

**Questions: why $|\cos\theta_s|$ is used here, how $\mathbf{R}$ is defined?**

### Parameter Definitions

| Parameter | Description |
|---|---|
| $ \theta_s $ | solar zenith angle |
| $ \theta_v $ | viewing zenith angle |
| $ \phi $ | relative azimuth angle |
| $ f_{iso} $ | isotropic reflectance coefficient |
| $ f_{vol} $ | volumetric scattering coefficient |
| $ f_{geo} $ | geometric scattering coefficient |
| $ B_{pol} $ | polarization strength |

### Scaled Parameter Formulation (Training Representation)

For parameter sampling (e.g., in neural network training), a scaled formulation similar to **RemoTAP** is used:

$$
\mathbf{R}(\theta_s,\theta_v,\phi) =
|\cos\theta_s|
\left[
f'_{iso}(\lambda)
\left(
1
+
f'_{vol} K_{vol}
+
f'_{geo} K_{geo}
\right)
\right]\mathbf{I}
+
B_{pol}\mathbf{K}_{pol}
$$

where:
- $ f'_{iso}(\lambda) $ is spectrally dependent  
- $ f'_{vol} $, $ f'_{geo} $ are spectrally invariant  

The relationship to the physical parameters is:

$$
f_{iso}(\lambda) = f'_{iso}(\lambda)
$$

$$
f_{vol}(\lambda) = f'_{iso}(\lambda) \times f'_{vol}
$$

$$
f_{geo}(\lambda) = f'_{iso}(\lambda) \times f'_{geo}
$$

### Geometry Definition

Define cosine terms:

$$
\mu_s = \cos\theta_s
$$

$$
\mu_v = \cos\theta_v
$$

Sine terms:

$$
\sin\theta_s = \sqrt{1-\mu_s^2}
$$

$$
\sin\theta_v = \sqrt{1-\mu_v^2}
$$

Relative azimuth:

$$
\phi_r = \phi_v - \phi_s
$$

### RTSOS Geometry Convention

The model follows the **RTSOS photon ray convention**.

#### Geometry Definitions

| Quantity | RTSOS definition |
|---|---|
| solar zenith | $ \theta_s > 90^\circ $ |
| solar azimuth | $ \phi_s = 0 $ |
| viewing zenith | $ \theta_v < 90^\circ $ |
| relative azimuth | $ \phi_r = \phi_v - \phi_s $ |

#### Scattering Planes

| Condition | Geometry |
|---|---|
| $ \phi_r = 0 $ | forward scattering |
| $ \phi_r = \pi $ | backscattering |

#### Scattering Angle

The scattering angle between illumination and viewing directions is

$$
\cos\Theta =
\mu_s\mu_v +
\sqrt{1-\mu_s^2}\sqrt{1-\mu_v^2}\cos(\phi_r)
$$

$$
\Theta = \arccos(\cos\Theta)
$$

The phase angle used in BRDF models is

$$
\Theta_{phase} = \pi - \Theta
$$

## Ross Volumetric Kernel

The volumetric kernel represents multiple scattering within vegetation canopies.

$$
K_{vol} =
\frac{\left(\frac{\pi}{2}-\Theta\right)\cos\Theta + \sin\Theta}
{|\mu_s| + |\mu_v|}
-
\frac{\pi}{4}
$$

Reference: Wanner et al. (1995), JGR.

## Li Geometric Kernel (Li-Sparse-R)

Define tangent terms:

$$
\tan\theta_s = \frac{\sqrt{1-\mu_s^2}}{|\mu_s|}
$$

$$
\tan\theta_v = \frac{\sqrt{1-\mu_v^2}}{|\mu_v|}
$$

Projected angles:

$$
\theta'_s = \tan^{-1}(B/R \cdot \tan\theta_s)
$$

$$
\theta'_v = \tan^{-1}(B/R \cdot \tan\theta_v)
$$

| Parameter | Meaning |
|---|---|
| $ B/R $ | vertical-to-horizontal crown ratio |

### Projected Phase Angle

$$
\cos\xi =
\cos\theta'_s\cos\theta'_v +
\sin\theta'_s\sin\theta'_v\cos(\phi_r+\pi)
$$

### Projected Distance

$$
D =
\sqrt{
\tan^2\theta'_s +
\tan^2\theta'_v -
2\tan\theta'_s\tan\theta'_v\cos(\phi_r+\pi)
}
$$

Define:

$$
sec_s = \frac{1}{\cos\theta'_s}
$$

$$
sec_v = \frac{1}{\cos\theta'_v}
$$

### Ancillary Function (meaning?)

$$
\cos t =
\frac{H/B
\sqrt{
D^2 + (\tan\theta'_s\tan\theta'_v\sin(\phi_r+\pi))^2
}}
{sec_s + sec_v}
$$

If

$$
|\cos t| > 1
$$

then

$$
O = 0
$$

Otherwise

$$
O =
(\arccos(\cos t) - \sin t \cos t)
\frac{sec_s + sec_v}{\pi}
$$

## Final Geometric Kernel

$$
K_{geo} =
O
-
sec_s
-
sec_v
+
\frac{1}{2}(1+\cos\xi)sec_s sec_v
$$

## Polarized Surface Reflectance (BPDF)

Define scattering cosine:

$$
C =
\mu_s\mu_v +
\sqrt{1-\mu_s^2}\sqrt{1-\mu_v^2}\cos\phi_r
$$

$$
\theta_1 =
\frac{\pi - \arccos(C)}{2}
$$

### Fresnel Reflection

Let the refractive index be

$$
n = n_r + i n_i
$$

Parallel polarization:

$$
r_p =
\frac{n^2\cos\theta_1 - \sqrt{n^2-\sin^2\theta_1}}
{n^2\cos\theta_1 + \sqrt{n^2-\sin^2\theta_1}}
$$

Perpendicular polarization:

$$
r_s =
\frac{\cos\theta_1 - \sqrt{n^2-\sin^2\theta_1}}
{\cos\theta_1 + \sqrt{n^2-\sin^2\theta_1}}
$$

**Assumption: $ n = 1.5 + 0i $, for leaves?**

### Fresnel Mueller Matrix

$$
A = \frac{1}{2}|r_p|^2
$$

$$
B = \frac{1}{2}|r_s|^2
$$

$$
\gamma = r_p^* r_s
$$

$$
F =
\begin{bmatrix}
A+B & A-B & 0 & 0 \\
A-B & A+B & 0 & 0 \\
0 & 0 & Re(\gamma) & -Im(\gamma) \\
0 & 0 & Im(\gamma) & Re(\gamma)
\end{bmatrix}
$$

### Polarization Kernel

$$
\mathbf{K}_{pol} =
\frac{
e^{-\tan\theta_1}e^{-v_{fac}}
}{4(|\mu_s|+|\mu_v|)}
F
$$

with

$$
v_{fac} = 0.1
$$

### Final Surface Mueller Matrix

$$
\mathbf{M} =
|\mu_s|
\left[
f_{iso}
+
f_{vol}K_{vol}
+
f_{geo}K_{geo}
\right]
\mathbf{I}
+
B_{pol}\mathbf{K}_{pol}
$$


Note: The original formulation includes an NDVI factor in $\mathbf{K}_{pol}$, which is absorbed into $ B_{pol} $.

For scalar radiative transfer simulations, only

$$
M_{11}
$$

is used.

---

## References

Wanner, W., Li, X., & Strahler, A. (1995)  
*On the derivation of kernels for kernel-driven BRDF models.*  
Journal of Geophysical Research.

MODIS Surface Reflectance ATBD  
https://modis.gsfc.nasa.gov/data/atbd/atbd_mod09.pdf