# Ultra-Slow Component Model

**Author:** Sofia Korneva  


---

## Overview
This repository presents the **Ultra-Slow Component Model (USC-Model)** — an ODE-based quantitative model describing DNA double-strand break (DSB) repair kinetics and ATM/γH2AX signaling in human stem cells.

The model extends the baseline **Bi-Component Repair Model (BCRM)** by introducing an **ultraslow fraction** of complex DNA damage that reproduces the persistent γH2AX / pATM “tail” observed at 24 h.

-

## Biological interpretation

The ultraslow pool corresponds to damage that persists for tens of hours:

Clustered/dirty-end DSBs requiring long enzymatic processing (PNKP, Artemis).

Heterochromatin-associated breaks (H3K9me3 / HP1α) repairing via delayed HR.

Stable ATM microdomains around unrepaired DSBs or telomeres.

The Model provides a mechanistic—not phenomenological—explanation of long-lived DNA damage signaling.

## Running the model without the ultraslow component

The model can also be evaluated in a reduced form that includes only the fast-repair and complex-damage compartments. To suppress the ultraslow damage compartment (D_u), fix the following parameters at zero during parameter estimation:

```python
k_cu = 0.0
pi_g = 0.0
pi_n = 0.0
```

Here, `pi_g` and `pi_n` control the initial fraction of ultraslow damage after gamma-ray and neutron irradiation, respectively, whereas `k_cu` controls the transfer of complex damage into the ultraslow compartment during the simulation.

When these parameters are fixed at zero, no damage is initialized in or transferred to (D_u), and the model describes repair using only the fast-repair and complex-damage compartments.

These parameters must be fixed at zero during fitting, rather than specified only as initial guesses, because they are optimized variables in the full implementation.

