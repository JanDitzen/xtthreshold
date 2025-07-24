# `xtthreshold` – Threshold Estimation for Interactive Fixed Effects Models

![version](https://img.shields.io/github/v/release/janditzen/xtthreshold)  ![release](https://img.shields.io/github/release-date/janditzen/xtthreshold) 

## Title

`xtthreshold` – Threshold estimation for interactive fixed effects models.

---

## Syntax

```stata
xtthreshold depvar indepvars1 | [indepvars2] [if] [in], threshold(thresvar) [csa(varlist1) grid(integer)]
```

- Data must be `tsset` or `xtset` before using `xtthreshold`.  
- Variables may include time-series operators (see `tsvarlist` help).  
- Requires the [`moremata`](https://stata.com) package.

**Variable Notes:**
- `thresvar`: thresholding variable.
- `indepvars1`: variables affected by the threshold.
- `indepvars2`: variables **not** affected by the threshold.
- `varlist1`: variables added as cross-section averages.

---

## Contents

- [Description](#description)
- [Options](#options)
- [Postestimation](#postestimation)
- [Examples](#examples)
- [References](#references)
- [About, Authors and Version History](#about-authors-and-version-history)

---

## Description

We consider a panel dataset over cross-sectional units *i* and time *t*, with three sets of independent variables:

- `x(i,t)`: unaffected by the threshold  
- `w(i,t)`: effects depend on whether `z(i,t)` is above or below threshold `v`  
- `z(i,t)`: the thresholding variable

The model is defined as:

```math
y(i,t) = b₁ x(i,t) + g₁ I(z(i,t) < v) w(i,t) + g₂ I(z(i,t) > v) w(i,t) + u(i,t)
```

The error term has an interactive fixed effect structure:

```math
u(i,t) = g(i) f(t) + ε(i,t)
```

Where:
- `g(i)`: factor loading  
- `f(t)`: common factor  
- `ε(i,t)`: iid noise

The estimator uses cross-sectional averages to span the factor space (see Ditzen & Karavias, 2025).

### Implementation

`xtthreshold` follows Ditzen et al. (2025) to:

1. Estimate the threshold `v` using a grid over `z(i,t)`
2. Estimate coefficients `b₁`, `g₁`, and `g₂`
3. Choose `v` minimizing the sum of squared residuals (SSR)

### Likelihood Ratio Test

```math
LR(v₀) = NT × [SSR(v₀) - SSR(v)] / SSR(v)
```

---

## Options

- `threshold(thresvar)`: defines the thresholding variable `Z`
- `csa(varlist1)`: adds variables as cross-section averages
- `grid(integer)`: length of the threshold grid (default: 100)

---

## Postestimation

You can use `estat` to explore and visualize results:

```stata
estat split         // Splits variables in indepvars1 at threshold
estat graph lr      // Graphs LR test across thresholds
```

---

## Examples

Download data from the [GitHub repository](https://github.com/JanDitzen/xtthreshold/tree/main/data).

### Estimate Threshold

```stata
xtthreshold gdppcgr govspend khanvar | open popgr grosscap, threshold(khanvar) grid(400) csa(govspend khanvar open popgr grosscap)
```

### Split Thresholded Variables

```stata
estat split
```

### Estimate Final Model (using `xtdcce2`)

```stata
xtdcce2 gdppcgr govspend_0 khanvar_0 govspend_1 khanvar_1 open popgr grosscap, cr(govspend khanvar open popgr grosscap)
```

### Plot Likelihood Ratio Test

```stata
estat graph lr
```

---

## References

- **Ditzen, J. & Karavias, Y. (2025)**  
  *Interactive, Grouped and Non-separable Fixed Effects: A Practitioner’s Guide to the New Panel Data Econometrics*  
  [Link](abc)

- **Ditzen, J., Karavias, Y. & Westerlund, J. (2025)**  
  *Threshold Regression in Interactive Fixed Effects Panel Data and the Impact of Inflation on Economic Growth*  
  [Link](abc)

---

## About, Authors and Version History

**Jan Ditzen**  
Free University of Bozen-Bolzano  
📧 [jan.ditzen@unibz.it](mailto:jan.ditzen@unibz.it)  
🌐 [www.jan.ditzen.net](https://www.jan.ditzen.net)

**Yiannis Karavias**  
Brunel University  
📧 [yiannis.karavias@brunel.ac.uk](mailto:yiannis.karavias@brunel.ac.uk)  
🌐 [Google Scholar](https://sites.google.com/site/yianniskaravias/)

**Joakim Westerlund**  
Lund University  
📧 [joakim.westerlund@nek.lu.se](mailto:joakim.westerlund@nek.lu.se)  
🌐 [Google Scholar](https://sites.google.com/site/perjoakimwesterlund/)

---

## Installation

Run the following command in Stata to install:

```stata
net from https://github.com/JanDitzen/xtthreshold
```

---

## Notes

- Requires Stata version 15 or higher.

---

## Changelog

**Version 0.1 – 24.07.2025**
