---
contributors: [D.M.Hai K67]
---
*Implemented by pure_entity_researcher and grid_interaction_researcher*

-> [[Inside_Attack]]

# Denial of Service (DoS) Attack

## Yin (Physics/Code/Vulnerability)
Denial of Service (DoS) and Distributed Denial of Service (DDoS) attacks target the availability of communication networks and control centers in the power grid. Attackers flood the network with overwhelming amounts of traffic, exhausting bandwidth, processing power, or memory resources of critical devices like RTUs (Remote Terminal Units), PMUs, or control servers. Vulnerabilities include inadequate rate limiting, lack of network segmentation, and insufficient resources to handle traffic spikes. This disrupts the real-time flow of critical operational data.

## Yang (Grid Impact)
The disruption of communication caused by DoS attacks directly impacts the grid's observability and controllability. When operators lose real-time visibility (loss of telemetry) and the ability to send control commands, the grid operates "blind." This delay or failure in communication can prevent timely responses to natural disturbances or faults, potentially causing localized outages to spread. While a DoS attack alone might not directly damage physical equipment, the resulting inability to balance generation and load or clear faults can lead to frequency instability, voltage collapse, and eventual cascading blackouts.