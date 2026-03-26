# Aerosol characterization: Two-way conversions between submode volume density and bulk effective size

In the aerosol model adopted here, the retrieved state vector contains the volume densities of the fixed log-normal submodes. From these submode volume densities, one can compute bulk properties such as total fine-mode volume density, total coarse-mode volume density, effective radius, and effective variance for the fine and coarse aerosol populations.

The same relationship can also be used in the reverse direction. If another algorithm provides bulk aerosol quantities such as fine-mode and coarse-mode volume density, effective radius, and effective variance, then these bulk quantities can be used to approximately recover the corresponding submode volume densities under the assumed fixed log-normal basis. This provides a practical way to compare external aerosol products with the FastMAPOL parameterization.

The theory below summarizes this inversion.

## Fixed log-normal basis

Assume each aerosol submode is represented by a fixed log-normal **volume** distribution with prescribed volume-mean radius $r_{v,i}$ and logarithmic standard deviation $\sigma_i$. The only unknown for each submode is its column volume density $V_i$.

For the five-mode basis used here (Aer-1), the fixed parameters are

$$
r_v = [0.1,\ 0.1732,\ 0.3,\ 1.0,\ 2.9] \ \mu m
$$

and

$$
\sigma = [0.35,\ 0.35,\ 0.35,\ 0.5,\ 0.5].
$$

The first three modes represent the fine aerosol, and the last two modes represent the coarse aerosol.

## Moments for a log-normal volume distribution

For a single log-normal volume mode, define the moment

$$
m'_k = \int r^k \frac{dV(r)}{d\ln r} d\ln r.
$$

For the assumed log-normal form, this moment can be written analytically as

$$
m'_{k,i} = V_i \, r_{v,i}^k \exp\left(\frac{k^2 \sigma_i^2}{2}\right).
$$

For convenience, define

$$
M_k(r_{v,i},\sigma_i)=r_{v,i}^k \exp\left(\frac{k^2 \sigma_i^2}{2}\right).
$$

Then the moment of mode $i$ is simply

$$
m'_{k,i}=V_i M_k(r_{v,i},\sigma_i).
$$

## Bulk fine- and coarse-mode constraints

For a group of modes, the bulk volume density is the sum of the component volumes:

$$
V_d = \sum_i V_i.
$$

Using the volume-distribution moments, the effective radius is

$$
r_{eff}=\frac{\sum_i m'_{0,i}}{\sum_i m'_{-1,i}}
       =\frac{\sum_i V_i}{\sum_i V_i M_{-1}(r_{v,i},\sigma_i)}.
$$

Therefore,

$$
\sum_i V_i M_{-1}(r_{v,i},\sigma_i)=\frac{V_d}{r_{eff}}.
$$

Similarly, the effective variance is

$$
\nu_{eff}=\frac{\left(\sum_i m'_{1,i}\right)\left(\sum_i m'_{-1,i}\right)}
{\left(\sum_i m'_{0,i}\right)^2}-1.
$$

Using the previous relations,

$$
\nu_{eff}+1
=\frac{\left(\sum_i V_i M_1(r_{v,i},\sigma_i)\right)\left(V_d/r_{eff}\right)}{V_d^2},
$$

which gives

$$
\sum_i V_i M_1(r_{v,i},\sigma_i)
=(1+\nu_{eff})V_d r_{eff}.
$$

These three equations form the basis for the inversion from bulk effective properties back to submode volume densities.

## Fine-mode inversion

The fine aerosol is represented by three fixed submodes, with unknown volumes $V_1$, $V_2$, and $V_3$.

Because there are three unknowns, three independent bulk constraints are required:

1. total fine-mode volume density
2. fine-mode effective radius
3. fine-mode effective variance

Let

$$
A_i = M_{-1}(r_{v,i},\sigma_i)
$$

and

$$
B_i = M_{1}(r_{v,i},\sigma_i),
$$

for the three fine modes.

Then the fine-mode inversion is determined by the linear system

$$
V_1 + V_2 + V_3 = V_{d,f},
$$

$$
A_1V_1 + A_2V_2 + A_3V_3 = \frac{V_{d,f}}{r_{eff,f}},
$$

$$
B_1V_1 + B_2V_2 + B_3V_3 = (1+\nu_{eff,f})V_{d,f}r_{eff,f}.
$$

In matrix form,

$$
\begin{bmatrix}
1 & 1 & 1 \\
A_1 & A_2 & A_3 \\
B_1 & B_2 & B_3
\end{bmatrix}
\begin{bmatrix}
V_1 \\ V_2 \\ V_3
\end{bmatrix}
=
\begin{bmatrix}
V_{d,f} \\
V_{d,f}/r_{eff,f} \\
(1+\nu_{eff,f})V_{d,f}r_{eff,f}
\end{bmatrix}.
$$

## Coarse-mode inversion

The coarse aerosol is represented by two fixed submodes, with unknown volumes $V_4$ and $V_5$.

Because there are only two unknowns, only two independent constraints are needed:

1. total coarse-mode volume density
2. coarse-mode effective radius

Let

$$
A_i = M_{-1}(r_{v,i},\sigma_i)
$$

for the two coarse modes.

Then the coarse-mode inversion is determined by

$$
V_4 + V_5 = V_{d,c},
$$

$$
A_4V_4 + A_5V_5 = \frac{V_{d,c}}{r_{eff,c}}.
$$

In matrix form,

$$
\begin{bmatrix}
1 & 1 \\
A_4 & A_5
\end{bmatrix}
\begin{bmatrix}
V_4 \\ V_5
\end{bmatrix}
=
\begin{bmatrix}
V_{d,c} \\
V_{d,c}/r_{eff,c}
\end{bmatrix}.
$$

Since there are only two coarse submodes, the effective variance is not needed as an independent input. In this basis, once $V_{d,c}$ and $r_{eff,c}$ are specified, the pair $(V_4,V_5)$ is uniquely determined. The resulting coarse-mode effective variance is then implied by the recovered submode mixture.

## Why the inversion is linear

A useful feature of this formulation is that the inversion is linear in the unknown submode volume densities.

This is possible because:

- the mode radii $r_{v,i}$ are fixed,
- the logarithmic widths $\sigma_i$ are fixed,
- the bulk quantities can be written as linear combinations of $V_i$ after rearranging the effective-radius and effective-variance formulas.

Therefore, recovering the submode volumes reduces to solving a small linear system.

## Interpretation

This inversion should be interpreted as a projection of an external bulk aerosol product onto the fixed FastMAPOL submode basis.

That is, if another algorithm reports:

- fine-mode volume density $V_{d,f}$
- coarse-mode volume density $V_{d,c}$
- fine-mode effective radius $r_{eff,f}$
- coarse-mode effective radius $r_{eff,c}$
- fine-mode effective variance $\nu_{eff,f}$
- optionally coarse-mode effective variance $\nu_{eff,c}$

then one may recover an approximate set of five submode volume densities

$$
[V_1,V_2,V_3,V_4,V_5]
$$

that is consistent with the FastMAPOL parameterization.

This recovered vector is not necessarily the unique physical aerosol decomposition of the original product. Rather, it is the unique decomposition **within the chosen fixed log-normal basis**.



## Extension to six-mode models

The same approach extends naturally to a six-mode basis.

If the fine and coarse partitions still have fixed mode radii and widths, then the inversion remains a linear system, but the number of independent bulk constraints must match the number of unknown submode volumes in each partition.

For example:

- three fine modes require three bulk constraints,
- two coarse modes require two constraints,
- three coarse modes would require three independent coarse-mode constraints.

In general, the inversion is possible whenever the number of bulk moments provided is sufficient to determine the unknown modal amplitudes.

## Summary

The submode volume densities and the bulk effective properties are related in both directions within a fixed log-normal aerosol basis.

Forward direction:
- submode volume densities $\rightarrow$ bulk volume density, effective radius, and effective variance

Inverse direction:
- bulk volume density, effective radius, and effective variance $\rightarrow$ approximate submode volume densities

Because the modal radii and widths are fixed, the inversion becomes a small linear algebra problem. This makes it straightforward to map bulk aerosol products from other algorithms onto the FastMAPOL five-mode or six-mode parameterization and enables direct product intercomparison at the submode level.

## [Optional] Code test
moment: `mkv(r, sigma, k)`
Compute coarse mode volume density: `invert_coarse`
Compute fine mode volume density: `invert_fine`
Pixel by pixel conversion: `update_vdv(datax)

Using the prescribed mode radii and widths,

$$
r_v = [0.1,\ 0.1732,\ 0.3,\ 1.0,\ 2.9]
$$

and

$$
\sigma = [0.35,\ 0.35,\ 0.35,\ 0.5,\ 0.5],
$$

the code:

1. reads bulk fine- and coarse-mode quantities from the dataset,
2. solves the 3-by-3 linear system for the fine-mode subvolumes,
3. solves the 2-by-2 linear system for the coarse-mode subvolumes,
4. concatenates the recovered values into five reconstructed submode volume densities,
5. stores them as new variables:
   - `vd_mode1_new`
   - `vd_mode2_new`
   - `vd_mode3_new`
   - `vd_mode4_new`
   - `vd_mode5_new`

Thus the output is a recovered five-mode volume-density representation that can be directly compared with the original FastMAPOL state vector or with any other product expressed in the same basis.