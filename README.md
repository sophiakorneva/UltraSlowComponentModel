# Ultra-Slow Component Model (USC-Model, Model C⁺)

**Author:** Sofia Korneva  
**Affiliation:** Federal Medical Biophysical Center (FMBA of Russia) & Lomonosov Moscow State University  

---

## Overview
This repository presents the **Ultra-Slow Component Model (USC-Model)** — an ODE-based quantitative model describing DNA double-strand break (DSB) repair kinetics and ATM/γH2AX signaling in human stem cells after **0.5 Gy γ-ray** and **14.1 MeV neutron** exposure.

The model extends the baseline **Bi-Component Repair Model (BCRM)** by introducing an **ultraslow fraction** of complex DNA damage that reproduces the persistent γH2AX / pATM “tail” observed at 24 h.

---

## Model structure

**State variables:**  
`[D_s, D_c, D_u, pATM_f, pATM_s, γH2AX]`

| Symbol | Meaning | Typical value / notes |
|---------|----------|-----------------------|
| D_s | Simple DSBs (fast repair) | decays ≈ 1 h⁻¹ |
| D_c | Complex DSBs (saturable repair, V_rc / K_rc) | MM-type |
| D_u | Ultraslow DSBs ← migration D_c → D_u (k_cu), very slow repair (k_u) | t₁/₂ ≈ 30 h |
| pATM_f / pATM_s | Fast / slow ATM phases | biphasic response |
| γH2AX | Phosphorylated H2AX signal | proxy for DSBs |

Ultraslow breaks activate ATM more weakly:  
`w_du = s_du × w_dc`, where `s_du ∈ [0, 1]`.  
At *t = 0* a small portion of complex breaks is already ultraslow (πγ for γ, πₙ for n).

---

## Differential equations
```python
dD_s = -k_rs * D_s
dD_c = -(V_rc * D_c)/(K_rc + D_c) - k_cu * D_c
dD_u = -k_u * D_u + k_cu * D_c
dpATM_f = (V_af * weighted)/(K_m + weighted) - k_df * pATM_f
dpATM_s = k_as * pATM_f - k_ds * pATM_s
dgH2AX  = k_p * (pATM_f + pATM_s) - k_g * gH2AX

where weighted = D_s + w_dc·D_c + s_du·w_dc·D_u.

Numerical integration: solve_ivp(method="LSODA").

## Fit results (0.5 Gy γ / 14.1 MeV n)

| Metric     | Value                       |
| ---------- | --------------------------- |
| χ²         | **7.13**                    |
| AIC        | **45.13**                   |
| Parameters | 19                          |
| πγ         | **0.07**                    |
| πₙ         | **0.20**                    |
| k_u        | **0.022 h⁻¹** → t₁/₂ ≈ 32 h |
| k_cu       | **0.033 h⁻¹**               |
| s_du       | **0.34**                    |

Residuals (z-score):

γ gH2AX = [ −0.39, 0.45, 0.06, 0.24, −0.44 ]

γ pATM = [ −0.45, −0.79, −0.46, 0.25, 1.26 ]

n gH2AX = [ 1.03, −0.06, −0.94, 0.86, −0.46 ]

n pATM = [ 0.23, −0.42, 0.05, 0.64, 0.54 ]

Residuals within ≈ ±1 σ → good agreement with experiment.

## Biological interpretation

The ultraslow pool corresponds to damage that persists for tens of hours:

Clustered / dirty-end DSBs requiring long enzymatic processing (PNKP, Artemis).

Heterochromatin-associated breaks (H3K9me3 / HP1α) repairing via delayed HR.

Stable ATM microdomains around unrepaired DSBs or telomeres.

Thus Model C⁺ provides a mechanistic—not phenomenological—explanation of long-lived DNA damage signaling.
