# Ultraslow Damage Pool in DNA Repair Kinetics

This repository contains the implementation of the **biologically motivated Model C+** that extends the Bi-Component Repair Model (BCRM) by introducing an *ultraslow* fraction of complex DNA double-strand breaks (DSBs).  
It reproduces γH2AX and pATM kinetics in human stem cells exposed to 0.5 Gy of γ-rays and 14.1 MeV neutrons.

---

### 📘 Model overview
**States:**  
`[D_s, D_c, D_u, pATM_f, pATM_s, γH2AX]`

- `D_s` — simple DSBs, first-order repair  
- `D_c` — complex DSBs, saturable repair (Michaelis–Menten)  
- `D_u` — ultraslow DSBs formed via migration from `D_c` (`k_cu`) and repaired very slowly (`k_u`)  
- `pATM_f`, `pATM_s` — fast and slow ATM activation phases  
- `γH2AX` — cumulative marker of DSB signaling  

Ultraslow damage contributes less to ATM activation:  
`w_du = s_du × w_dc`, where `s_du ∈ [0,1]`.

---

### ⚙️ Simulation
The model is implemented in Python 3 using:
```bash
numpy
scipy
matplotlib
