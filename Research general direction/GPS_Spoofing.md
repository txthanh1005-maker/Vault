---
contributors: [D.M.Hai K67]
---
*This node was implemented by the subagents `pure_entity_researcher` and `grid_interaction_researcher`.*

-> [[Outside_Attack]]

# GPS Spoofing

## Yin (Physics/Code/Vulnerability)
GPS Spoofing involves broadcasting counterfeit GPS signals that overpower genuine satellite signals, deceiving receivers into calculating false location or timing data. In the context of critical infrastructure like the power grid, the primary vulnerability lies in the reliance on GPS for precise timing synchronization (e.g., via Phasor Measurement Units or PMUs). Attackers use software-defined radios (SDRs) and signal generators to simulate realistic GPS satellite constellations. By subtly shifting time signals, they can introduce phase angle errors into the system without triggering immediate loss-of-signal alarms. The lack of cryptographic authentication in civilian GPS signals makes them highly susceptible to such deception.

## Yang (Grid Impact)
The impact of GPS Spoofing on the power grid can be severe, primarily affecting wide-area monitoring, protection, and control (WAMPAC) systems. PMUs rely on microsecond-level GPS timing to timestamp voltage and current phasors. If this timing is spoofed, calculated phase angles will be incorrect, leading to:
1.  **False State Estimation:** Energy Management Systems (EMS) may perceive a stable grid as unstable, or vice versa, leading to incorrect dispatch decisions.
2.  **Misoperation of Protection Relays:** Distance relays and differential protection schemes that rely on synchronized measurements across long transmission lines may trip falsely or fail to trip during an actual fault.
3.  **Loss of Islanding Detection:** Accurate timing is crucial for detecting when parts of the grid have become isolated. Spoofed timing can prevent safe management of microgrids.