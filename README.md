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
