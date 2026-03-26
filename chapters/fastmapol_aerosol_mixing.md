# Aerosol characterization: mixing with size, mode and shapes

## Overview

This chapter describes the computation of aerosol bulk scattering properties using:

- Lookup-table (LUT)–based single-particle scattering  
- Lognormal **volume size distributions**  
- Multi-modal (fine/coarse) representation  
- Linear mixing of **spherical and non-spherical particles**  

All formulations strictly follow the implementation, including:

- Volume-based integration  
- Internal normalization strategy  
- Use of **unnormalized phase matrices** during accumulation  

---

## Single-Particle Optical Properties

For each particle size parameter \( x \), refractive index \( m = m_r + i m_i \), and scattering angle \( \theta \), the LUT provides:

- Mueller matrix:
  $$
  \mathbf{M}_1(x, \theta)
  $$
- Scattering efficiency:
  $$
  Q_{\text{sca}}(x)
  $$
- Extinction efficiency:
  $$
  Q_{\text{ext}}(x)
  $$

These quantities are:

- Defined on discrete grids of \( (m_r, m_i, x) \)  
- Interpolated to target refractive indices  
- **Not scaled by number density or volume**  

---

## Size Distribution and Volume-Based Weighting

Aerosol size distributions are represented using **lognormal volume distributions**:

$$
\frac{dV}{d\ln r} = V_0 \cdot f(r; r_v, \sigma)
$$

The implementation converts this to scattering-weighted quantities using:

$$
C_{\text{sca}}^{\text{vol}}(r)
=
Q_{\text{sca}}(r)\,\pi r^2 \cdot \frac{3}{4\pi r^3}
$$

$$
C_{\text{ext}}^{\text{vol}}(r)
=
Q_{\text{ext}}(r)\,\pi r^2 \cdot \frac{3}{4\pi r^3}
$$

### Phase matrix (unnormalized):

$$
\mathbf{M}_2(\theta)
=
\int \mathbf{M}_1(r,\theta)\,
C_{\text{sca}}^{\text{vol}}(r)\,
\frac{dV}{d\ln r}\, d\ln r
$$

### Cross sections:

$$
C_{\text{sca},2}
=
\int C_{\text{sca}}^{\text{vol}}(r)\,
\frac{dV}{d\ln r}\, d\ln r
$$

$$
C_{\text{ext},2}
=
\int C_{\text{ext}}^{\text{vol}}(r)\,
\frac{dV}{d\ln r}\, d\ln r
$$

---

## Internal Normalization Strategy

After size integration, the implementation performs an intermediate normalization:

$$
\mathbf{P}_k(\theta) = \frac{\mathbf{M}_{2,k}(\theta)}{C_{\text{sca},2,k}}
$$

$$
\tilde{C}_{\text{sca},k} = \frac{C_{\text{sca},2,k}}{\text{norm}_k}
$$

$$
\tilde{C}_{\text{ext},k} = \frac{C_{\text{ext},2,k}}{\text{norm}_k}
$$

where:

- $\text{norm}_k$ is the integrated volume normalization factor  
- $\mathbf{P}_k$ is a normalized phase matrix per mode  

---

## Mode Aggregation (Exact Implementation Form)

For each mode \( k \), the number density is reconstructed from volume:

$$
n_k
=
V_k \cdot \frac{3}{4\pi r_k^3} \exp(4.5\sigma_k^2)
$$

The bulk phase matrix is accumulated as:

$$
\mathbf{M}_3(\theta)
=
\sum_k
\mathbf{P}_k(\theta)\,
\tilde{C}_{\text{sca},k}\,
n_k
$$

Similarly:

$$
C_{\text{sca},3} = \sum_k \tilde{C}_{\text{sca},k} \, n_k
$$

$$
C_{\text{ext},3} = \sum_k \tilde{C}_{\text{ext},k} \, n_k
$$

### Interpretation

$$
\mathbf{M}_3(\theta)
\sim C_{\text{sca}} \cdot n \cdot P(\theta)
$$

This quantity is **not normalized**.

---

## Fine and Coarse Mode Separation

Modes are partitioned into:

- Fine mode:
  $$
  k = 1, \dots, k_f
  $$
- Coarse mode:
  $$
  k = k_f+1, \dots, N
  $$

Each produces:

- $(\mathbf{M}_{3,f}, C_{\text{sca},f}, C_{\text{ext},f}, n_f)$
- $(\mathbf{M}_{3,c}, C_{\text{sca},c}, C_{\text{ext},c}, n_c)$

---

## Shape-Resolved Computation

The full computation is performed separately for:

- **Spherical particles** → \( (s) \)  
- **Non-spherical particles** → \( (n) \)  

Important:

- The **same size distribution** is used for both shapes  
- Only scattering properties differ  

---

## Shape Mixing (Implementation-Exact Form)

### Cross Sections

Fine mode:

$$
C_{\text{sca},f}
=
f_{\text{sph},f}\, C_{\text{sca},f}^{(s)}
+
(1-f_{\text{sph},f})\, C_{\text{sca},f}^{(n)}
$$

$$
C_{\text{ext},f}
=
f_{\text{sph},f}\, C_{\text{ext},f}^{(s)}
+
(1-f_{\text{sph},f})\, C_{\text{ext},f}^{(n)}
$$

Similarly for coarse mode.

---

### Phase Matrix (Critical Detail)

$$
\mathbf{M}_{3,f}
=
f_{\text{sph},f}\, \mathbf{M}_{3,f}^{(s)}
+
(1-f_{\text{sph},f})\, \mathbf{M}_{3,f}^{(n)}
$$

$$
\mathbf{M}_{3,c}
=
f_{\text{sph},c}\, \mathbf{M}_{3,c}^{(s)}
+
(1-f_{\text{sph},c})\, \mathbf{M}_{3,c}^{(n)}
$$

Total:

$$
\mathbf{M}_3 = \mathbf{M}_{3,f} + \mathbf{M}_{3,c}
$$

### Important Clarification

Each component already includes:

$$
\mathbf{M}_3 \sim C_{\text{sca}} \cdot n \cdot P(\theta)
$$

Thus mixing is:

$$
\mathbf{M}
=
f (C_{\text{sca}} n P)_s
+
(1-f)(C_{\text{sca}} n P)_n
$$

**Not:**

$$
P = f P_s + (1-f)P_n
$$

---

## AOD-Based Scaling

To enforce a prescribed aerosol optical depth \( \tau_{\text{ref}} \):

$$
\mathbf{V}' = \frac{\tau_{\text{ref}}}{C_{\text{ext}}(\lambda_{\text{ref}})} \cdot \mathbf{V}
$$

This scaling propagates to:

- Number density  
- Cross sections  
- Phase matrix  

---

## Final Output Quantities

For each wavelength:

- $\mathbf{M}_3(\theta)$: unnormalized phase matrix  
- $C_{\text{sca}}$, $C_{\text{ext}}$  
- Fine and coarse components  

Normalized phase function:

$$
P(\theta) = \frac{\mathbf{M}_3(\theta)}{C_{\text{sca}}}
$$

---

## Full Computational Flow

1. Interpolate LUT: $\mathbf{M}_1(x)$  
2. Convert size: $r = \frac{x\lambda}{2\pi}$  
3. Size integration → $\mathbf{M}_2$  
4. Normalize per mode → $\mathbf{P}_k$  
5. Mode aggregation → $\mathbf{M}_3$  
6. Split fine / coarse  
7. Compute sphere and non-sphere separately  
8. Shape mixing  
9. AOD scaling  
10. Repeat for all wavelengths  

---

## Key Implementation Characteristics

- Volume-based integration  
- Two-stage normalization  
- Phase matrix stored unnormalized  
- Shape mixing after full integration  
- Fine/coarse handled independently  