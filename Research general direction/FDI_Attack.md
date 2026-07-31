---
contributors: [D.M.Hai K67]
---
*Implemented by pure_entity_researcher and grid_interaction_researcher*

-> [[Inside_Attack]]

# False Data Injection (FDI) Attack

## Yin (Physics/Code/Vulnerability)
False Data Injection (FDI) attacks involve maliciously altering sensor readings or control signals within the power grid's cyber-physical system. In smart grids, attackers exploit vulnerabilities in SCADA systems, PMUs (Phasor Measurement Units), or communication networks (e.g., DNP3, IEC 61850). By bypassing bad data detection (BDD) mechanisms, attackers can inject carefully crafted data vectors that align with the system's topology, making the compromised data appear legitimate. The vulnerability lies in the unencrypted or poorly authenticated communication channels and the reliance on state estimation algorithms that can be mathematically bypassed.

## Yang (Grid Impact)
The impact of FDI attacks on the grid can be catastrophic. By tricking the state estimator into believing the system is in a different state (e.g., altering load measurements or voltage magnitudes), operators or automated control systems may take incorrect actions. This can lead to uneconomic dispatch, improper relay settings, or unnecessary load shedding. In severe cases, it can trigger cascading failures, equipment damage (such as generator tripping), and widespread blackouts, directly compromising grid stability and reliability.