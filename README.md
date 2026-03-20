<div align="center">
  <h1>🛡️ Guidewire CodeZap</h1>
  <p><strong>Adversarial Defense & Anti-Spoofing Strategy</strong></p>

  [![Status](https://img.shields.io/badge/Status-Active-success.svg)](#) 
  [![Security](https://img.shields.io/badge/Security-Advanced-blue.svg)](#)
  [![Platform](https://img.shields.io/badge/Platform-Delivery-orange.svg)](#)
  [![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](#)
</div>

<hr/>

## Table of Contents
- [Executive Summary](#executive-summary)
- [Threat Model Summary](#threat-model-summary)
- [Adversarial Defense Core Layers](#adversarial-defense-core-layers)
  - [1. Three-Point Verification](#1-three-point-verification)
  - [2. Dual GPS Stream Verification](#2-dual-gps-stream-verification)
  - [3. Self-Powered Tracker](#3-self-powered-tracker-using-piezoelectric-energy-harvesting)
  - [4. Financial Circuit Breakers](#4-financial-circuit-breakers-on-the-payout-side)
  - [5. Network-Level Fraud Detection](#5-network-level-fraud-ring-detection)
  - [6. Physics-Based Detection & Fuel Corroboration](#6-physics-based-fraud-detection-including-fuel-allowance-corroboration)
- [Protecting Honest Workers — Fairness by Design](#protecting-honest-workers--fairness-by-design)
- [Beyond the Prototype — Why This Is Infrastructure, Not a Feature](#beyond-the-prototype)

---

## Executive Summary <a name="executive-summary"></a>
Our platform implements a multi-layered defense strategy designed to prevent fraud, location spoofing, and platform manipulation. The defense relies on hardware consistency, network analysis, strict financial circuit breakers, and physics-based logic to safeguard delivery operations.

---

## Threat Model Summary <a name="threat-model-summary"></a>

| Attack Type / Threat | Correlated Defense Layer | Response & Mitigation |
| :--- | :--- | :--- |
| **Partner / Customer Collusion** | [Three-Point Verification](#1-three-point-verification) | Requires physical multi-party authentication (Vendor, Partner, Customer) spanning the delivery timeline. |
| **GPS Spoofing (Mock Locations)** | [Dual GPS Stream Corroboration](#2-dual-gps-stream-verification) | Automatically detects discrepancies between the partner's phone and the independent container tracker. |
| **Hardware Tampering / Disabling** | [Self-Powered Piezoelectric Tracker](#3-self-powered-tracker-using-piezoelectric-energy-harvesting) | Disabling the tracker physically locks the partner out of receiving new orders until legitimately resolved. |
| **Liquidity & Payout Drain** | [Financial Circuit Breakers](#4-financial-circuit-breakers-on-the-payout-side) | Halts or delays suspicious earnings spikes and abnormally large payouts for manual review. |
| **Coordinated Fraud Rings** | [Network-Level Detection](#5-network-level-fraud-ring-detection) | Flags shared IP arrays, device fingerprints, and synchronized mass account activity. |
| **Phantom / Speed Deliveries** | [Physics-Based Detection & Fuel Defense](#6-physics-based-fraud-detection-including-fuel-allowance-corroboration) | Validates timestamps, distance limits, and fuel claims against physical reality and traffic corridors. |

---

## Adversarial Defense Core Layers <a name="adversarial-defense-core-layers"></a>

### 1. Three-Point Verification <a name="1-three-point-verification"></a>
Instead of relying only on the delivery partner's GPS confirmation, the system creates a three-sided verification chain:
- **Vendor Confirmation:** The vendor confirms that a real order was prepared and dispatched from their location.
- **Partner Confirmation:** The delivery partner confirms pickup.
- **Customer Confirmation:** The customer confirms receipt.

Compromising all three simultaneously is exponentially harder than compromising one, and it prevents collusion between just the delivery partner and customer.

### 2. Dual GPS Stream Verification <a name="2-dual-gps-stream-verification"></a>
A tamper-proof GPS tracker is embedded directly inside the delivery container, completely independent of the delivery partner's phone. This means two separate GPS data streams must corroborate each other during every delivery. 
- If the phone GPS claims movement through one location but the container tracker shows it is stationary, the fraud is immediately detectable. 
- Spoofing the phone becomes irrelevant because the container tells its own story.

### 3. Self-Powered Tracker Using Piezoelectric Energy Harvesting <a name="3-self-powered-tracker-using-piezoelectric-energy-harvesting"></a>
The embedded container tracker powers itself using piezoelectric transducers that convert mechanical pressure and vibration into electricity. Indian road conditions produce near-continuous vibration during transit, making this a highly reliable and context-specific power source.
- **Hardware Design:** A dense insulating material surrounds the transducer to protect it and concentrate energy transfer. Surplus power is stored in an onboard battery. 
- **Compliance Lockout:** A wired charging cable exists as a fallback — but while the tracker is charging via cable, the delivery partner cannot accept new orders. If the tracker is offline for any reason, the partner is entirely locked out of the platform. This means an honest worker is mildly inconvenienced at worst, while a fraudster who tampers with the tracker loses their ability to work completely.

### 4. Financial Circuit Breakers on the Payout Side <a name="4-financial-circuit-breakers-on-the-payout-side"></a>
Most fraud detection focuses on cash collection, but this fraud targets platform-side payouts. The defense actively protects liquidity by applying:
- **Delayed Settlements:** Applied for all new accounts.
- **Earnings Spikes Alerts:** Flags earnings that spike suddenly without a corresponding order history.
- **Review Holds:** Unusually large payouts are automatically held for manual review.

This mirrors the remittance cap logic commonly used on the collection side but strictly applies it in reverse.

### 5. Network-Level Fraud Ring Detection <a name="5-network-level-fraud-ring-detection"></a>
Individual fake transactions can look clean in isolation. The fraud only becomes visible when you look at accounts collectively. The system uncovers organized fraud rings through:
- Shared device fingerprints across accounts.
- IP address clustering and accounts created in batches within short timeframes.
- Suspiciously identical GPS path shapes across different partners.
- Payout destinations converging to the same UPI IDs or bank accounts.

A single suspicious account is a flag. Five hundred accounts sharing the same device ID is a confirmed ring.

### 6. Physics-Based Fraud Detection Including Fuel Allowance Corroboration <a name="6-physics-based-fraud-detection-including-fuel-allowance-corroboration"></a>
The system analyzes the physical plausibility of every delivery to proactively detect anomalies:
- **Timestamp Corridors:** Deliveries are bounded by vendor-side dispatch (start) and customer receipt (end) timestamps. The claimed distance must be physically plausible within this specific time window given real traffic conditions.
- **Fuel Claim Verification:** Fuel allowances claimed by delivery partners are heavily cross-referenced against actual movement data from the container tracker.
- **Map Corroboration:** Both the phone-based map and container tracker map must tell the exact same story. A stationary container tracker combined with a maximum fuel claim acts as an immediate fraud signal to halt payouts.

---

## Protecting Honest Workers — Fairness by Design <a name="protecting-honest-workers--fairness-by-design"></a>

The core principle is that no single anomaly should result in an immediate ban — the system should respond in graduated steps based on accumulating evidence.

### Tracker Offline or Low Power
A genuine delivery partner whose tracker runs flat is not immediately flagged as a fraudster. They are simply locked out of accepting new orders until the tracker is functional again, either through the piezoelectric system recharging naturally during their next trip or through the wired cable fallback. This is a temporary inconvenience, not a punishment. A bad actor who has tampered with or disabled their tracker faces the same lockout — but unlike the honest worker, they cannot resolve it legitimately.

### Unusual GPS Patterns
A genuine delivery partner might show irregular GPS data due to poor network coverage, tunnels, or waiting at a location for an extended time. A single irregular GPS event is not a flag. The system only escalates when the phone GPS and the independent container tracker simultaneously show patterns that contradict each other — because a legitimate signal issue would affect both streams similarly, while deliberate spoofing would create a discrepancy between them.

### Timestamp Corridor Violations
A genuine delivery partner might take longer than expected due to traffic, road conditions, or a difficult address. The timestamp corridor has a built-in tolerance buffer for real-world variability. A flag is only raised when the claimed distance and fuel allowance are physically impossible within the time elapsed — not merely slow or delayed.

### Financial Anomalies
A genuine delivery partner who has an unusually good day during a surge period should not be penalized for earning more than usual. The financial circuit breaker only flags accounts where earnings spike without a corresponding increase in verified order volume, or where payout destinations match those of other flagged accounts in the network.

### Graduated Response Logic
The system should never jump straight to a ban. The response ladder works as follows:
- **First Anomaly:** Triggers a silent internal flag with no action taken against the partner.
- **Second Anomaly (within a short window):** Triggers a temporary order hold pending automatic review.
- **Third Anomaly (or a confirmed discrepancy between dual GPS streams):** Triggers a manual review by a human investigator before any account action is taken.
- **Confirmed Fraud Ring:** A confirmed pattern of coordinated fraud across multiple accounts triggers suspension and escalation.

At every stage the delivery partner should be able to see their status and understand why a hold has been placed, with a clear path to resolution.

---

## Beyond the Prototype — Why This Is Infrastructure, Not a Feature <a name="beyond-the-prototype"></a>

### Hardware Leasing Model
The self-powered tamper-proof container tracker is manufactured and leased to delivery platforms on a per-container subscription basis. The unit cost is recovered within weeks through prevented fraud losses alone, making the ROI case straightforward for any platform operating at scale.

### Fraud Detection SaaS
The software verification layer — including biometric anchoring, timestamp corridor validation, dual GPS corroboration, and network-level ring detection — is packaged as a standalone API that any gig economy platform can license. Food delivery today. Ride sharing tomorrow. Any platform that moves goods or people using independent contractors faces this problem.

### Trust Certification
Platforms that adopt this architecture can be independently certified as fraud-resistant. This certification becomes a meaningful signal to investors, insurance providers, and enterprise clients who depend on last mile reliability and cannot afford liquidity exposure from coordinated fraud rings.
