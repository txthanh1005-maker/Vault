---
contributors: [D.M.Hai K67]
---
*This node was implemented by the subagents `pure_entity_researcher` and `grid_interaction_researcher`.*

-> [[Outside_Attack]]

# Ransomware Attack

## Yin (Physics/Code/Vulnerability)
A Ransomware Attack is malicious software designed to block access to a system or data, usually by encrypting it, until a sum of money is paid. In power systems, vulnerabilities often stem from the convergence of Information Technology (IT) and Operational Technology (OT) networks. Attack vectors typically include phishing emails, exploiting unpatched vulnerabilities in IT systems, or compromised third-party credentials. Once inside the IT network, attackers attempt to pivot into the OT environment (e.g., SCADA systems). The code utilizes strong encryption algorithms to lock critical files, databases, or operating systems of human-machine interfaces (HMIs) and engineering workstations.

## Yang (Grid Impact)
The impact of ransomware on the power grid depends heavily on how deeply it penetrates the OT network.
1.  **Loss of Visibility and Control:** If SCADA servers or HMIs are encrypted, grid operators lose the ability to monitor real-time grid conditions and remotely control breakers and switches. This "blindness" drastically increases the risk of instability during a disturbance.
2.  **Forced Manual Operations:** Utilities may have to revert to manual, localized control of substations, which is significantly slower and less coordinated.
3.  **Billing and Customer Service Disruption:** Even if the physical grid remains stable, ransomware hitting the IT side can paralyze customer billing, outage reporting systems, and communication channels, causing massive operational and reputational damage.