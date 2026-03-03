# MA_LLM_Dynamic_Clustering_VVC

This repository provides supplementary materials and reproducible resources for the paper:

**"Multi-Agent LLM–Guided Dynamic Clustering for Decentralized Volt/VAR Control"**

**Authors:**  
**Rouzbeh Haghighi**, Graduate Student Member IEEE, **Hualong Liu**, Member IEEE, **Van-Hai Bui**, Senior Member IEEE, **Wencong Su**, Senior Member IEEE

**Note:** This paper has been submitted to **IEEE Transactions on Smart Grid**.

**Paper link:** (to be added)

## Description

This repository includes mathematical formulations, tables, and figures discussed in the paper. It aims to provide additional resources and data to support the research findings and facilitate further studies.

## Contents

### DER Generation Models

This study adopts standard steady-state WT and PV generation models to represent time-varying active-power injections from inverter-interfaced DERs. Environmental inputs are mapped to active-power outputs that serve as exogenous injections in the power-flow model and the subsequent VVC optimization.

$$
P_{\mathrm{WT}}(v)=
\begin{cases}
0, & v < v_{\mathrm{ci}} \text{ or } v \ge v_{\mathrm{co}}, \\
C_1 v^3 - C_2 P_{\mathrm{WT}}^{\mathrm{r}}, & v_{\mathrm{ci}} \le v \le v_{\mathrm{r}}, \\
P_{\mathrm{WT}}^{\mathrm{r}}, & v_{\mathrm{r}} \le v \le v_{\mathrm{co}}
\end{cases}
$$

$$
C_1=\frac{P_{\mathrm{WT}}^{\mathrm{r}}}{v_{\mathrm{r}}^{3}-v_{\mathrm{ci}}^{3}},
\qquad
C_2=\frac{v_{\mathrm{ci}}^{3}}{v_{\mathrm{r}}^{3}-v_{\mathrm{ci}}^{3}}
$$

where $P_{\mathrm{WT}}^{\mathrm{r}}$ is the rated WT active power (kW); $v$ is the wind speed (m/s); and $v_{\mathrm{ci}}$, $v_{\mathrm{r}}$, and $v_{\mathrm{co}}$ denote the cut-in, rated, and cut-out wind speeds, respectively.

For PV units, the cell temperature is approximated using the NOCT model, and the PV active power $P_{\mathrm{PV}}$ is computed using an irradiance- and temperature-corrected linear model:

$$
T_{\mathrm{c}} = T_{\mathrm{a}} + \frac{G_{\mathrm{irr}}(\mathrm{NOCT}-20)}{800}
$$

$$
P_{\mathrm{PV}}(G_{\mathrm{irr}},T_{\mathrm{c}})=
P_{\mathrm{PV}}^{\mathrm{r}}
\left(\frac{G_{\mathrm{irr}}}{G_{\mathrm{ref}}}\right)
\left[1+\eta_T(T_{\mathrm{c}}-T_{\mathrm{ref}})\right]
$$

where $P_{\mathrm{PV}}^{\mathrm{r}}$ is the rated PV active power (kW); $G_{\mathrm{irr}}$ is the solar irradiance (W/m²); $T_{\mathrm{a}}$ is the ambient temperature (°C); $\eta_T$ is the PV temperature coefficient (1/°C); $G_{\mathrm{ref}}$ is the reference irradiance; and $T_{\mathrm{ref}}$ is the reference cell temperature.

### Inverter constraints and LinDistFlow-based VVC

Detailed formulations of the inverter capability constraints, operational power-factor limits, and the LinDistFlow-based VVC model are provided in this repository (cited in the paper as the companion repository).

**Inverter capability (apparent-power limit).** For each inverter at bus $i$, active and reactive outputs $P_i$, $Q_i$ are limited by the rated apparent power $\overline{S}_i$:

$$
P_i^2 + Q_i^2 \le \overline{S}_i^2, \qquad \forall i \in \mathcal{N}_{\mathrm{inv}}.
$$

**Operational power-factor limits.** Reactive power is constrained by the allowed power-factor range (e.g. $\underline{\mathrm{pf}} \le \mathrm{pf}_i \le \overline{\mathrm{pf}}$). With $\phi_i$ the power-factor angle at bus $i$, this is equivalent to bounds on $Q_i$ given $P_i$:

$$
Q_i \ge -P_i \tan\overline{\phi}, \qquad Q_i \le P_i \tan\overline{\phi}, \qquad \forall i \in \mathcal{N}_{\mathrm{inv}},
$$

where $\overline{\phi}$ is the maximum allowed angle (e.g. corresponding to a minimum power factor).

**LinDistFlow-based VVC.** Using the linearized distribution flow, voltage magnitude at bus $i$ is

$$
V_i = V_0 - \frac{1}{V_0} \sum_{l \in \mathcal{P}_i} \big( r_l P_l + x_l Q_l \big), \qquad \forall i \in \mathcal{N},
$$

where $V_0$ is the slack voltage; $\mathcal{P}_i$ is the set of lines on the path from the slack to bus $i$; $r_l$, $x_l$ are resistance and reactance of line $l$; and $P_l$, $Q_l$ are active and reactive power flow on line $l$. The VVC problem minimizes voltage deviation subject to voltage limits and the above constraints:

$$
\min \sum_{i \in \mathcal{N}} (V_i - V^{\mathrm{ref}})^2 \quad \text{s.t.} \quad \underline{V} \le V_i \le \overline{V}, \quad \forall i \in \mathcal{N},
$$

plus the inverter capability and power-factor constraints, with $V^{\mathrm{ref}}$ the reference voltage and $\underline{V}$, $\overline{V}$ the allowed voltage bounds. $\mathcal{N}$ denotes the set of buses and $\mathcal{N}_{\mathrm{inv}}$ the set of buses with inverters.

### Tables

1. **parameters**:


### Figures
1. **fe**:


## System requirements

- **Hardware:** Intel Core i7-4790 CPU (3.60 GHz), 16 GB RAM (or equivalent)
- **Software:** Python 3.x, Spyder IDE
- **Optimization:** CVXPY (LinDistFlow-based VVC formulation), ECOS solver
- **LLM:** GPT model via the OpenAI API
- **Other:** NumPy, Pandas, Matplotlib

## Simulation environment

All simulations were conducted on an Intel Core i7-4790 CPU (3.60 GHz) with 16 GB RAM using Python in the Spyder environment. The LinDistFlow-based VVC problems were formulated in CVXPY and solved using the ECOS solver. The LLM component used the GPT model accessed via the OpenAI API. Network and operating descriptors were converted into structured text prompts using the template provided in this repository (see Fig. 3 in the paper) to ensure consistent formatting and tokenization.

