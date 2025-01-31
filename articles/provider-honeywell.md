---
author: Mobius5150
description: This document provides the technical details of the Honeywell quantum provider
ms.author: mblouin
ms.date: 07/26/2021
ms.service: azure-quantum
ms.subservice: computing
ms.topic: reference
title: Honeywell provider
uid: microsoft.quantum.providers.honeywell
---

# Honeywell provider

- Publisher: [Honeywell](https://www.honeywell.com)
- Provider ID: `honeywell`

## Targets

The following targets are available from this provider:

- [API Validator](#api-validator)
- [Honeywell System Model H1](#honeywell-system-model-h1)

### Target Availability

A target's status indicates its current ability to process jobs. The possible states of a target include:

- **Available**: The target is processing jobs at a normal rate.
- **Degraded**: The target is currently processing jobs at a slower rate than usual.
- **Unavailable**: The target currently does not process jobs.

Current status information may be retrieved from the *Providers* tab of a workspace on the [Azure Portal](https://portal.azure.com).

### API Validator

Tool to verify proper syntax and compilation completion. Full stack is exercised with the exception of the actual quantum operations. Assuming no bugs, all zeros are returned in the proper data structure.

- Job type: `Simulation`
- Data Format: `honeywell.openqasm.v1`
- Target ID: `honeywell.hqs-lt-1.0-apival`
- Target Execution Profile: [Basic Measurement Feedback](xref:microsoft.quantum.target-profiles)

Billing information:  No charge for usage.

### Honeywell System Model H1

Honeywell Quantum Solutions' Quantum Computer, System Model H1

- Job type: `Quantum Program`
- Data Format: `honeywell.openqasm.v1`
- Target ID: `honeywell.hqs-lt-s1`
- Target Execution Profile: [Basic Measurement Feedback](xref:microsoft.quantum.target-profiles)


Billing information:

- **Standard Subscription:**
Monthly subscription plan with 10K Honeywell quantum credits (HQCs) / month, available through queue.
- **Premium Subscription:**
Monthly subscription plan with 17K Honeywell quantum credits (HQCs) / month, available through queue.

The following equation defines how circuits are translated into Honeywell Quantum Credits (HQCs):

$$
HQC = 5 + C(N_{1q} + 10 N_{2q} + 5 N_m)/5000
$$

where:

- $N_{1q}$ is the number of one-qubit operations in a circuit.
- $N_{2q}$ is the number of native two-qubit operations in a circuit. Native gate is equivalent to CNOT up to several one-qubit gates.
- $N_{m}$ is the number of state preparation and measurement (SPAM) operations in a circuit including initial implicit state preparation and any intermediate and final measurements and state resets.
- $C$ is the shot count.

#### Technical Specifications

- Trapped-ion based quantum computer with laser based gates
- QCCD architecture with linear trap and three parallel operational zones
- 10 physical qubits, fully connected
- Typical limiting fidelity >99.5% (two-qubit fidelity)
- Coherence Time (T2) ~3 sec
- Ability to perform mid-circuit measurement and qubit reuse
- High-resolution rotations (> $\pi$/500)
- Native Gate set:
  - single-qubit rotations
  - two-qubit ZZ-gates

More details available under NDA.
