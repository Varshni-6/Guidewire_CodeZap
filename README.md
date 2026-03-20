# Vigilant
### Adversarial Fraud Defense for Last-Mile Delivery Platforms

> Built by **CodeZap**

---

## What Is Vigilant?

Vigilant is a multi-layered fraud detection and prevention infrastructure designed to stop coordinated GPS spoofing, fake delivery rings, and platform payout drain in last-mile delivery operations.

Individual fraudulent transactions are designed to look clean. Vigilant exposes them by cross-referencing hardware telemetry, physics-based delivery validation, and network-level pattern analysis — simultaneously.

---

## The Problem

A coordinated fraud ring of 500 fake accounts — combining fake delivery partners, fake customers, and potentially fake vendors — uses GPS spoofing to simulate deliveries that never happen. Each transaction looks legitimate in isolation. The platform bleeds out through its own payout system.

Traditional fraud detection looks at individual transactions. It misses this entirely.

---

## System Architecture

```mermaid
flowchart TD
    subgraph INPUT["Data Inputs"]
        A[Partner Phone GPS]
        B[Container Tracker GPS]
    end

    subgraph VERIFY["Verification Engine"]
        D[Dual GPS Corroboration]
        F[Physics and Fuel Checker]
        G[Three-Point Confirmation]
    end

    subgraph NETWORK["Network Analysis"]
        H[Device Fingerprint Clustering]
        I[IP and UPI Destination Analysis]
        J[Fraud Ring Detector]
    end

    subgraph FINANCE["Financial Layer"]
        K[Earnings Spike Monitor]
        L[Payout Destination Check]
        M[Settlement Delay Engine]
    end

    subgraph RESPONSE["Response Engine"]
        N{Anomaly Score}
        O[Silent Flag]
        P[Temporary Hold]
        Q[Manual Review]
        R[Suspend and Escalate]
    end

    A --> D
    B --> D
    D --> F
    F --> G
    G --> N
    H --> J
    I --> J
    J --> N
    K --> N
    L --> N
    M --> N
    N -->|1 anomaly| O
    N -->|2 anomalies| P
    N -->|3 anomalies| Q
    N -->|confirmed ring| R
```

---

## Threat Model

| Attack Type | Defense Layer | Response |
|---|---|---|
| Partner and Customer Collusion | Three-Point Verification | Requires physical multi-party authentication spanning the full delivery timeline |
| GPS Spoofing via Mock Locations | Dual GPS Stream Corroboration | Detects discrepancy between phone GPS and independent container tracker |
| Hardware Tampering or Disabling | Self-Powered Piezoelectric Tracker | Tracker offline means partner is locked out of accepting orders |
| Liquidity and Payout Drain | Financial Circuit Breakers | Earnings spikes and large payouts held for review |
| Coordinated Fraud Rings | Network-Level Detection | Shared device fingerprints, IP clusters, and UPI destination convergence |
| Phantom and Speed Deliveries | Physics-Based and Fuel Corroboration | Distance and fuel claims validated against container movement |

---

## How a Clean Delivery Works End to End

A vendor prepares an order and confirms dispatch on the platform. That confirmation activates the container tracker embedded in the delivery container. The delivery partner arrives and confirms pickup. Two independent GPS streams now run in parallel — the partner's phone and the container tracker — both feeding live data to the verification engine.

During transit, the physics checker continuously validates whether the claimed route and fuel usage are plausible given the distance and real traffic conditions. When the customer confirms receipt via OTP, the system corroborates all three confirmations, cross-references the dual GPS streams, validates the fuel claim against actual container movement, and only then releases the payout.

Every honest delivery passes through this pipeline invisibly. Every fraudulent one surfaces a discrepancy.

---

## Adversarial Defense & Anti-Spoofing Strategy

### 1. Differentiation — Genuine Stranded Partner vs Bad Actor

The core challenge is that a genuinely stranded delivery partner and a bad actor spoofing their location can look identical when you only look at one data stream. Vigilant solves this by never relying on a single stream.

A genuine signal disruption — poor network coverage, a tunnel, an extended wait at an address — affects both the phone GPS and the independent container tracker in the same way. Both streams go quiet or show the same anomaly simultaneously. The system recognises this as a real-world condition and applies a tolerance buffer rather than a flag.

A deliberate GPS spoof is fundamentally different. The fraudster manipulates the phone GPS to show movement, but the container tracker — which is a completely independent hardware device with no connection to the phone — stays stationary. This creates a discrepancy between the two streams that cannot occur naturally. That discrepancy is the fraud signal.

```mermaid
flowchart LR
    subgraph GENUINE["Genuine Signal Issue"]
        GA[Phone GPS — disrupted]
        GB[Container GPS — disrupted]
        GC{Both streams affected?}
        GD[Tolerance buffer applied]
        GA --> GC
        GB --> GC
        GC -->|Yes — same pattern| GD
    end

    subgraph SPOOF["GPS Spoofing Attack"]
        SA[Phone GPS — spoofed movement]
        SB[Container GPS — stationary]
        SC{Both streams affected?}
        SD[Fraud signal raised]
        SA --> SC
        SB --> SC
        SC -->|No — discrepancy| SD
    end
```

---

### 2. Data Points — Beyond Basic GPS Coordinates

Vigilant analyses the following data points to detect coordinated fraud rings. No single signal triggers action — the system looks for convergence across multiple dimensions simultaneously.

**Hardware telemetry**
- Dual GPS stream corroboration — phone GPS vs independent container tracker
- Container tracker power state — online, charging via cable, or offline
- Movement pattern continuity — speed, acceleration, and stop duration profiles

**Delivery physics**
- Distance plausibility — claimed distance vs time elapsed vs real traffic conditions
- Fuel claim corroboration — fuel allowance claimed vs actual container tracker movement
- Route map corroboration — both GPS streams must describe the same route

**Account and network signals**
- Device fingerprint clustering — same device ID across multiple accounts
- IP address clustering — accounts created or operating from the same IP ranges
- Batch account creation — multiple accounts created within short timeframes
- GPS path shape matching — suspiciously identical route shapes across different partners
- Payout destination convergence — multiple accounts paying out to the same UPI IDs or bank accounts

**Financial behaviour**
- Earnings spike detection — sudden earnings increase without corresponding verified order volume
- New account settlement delay — all new accounts held on delayed settlement by default
- Payout destination flagging — outgoing payments cross-referenced against known fraud ring destinations

```mermaid
graph TD
    A[Account A] -->|shared device ID| X[Fraud Ring Node]
    B[Account B] -->|shared device ID| X
    C[Account C] -->|batch creation same IP| X
    D[Account D] -->|identical GPS path shape| X
    E[Account E] -->|same UPI destination| X
    F[Account F] -->|shared device ID| X
    X --> G{Ring Confirmed?}
    G -->|500+ accounts converge| H[Full Ring Suspension]
    G -->|partial signal| I[Escalate to Investigator]
```

A single suspicious account is a flag. Five hundred accounts sharing the same device ID is a confirmed ring.

---

### 3. UX Balance — Handling Flagged Claims Without Penalising Honest Workers

No single anomaly triggers a ban or a penalty. The system uses a graduated response ladder that separates temporary inconvenience from actual punishment, and gives honest workers a clear path to resolution at every stage.

```mermaid
flowchart TD
    A[Anomaly Detected] --> B{First Anomaly?}
    B -->|Yes| C[Silent Internal Flag - No action taken]
    C --> D{Second Anomaly within window?}
    D -->|Yes| E[Temporary Order Hold - Auto review]
    D -->|No| F[Flag cleared after clean deliveries]
    E --> G{Third Anomaly or Dual GPS discrepancy?}
    G -->|Yes| H[Manual Review by Human Investigator]
    G -->|No| I[Hold lifted after review]
    H --> J{Fraud Confirmed?}
    J -->|Yes| K[Account Suspension and Escalation]
    J -->|No| L[Account Reinstated - Partner Notified]
```

**First anomaly** — a silent internal flag is raised. The partner sees nothing, receives no notification, and continues working normally.

**Second anomaly within a short window** — a temporary order hold is applied and an automated review begins. The partner is notified with a plain-language explanation of why the hold exists and what resolves it.

**Third anomaly or a confirmed dual GPS discrepancy** — the case is escalated to a human investigator. No automated account action is taken until a human has reviewed the evidence.

**Confirmed fraud ring** — only at this stage is an account suspended. The partner is notified of the reason and the escalation path.

The tracker offline edge case is handled specifically — a genuine partner whose tracker runs flat is locked out of accepting new orders until the tracker is functional again. This is a temporary inconvenience, not a punishment. A bad actor who deliberately disables their tracker faces the same lockout but cannot resolve it legitimately, which is the distinction that matters.

At every stage the delivery partner can see their current status in the app and understands exactly why a hold has been placed, with a visible resolution path. No silent bans. No unexplained blocks.

---

## Defense Layers In Detail

### 1. Three-Point Verification

```mermaid
sequenceDiagram
    participant V as Vendor
    participant P as Delivery Partner
    participant S as Platform System
    participant C as Customer

    V->>S: Confirms order prepared and dispatched
    S->>S: Container Tracker activated

    P->>S: Confirms pickup
    S->>S: Container Tracker stream live

    Note over S: Dual GPS streams now live - Phone GPS vs Container GPS

    P->>C: Delivery in transit
    S->>S: Physics and fuel validation running

    C->>S: Confirms receipt via OTP

    S->>S: All 3 confirmations corroborated?

    alt Clean delivery - all match
        S->>P: Payout approved
    else Discrepancy detected
        S->>S: Anomaly flag raised
        S->>P: Payout held for review
    end
```

Compromising all three confirmation points simultaneously is exponentially harder than compromising one. Partner-customer collusion — the most common attack vector — is neutralized because the vendor confirmation exists independently of both.

---

### 2. Dual GPS Stream Verification

The delivery container carries a tamper-proof GPS tracker that is entirely independent of the delivery partner's phone. Both streams must corroborate each other throughout the delivery.

```mermaid
flowchart LR
    subgraph LEGIT["Legitimate Delivery"]
        LA[Phone GPS - Moving]
        LB[Container GPS - Moving]
        LC{Streams match?}
        LD[Payout Approved]
        LA --> LC
        LB --> LC
        LC -->|Yes| LD
    end

    subgraph SPOOF["GPS Spoofing Attack"]
        SA[Phone GPS - Spoofed movement]
        SB[Container GPS - Stationary]
        SC{Streams match?}
        SD[Anomaly Flagged]
        SA --> SC
        SB --> SC
        SC -->|No| SD
    end
```

Spoofing the phone GPS becomes irrelevant. The container tells its own story independently.

---

### 3. Self-Powered Piezoelectric Container Tracker

The container tracker powers itself using piezoelectric transducers that convert mechanical vibration into electricity. Indian road conditions produce near-continuous vibration during transit, making this a reliable and context-specific power source.

#### Hardware Design — Cross-Sectional View

```mermaid
block-beta
    columns 1
    F["Food compartment\nInsulated interior — food placement zone"]
    C1["· · · · · · · · · · · · Cushion layer · · · · · · · · · · · ·"]
    W["//////////  1 kg counterweight layer  //////////\nDense mass — stabilises piezo vibration capture"]
    C2["· · · · · · · · · · · · Cushion layer · · · · · · · · · · · ·"]
    P["            Piezoelectric crystal layer\nVibration to electricity"]
    T["GPS module          Battery reserve\nTracking layer — self-powered"]
    C3["· · · · · · · · · · · · Cushion layer · · · · · · · · · · · ·"]
    S["Bag shell — rigid outer frame"]
    F --> C1
    C1 --> W
    W --> C2
    C2 --> P
    P --> T
    T --> C3
    C3 --> S
```

```mermaid
flowchart LR
    A[Road Vibration] --> B[Piezoelectric Transducer]
    B --> C[Power Conditioning Circuit]
    C --> D[Onboard Battery]
    D --> E[GPS Module]
    E --> F[Live Telemetry Stream]

    G[Wired Cable Fallback] --> D
    G --> H[Partner Order Lock Active]
```

A wired charging fallback exists but while the tracker charges via cable, the partner cannot accept new orders. If the tracker goes offline for any reason, the partner is locked out entirely. An honest worker is temporarily inconvenienced. A fraudster who disables the tracker loses their ability to work completely and cannot resolve it legitimately.

---

### 4. Financial Circuit Breakers

Most fraud detection focuses on cash collection. Vigilant protects the platform's outgoing payout side.

```mermaid
flowchart TD
    A[Payout Request Received] --> B{New Account?}
    B -->|Yes| C[Delayed Settlement Applied]
    B -->|No| D{Earnings Spike Detected?}
    D -->|Yes| E{Verified Order Volume Match?}
    E -->|No| F[Flag for Manual Review]
    E -->|Yes| G{Payout Destination Flagged?}
    D -->|No| G
    G -->|Yes| F
    G -->|No| H[Payout Approved]
    C --> D
```

Delayed settlements for new accounts, flags for earnings spikes without corresponding verified order volume, and payout destination analysis against known fraud ring UPI IDs and bank accounts.

---

### 5. Network-Level Fraud Ring Detection

Individual fake accounts look clean in isolation. The fraud becomes visible only when accounts are analyzed collectively.

A single suspicious account is a flag. Five hundred accounts sharing the same device ID is a confirmed ring.

---

### 6. Physics-Based Delivery and Fuel Corroboration

Every delivery is checked against physical reality.

- **Fuel Claim Verification** — Fuel allowances are cross-referenced against actual container tracker movement. A stationary tracker combined with a maximum fuel claim is an immediate fraud signal.
- **Map Corroboration** — Both GPS streams must describe the same route. Any divergence is flagged.
- **Distance Plausibility** — The claimed distance must be physically achievable within the time window between vendor dispatch and customer receipt, accounting for real traffic conditions.

---

## Why This Works at Scale

The hardware tracker is manufactured and leased to delivery platforms on a per-container subscription basis. The unit cost is recovered within weeks through prevented fraud losses alone.

The software verification layer — dual GPS corroboration, physics-based delivery validation, and network-level ring detection — is packaged as a standalone API that any gig economy platform can license. Food delivery today. Ride sharing tomorrow. Any platform that moves goods or people using independent contractors faces this exact problem.

Platforms that adopt Vigilant can be independently certified as fraud-resistant. That certification becomes a signal to investors, insurance providers, and enterprise clients who cannot afford liquidity exposure from coordinated fraud rings.

---

## Business Model

| Stream | Description |
|---|---|
| Hardware Leasing | Self-powered container tracker leased per unit per month |
| Fraud Detection SaaS | Verification API licensed to any gig economy platform |
| Trust Certification | Independent fraud-resistance certification for adopting platforms |

---

## Built With

- Piezoelectric energy harvesting hardware
- Dual GPS telemetry corroboration
- Physics-based delivery and fuel validation engine
- Network-level graph analysis for fraud ring detection
- Financial circuit breaker logic on the payout side

---

## Team

**CodeZap**

---

*Vigilant is infrastructure, not a feature.*
