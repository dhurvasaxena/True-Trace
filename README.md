<div align="center">

# 🔍 TRUE TRACE

### Digital Companion for Field Drug Testing

**Turning "trust me, it turned purple" into "verify me."**

![SIH 2026](https://img.shields.io/badge/Smart%20India%20Hackathon-2026-1f4e79?style=for-the-badge)
![Problem ID](https://img.shields.io/badge/PS%20ID-SIH26231-orange?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Software-green?style=for-the-badge)
![Theme](https://img.shields.io/badge/Theme-MedTech%20%2F%20BioTech%20%2F%20HealthTech-blueviolet?style=for-the-badge)

![React Native](https://img.shields.io/badge/React%20Native-Expo-61DAFB?logo=react&logoColor=black)
![TensorFlow Lite](https://img.shields.io/badge/TensorFlow-Lite-FF6F00?logo=tensorflow&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Colour%20Calibration-5C3EE8?logo=opencv&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-API-339933?logo=nodedotjs&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-PostGIS-4169E1?logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?logo=supabase&logoColor=white)
![Polygon](https://img.shields.io/badge/Polygon-Anchoring-8247E5?logo=polygon&logoColor=white)

**Team Moggers** · Smart India Hackathon 2026

</div>

---

> [!IMPORTANT]
> **True Trace produces a *presumptive* field-test result and a supporting digital record. It does NOT replace laboratory confirmatory testing** (e.g. GC-MS / LC-MS/MS). Every result screen, certificate and export carries this disclaimer.

---

## 📑 Table of Contents

1. [TL;DR](#-tldr)
2. [Problem Statement (SIH26231)](#-problem-statement-sih26231)
3. [Understanding the Problem](#-understanding-the-problem)
4. [Our Solution: True Trace](#-our-solution-true-trace)
5. [Key Features](#-key-features)
6. [How It Works — End-to-End Flow](#-how-it-works--end-to-end-flow)
7. [System Architecture](#-system-architecture)
8. [Tech Stack (and Why)](#-tech-stack-and-why)
9. [Module Deep-Dives](#-module-deep-dives)
   - [9.1 Guided Capture](#91-guided-capture)
   - [9.2 Reference Card Calibration](#92-reference-card-calibration)
   - [9.3 Classification Engine (On-Device AI)](#93-classification-engine-on-device-ai)
   - [9.4 Tamper-Evident Digital Record](#94-tamper-evident-digital-record)
   - [9.5 Blockchain Anchoring (Polygon)](#95-blockchain-anchoring-polygon)
   - [9.6 QR-Verifiable Certificate](#96-qr-verifiable-certificate)
   - [9.7 Offline-First Storage & Sync](#97-offline-first-storage--sync)
   - [9.8 Authentication, Roles & Secure Storage](#98-authentication-roles--secure-storage)
   - [9.9 Searchable Log & Command Dashboard](#99-searchable-log--command-dashboard)
10. [Data Model & Database Schema](#-data-model--database-schema)
11. [API Reference](#-api-reference)
12. [Security & Threat Model](#-security--threat-model)
13. [Project Structure](#-project-structure)
14. [Getting Started](#-getting-started)
15. [Demo Walkthrough](#-demo-walkthrough)
16. [Testing & Evaluation](#-testing--evaluation)
17. [Feasibility & Viability](#-feasibility--viability)
18. [Impact & Benefits](#-impact--benefits)
19. [Research & References](#-research--references)
20. [Limitations & Honest Disclaimers](#-limitations--honest-disclaimers)
21. [Roadmap](#-roadmap)
22. [Team](#-team)
23. [License](#-license)

---

## ⚡ TL;DR

| | |
|---|---|
| **What** | A mobile app that works alongside *existing* colorimetric drug-testing kits — no new hardware. |
| **Capture** | Officer photographs the reacted kit with a **reference colour card** in the frame. |
| **Standardise** | The app detects the card, **corrects for lighting**, and measures the reaction colour scientifically. |
| **Classify** | An **on-device AI + colour-science** engine outputs `POSITIVE` / `NEGATIVE` / `INCONCLUSIVE` with a confidence score. |
| **Seal** | A **tamper-evident record** is created: SHA-256 image hash + timestamp + GPS + operator ID, **digitally signed (ECDSA)** and **hash-chained**. |
| **Prove** | A **PDF certificate with a QR code** lets anyone verify the record hasn't been altered. |
| **Log** | A **searchable offline log** on the phone and a **map-based command dashboard** for supervisors. |
| **Offline** | Everything works in airplane mode; records sync when the network returns. |

---

## 📋 Problem Statement (SIH26231)

| Field | Detail |
|---|---|
| **Problem Statement ID** | SIH26231 |
| **Title** | Digital Companion for Field Drug Testing |
| **Theme** | MedTech / BioTech / HealthTech |
| **Category** | Software |
| **Team Name** | Team Moggers |
| **Team ID** | `<to be filled>` |

### Background (from the problem statement)

Field drug-testing kits currently in use rely on **visual interpretation of a colour-change reaction**. This makes results **subjective**, **difficult to standardise across officers**, and leaves **no verifiable record** that a test was actually conducted at a given place and time. As a result, field-test outcomes cannot presently be relied upon as documentary evidence.

### What the application must do

1. **Capture** an image of the test result using the device camera, with a **reference colour card in-frame** for lighting calibration.
2. **Automatically classify** the result against defined outcome categories (e.g. positive, negative, inconclusive).
3. **Generate a tamper-evident digital record** with timestamp, GPS location, operator identifier and a **cryptographic hash of the captured image**.
4. **Maintain a simple, searchable log** of tests conducted.

### Expected deliverable

A working mobile/web prototype demonstrating image capture, automated result classification and generation of a **signed** digital record.

---

## 🚔 Understanding the Problem

A colorimetric field test works like this: a small amount of a suspected substance is exposed to a chemical reagent, and the reagent **changes colour** depending on what the substance contains. The officer then compares that colour to a printed chart and decides what it means.

That single human judgement is where everything breaks down:

| # | Pain point | Why it matters |
|---|---|---|
| 1 | **Subjective interpretation** | Two officers can look at the same vial and call it differently. Colour perception varies from person to person (and ~8% of men have some form of colour-vision deficiency). |
| 2 | **Lighting changes the colour** | Sunlight, tube-lights, sodium street lamps and phone torches all shift how a colour looks. A "purple" at noon can look "brown" at night. |
| 3 | **No standardisation** | There is no common, measurable reference — every reading is relative to the officer's eyes and surroundings. |
| 4 | **No proof the test happened** | There is no record of *when*, *where*, *by whom*, or *what the reaction actually looked like*. |
| 5 | **Not usable as documentary evidence** | An unrecorded, unverifiable visual opinion cannot support a case file or chain of custody. |

**In one line:** today's process is *"trust me, it turned purple."*

---

## 💡 Our Solution: True Trace

True Trace is a **digital companion** that sits next to the existing kit and adds three things the kit alone cannot provide:

```
   ┌─────────────────┐     ┌─────────────────────┐     ┌──────────────────────┐
   │  OBJECTIVITY    │     │   ACCOUNTABILITY    │     │    VERIFIABILITY     │
   │                 │     │                     │     │                      │
   │ Reference-card  │ ──▶ │ Who, where, when,   │ ──▶ │ Signed, hash-chained │
   │ calibration +   │     │ what — captured     │     │ record + QR cert     │
   │ AI classifier   │     │ automatically       │     │ anyone can check     │
   └─────────────────┘     └─────────────────────┘     └──────────────────────┘
```

| Problem | How True Trace solves it |
|---|---|
| Subjective colour reading | Camera measurement + reference-card calibration + algorithmic classification |
| No standardisation across officers | The *same* calibrated algorithm runs for every officer, on every device |
| Lighting variation | Colour correction computed from known patches on the reference card in every photo |
| No proof the test happened | Signed record with image hash, timestamp, GPS and operator ID |
| Records can be quietly edited | Hash chain + digital signature + public blockchain anchoring expose any change |
| Remote areas without internet | Fully offline-first; syncs automatically later |
| Hard for supervisors to oversee | Command dashboard with map, filters and PDF export |

---

## ✨ Key Features

| Feature | Description |
|---|---|
| 📶 **Offline-First** | Capture → classify → sign → certificate → log all work with zero connectivity. |
| 🎯 **Guided Capture** | On-screen frame guide; capture is blocked until the reference card and kit are correctly placed and the image passes quality checks. |
| 🎨 **Reference Card Calibration** | OpenCV detects the card, measures its known colour patches, and computes a colour-correction matrix to normalise lighting. |
| 🧠 **On-Device AI** | A TensorFlow Lite model classifies the calibrated reaction zone — no cloud call, no data leaves the phone. |
| 📊 **Confidence Scoring** | Every result shows a confidence percentage plus an *expected vs observed* colour swatch comparison. |
| 🔐 **Tamper-Evident Records** | SHA-256 image hash, ECDSA P-256 signature, and a per-device hash chain. |
| ⛓️ **Blockchain Anchoring** | Periodic Merkle roots of records are anchored on Polygon for independent, public proof of existence. |
| 🧾 **QR-Verifiable Certificates** | PDF "Field Test Certificate" with a QR code that opens the verification portal. |
| 🔑 **Secure Storage** | Officer credentials and signing keys stored with `expo-secure-store` (Android Keystore / iOS Keychain backed). |
| 📍 **GPS Integration** | Location with accuracy check and mock-location detection on every record. |
| 🔎 **Searchable Log** | Filter tests by date, officer, kit type, result, and sync status. |
| 🗺️ **Command Dashboard** | Map view of all tests, filters, verification status, and PDF export for supervisors. |

---

## 🔄 How It Works — End-to-End Flow

The prototype flow has four stages:

```
① Scan the field kit  →  ② On-device AI analysis  →  ③ Tamper-evident report  →  ④ Searchable log + dashboard
```

### Detailed sequence

```mermaid
sequenceDiagram
    autonumber
    actor Officer
    participant App as True Trace App (offline)
    participant CV as Calibration Engine
    participant AI as TFLite Classifier
    participant Crypto as Crypto Module
    participant DB as Local SQLite
    participant API as Backend API
    participant Chain as Polygon

    Officer->>App: Log in (Officer ID + PIN)
    Officer->>App: Select kit type, start reaction timer
    App->>Officer: Camera with placement guide
    App->>CV: Live frames
    CV-->>App: Card detected + quality OK
    Officer->>App: Capture
    App->>Crypto: SHA-256 of raw image bytes
    App->>CV: Warp, sample patches, colour-correct
    CV->>AI: Calibrated reaction-zone crop
    AI-->>App: Class probabilities
    App->>App: Fuse AI + colour distance, apply inconclusive rules
    App->>App: Read GPS (accuracy + mock check)
    App->>Crypto: Canonical record, sign with ECDSA, link to chain
    Crypto->>DB: Store sealed record (status PENDING)
    App->>Officer: Result + confidence + PDF certificate with QR
    Note over App,API: Later, when network is available
    DB->>API: Sync queue uploads record + original image
    API->>API: Verify hash, signature, chain, sequence
    API->>Chain: Periodic Merkle root anchor
    API-->>DB: Mark SYNCED / ANCHORED
```

---

## 🏗️ System Architecture

```mermaid
flowchart LR
    subgraph Device["📱 Field Device - React Native + Expo"]
        CAM["Camera and GPS"]
        CAL["Calibration<br/>OpenCV / ML Kit"]
        TFL["TensorFlow Lite<br/>Classifier"]
        CRY["Crypto<br/>SHA-256 + ECDSA<br/>Hash Chain"]
        LDB[("Secure SQLite /<br/>WatermelonDB")]
        SEC["expo-secure-store<br/>Keys and Credentials"]
        PDF["PDF Certificate<br/>+ QR"]
        CAM --> CAL --> TFL --> CRY --> LDB
        SEC --- CRY
        CRY --> PDF
    end

    subgraph Cloud["☁️ Backend - Node.js + Supabase"]
        API["REST API<br/>JWT Auth"]
        PG[("PostgreSQL<br/>+ PostGIS")]
        OBJ[("Object Storage<br/>Original Images")]
        ANC["Anchoring Worker<br/>Merkle Roots"]
        API --> PG
        API --> OBJ
        PG --> ANC
    end

    subgraph Web["🖥️ Web"]
        DASH["Command Dashboard<br/>Map + Filters + Export"]
        VER["Public Verification<br/>Portal"]
    end

    POLY[["⛓️ Polygon"]]

    LDB -- "Background Sync Queue" --> API
    ANC --> POLY
    DASH --> API
    VER --> API
    PDF -. "QR scan" .-> VER
```

### Layers at a glance

| Layer | Responsibility | Runs on |
|---|---|---|
| **Frontend (Mobile App)** | Camera, GPS, capture wizard, results, certificates, offline log | Android (primary), iOS |
| **Offline Intelligence** | Calibration, classification, hashing, signing, hash chain | On-device |
| **Backend** | Auth, sync ingestion, verification, spatial queries, anchoring | Node.js + Supabase |
| **Dashboard & Verification** | Supervisor oversight, public record verification | Web browser |

---

## 🧰 Tech Stack (and Why)

### Frontend — Mobile App

| Technology | Purpose | Why we chose it |
|---|---|---|
| **React Native + Expo** | Cross-platform mobile app | One codebase for Android & iOS; fast iteration; Android-first for budget devices used in the field |
| **react-native-vision-camera** | High-control camera capture + frame processors | Lets us run card detection on live frames before allowing capture |
| **expo-location** | GPS coordinates, accuracy, mock-location flag | Accuracy gate and spoofing detection on Android |
| **expo-sqlite / WatermelonDB** | Offline local database | Reliable, queryable, works without network |
| **expo-secure-store** | Store officer credentials & signing keys | Backed by Android Keystore / iOS Keychain |
| **expo-crypto + @noble/curves** | SHA-256 hashing, ECDSA P-256 signing | Standards-based, audited, works fully offline |
| **expo-print + expo-sharing** | PDF certificate generation and sharing | Generates certificates offline from HTML templates |
| **react-native-qrcode-svg** | QR code on certificates | Lightweight, offline |

### Offline Intelligence

| Technology | Purpose |
|---|---|
| **OpenCV** (native module / frame processor) or **Google ML Kit** | Reference-card detection, perspective correction, patch sampling, colour correction |
| **TensorFlow Lite** (via `react-native-fast-tflite`) | On-device classification of calibrated reaction zones |
| **CIEDE2000 colour distance** | Perceptually meaningful colour comparison in CIELAB space |
| **SHA-256 Hash Chain** | Tamper evidence across records |
| **JWT Auth** | Stateless session tokens for sync and dashboard |

### Backend

| Technology | Purpose |
|---|---|
| **Node.js** (Express / Fastify) | REST API, sync ingestion, verification logic |
| **PostgreSQL + PostGIS** | Relational store with spatial queries (tests within X km, heatmaps) |
| **Supabase** | Managed Postgres, auth helpers and object storage for images |
| **Polygon** (Amoy testnet for prototype) | Public anchoring of Merkle roots for independent timestamp proof |

### Web

| Technology | Purpose |
|---|---|
| **React + Vite** | Command dashboard and verification portal |
| **Leaflet** | Map view with test pins |

> **Note on Expo:** VisionCamera, OpenCV and TFLite are native modules, so the app runs as an **Expo development build** (`npx expo run:android`), not inside Expo Go.

---

## 🔬 Module Deep-Dives

### 9.1 Guided Capture

The capture screen is a step-by-step wizard designed so that a tired officer at 2 a.m. on a highway still produces a usable, standardised image.

**Wizard steps**

1. **Select kit type** — e.g. Marquis, Mecke, Mandelin, Scott's (Cobalt Thiocyanate), Duquenois–Levine. Each kit has a *profile* (see [9.3](#93-classification-engine-on-device-ai)).
2. **Start reaction timer** — colour reactions evolve over time, so the app asks the officer to tap *"Reagent applied"* and then prompts capture inside the kit's recommended reading window. The elapsed time is stored in the record.
3. **Frame the shot** — an overlay shows two target zones: one for the **reference colour card**, one for the **reaction zone** (ampoule/vial/well).
4. **Automatic quality gates** — the capture button stays disabled until all checks pass:

| Check | Method | Default threshold* |
|---|---|---|
| Reference card detected | ArUco/fiducial markers or contour detection on the card corners | All 4 corners found |
| Sharpness | Variance of the Laplacian on the reaction zone | Above a tuned minimum |
| Exposure | % of pixels clipped at 0 or 255 | < 2% clipped |
| Glare | Fraction of specular-highlight pixels in reaction zone | < 5% |
| Reaction zone in ROI | Bounding box inside the guide region | Fully inside |
| Calibration quality | Residual error on card patches after correction (see 9.2) | Mean ΔE₀₀ below tuned limit |

<sub>*Thresholds are configurable and tuned during testing; values shown are starting points.</sub>

5. **Capture** — the original JPEG is written to disk **once** and its bytes are **hashed immediately**. All processing uses copies; the original is never modified.

---

### 9.2 Reference Card Calibration

This is the heart of **standardisation**. Instead of trusting how the phone camera sees colour, we measure a card whose true colours we already know, and use it to correct the whole image.

#### The reference card

A printed card with:
- **Fiducial markers** in the four corners (for robust detection and perspective correction)
- **Neutral patches**: white, 3 greys, black (for white balance and tone)
- **Chromatic patches**: red, green, blue, cyan, magenta, yellow
- **Reagent-relevant patches**: purples, oranges, browns, greens and blues that match the typical reaction colours of the supported kits

The true (reference) colour of every patch is measured once with a spectrophotometer/colorimeter and stored in the app as CIELAB values.

#### The algorithm

```
Captured image
   │
   ├─ 1. Detect the 4 fiducial markers
   ├─ 2. Compute homography → warp card to a canonical flat layout
   ├─ 3. Sample each patch (median of the inner 60% to avoid edges/noise)
   ├─ 4. Linearise sRGB (undo gamma) for both measured and reference values
   ├─ 5. Solve least-squares colour-correction matrix M
   ├─ 6. Validate: residual ΔE on patches must be below threshold
   ├─ 7. Apply M to the reaction zone
   └─ 8. Convert corrected reaction zone → XYZ → CIELAB (D65)
```

**Step 5 — solving the correction matrix.** With *N* patches, stack the measured linear RGB values (with a bias column) into matrix **P** (N × 4) and the reference linear RGB values into **R** (N × 3). We find the affine correction **M** (4 × 3) that best maps measured → reference:

$$
M = \arg\min_{M} \lVert P M - R \rVert^2 = (P^\top P)^{-1} P^\top R
$$

Applying **M** to every pixel in the reaction zone removes the colour cast introduced by the lighting and the specific phone camera.

**Step 6 — self-check.** After correction, we re-measure the card patches and compute the mean CIEDE2000 difference (ΔE₀₀) to their reference values. If it is too high, the lighting is too poor or the card is damaged, and the officer is asked to retake. **This residual is stored in the record** as a calibration-quality metric.

> **Why CIELAB and ΔE₀₀?** RGB distances don't match how humans perceive colour difference. CIELAB is designed to be perceptually uniform, and CIEDE2000 is the industry-standard formula for how different two colours *look*.

---

### 9.3 Classification Engine (On-Device AI)

We use a **hybrid** engine: a trained neural network *and* a transparent colour-science rule, fused together. This gives accuracy from the model and **explainability** from the rule — important when a result may be questioned.

#### Kit profiles

Each kit is described by a JSON profile, so new kits can be added without changing code:

```json
{
  "kit_id": "MARQUIS",
  "display_name": "Marquis Reagent",
  "read_window_sec": [10, 60],
  "outcomes": [
    { "label": "NEGATIVE", "indicates": "No significant colour change", "ref_lab": [[88.0, 1.0, 12.0]] },
    { "label": "POSITIVE", "indicates": "MDMA / MDA class", "ref_lab": [[25.0, 20.0, -25.0], [12.0, 5.0, -8.0]] },
    { "label": "POSITIVE", "indicates": "Amphetamine class", "ref_lab": [[55.0, 35.0, 60.0], [38.0, 25.0, 35.0]] },
    { "label": "POSITIVE", "indicates": "Opiate class", "ref_lab": [[35.0, 35.0, -30.0]] }
  ],
  "thresholds": {
    "max_delta_e": 12.0,
    "min_confidence": 0.75
  }
}
```

> ⚠️ The `ref_lab` values above are **illustrative placeholders**. Production values must be derived from the kit manufacturer's official colour chart and from calibrated captures of real reactions.

**Commonly documented colorimetric reactions** (for context — the app is configurable for any kit):

| Kit | Typical target | Typical indicative colour |
|---|---|---|
| Marquis | MDMA / MDA | Purple → black |
| Marquis | Amphetamine / Methamphetamine | Orange → reddish-brown |
| Marquis | Opiates (e.g. heroin, morphine) | Purple / violet |
| Scott's (Cobalt Thiocyanate) | Cocaine | Blue |
| Duquenois–Levine | Cannabinoids | Violet (in chloroform layer) |

#### Layer A — Colour-distance rule

For the calibrated reaction-zone colour **c** (in CIELAB), compute ΔE₀₀ to every reference colour of every outcome in the selected kit. The closest outcome and its distance are recorded.

#### Layer B — TensorFlow Lite model

| Item | Detail |
|---|---|
| **Input** | Calibrated, cropped reaction-zone image (e.g. 224 × 224) |
| **Architecture** | Lightweight mobile CNN (e.g. MobileNetV3-Small / EfficientNet-Lite0) |
| **Output** | Softmax probabilities over the kit's outcome classes |
| **Size** | Quantised (int8) to a few MB for budget Android phones |
| **Runtime** | Fully on-device via `react-native-fast-tflite` |

**Training data plan**
- Capture real reactions of each kit with the reference card across **multiple phones** and **lighting conditions** (daylight, shade, LED, fluorescent, sodium lamp, torch).
- Apply the calibration step *before* training so the model learns from normalised colour.
- Augment with small colour jitter, blur, rotation and glare.
- **Split train/test by device and session** (not random images) to avoid overly optimistic accuracy.

#### Fusion & the INCONCLUSIVE rule

The final result is decided by explicit, auditable rules:

```
IF   calibration quality failed                       → INCONCLUSIVE ("poor lighting")
ELIF model top-class probability < min_confidence     → INCONCLUSIVE ("low confidence")
ELIF nearest ΔE₀₀ > max_delta_e                       → INCONCLUSIVE ("colour not on chart")
ELIF model class ≠ colour-rule class                  → INCONCLUSIVE ("methods disagree")
ELSE                                                  → model class (POSITIVE / NEGATIVE)
```

**Confidence score** shown to the officer = model probability of the chosen class, displayed alongside:
- an **observed colour swatch** (calibrated),
- the **expected swatch** for the matched outcome,
- the **ΔE₀₀ distance** between them.

> Design principle: **when in doubt, say INCONCLUSIVE.** A presumptive tool should never overstate certainty. Inconclusive results are always flagged for laboratory testing.

---

### 9.4 Tamper-Evident Digital Record

This is what makes a field test **documentable**. Every test becomes a sealed record that cannot be silently changed.

#### What goes into a record

```json
{
  "schema_version": "1.0",
  "record_id": "NCB-77521",
  "device_id": "DEV-8f3a91c2",
  "device_seq": 142,
  "operator_id": "NCB-2291",
  "kit_type": "MARQUIS",
  "captured_at_utc": "2026-09-18T03:44:12.418Z",
  "gnss_time_utc": "2026-09-18T03:44:11.000Z",
  "reaction_elapsed_sec": 34,
  "location": {
    "lat": 28.4595,
    "lng": 77.0266,
    "accuracy_m": 8.2,
    "mocked": false,
    "label": "NH-48, Gurugram Toll Plaza"
  },
  "image_sha256": "9f2c…e71a",
  "result": {
    "label": "NEGATIVE",
    "confidence": 0.96,
    "matched_outcome": "No significant colour change",
    "observed_lab": [87.1, 1.6, 10.9],
    "delta_e00": 2.4,
    "calibration_residual_de": 1.8,
    "model_version": "marquis-v0.3-int8",
    "app_version": "0.9.0"
  },
  "prev_record_hash": "4b7d…a0c3",
  "disclaimer": "PRESUMPTIVE FIELD TEST — subject to laboratory confirmatory testing."
}
```

#### Sealing process (step by step)

```mermaid
flowchart TD
    A["Original JPEG bytes"] -->|"SHA-256"| B["image_sha256"]
    B --> C["Build record JSON<br/>+ prev_record_hash + device_seq"]
    C --> D["Canonicalise JSON<br/>RFC 8785 JCS"]
    D -->|"ECDSA P-256 sign<br/>with device private key"| E["signature"]
    D --> F["record_hash = SHA-256 of<br/>canonical record + signature"]
    E --> F
    F --> G["Becomes prev_record_hash<br/>of the NEXT record"]
    F --> H[("Stored in local DB")]
```

1. **Hash the image.** `image_sha256 = SHA-256(original JPEG bytes)`. Changing even one pixel produces a completely different hash.
2. **Build the record** with all metadata, the previous record's hash, and a per-device sequence number (`device_seq`).
3. **Canonicalise** the JSON using RFC 8785 (JSON Canonicalization Scheme), so the same data always produces the same bytes regardless of key order or whitespace.
4. **Sign** the canonical bytes with the device's **ECDSA P-256** private key.
5. **Compute the record hash** over the canonical record *plus* its signature.
6. **Chain** — this `record_hash` becomes the `prev_record_hash` of the next record. The first record on a device links to a fixed `GENESIS` value.

#### Why each piece matters

| Mechanism | What it protects against |
|---|---|
| **Image hash** | Photo being swapped or edited after capture |
| **Digital signature** | Metadata being edited (e.g. changing `POSITIVE` to `NEGATIVE`), or a record being forged by someone without the device key |
| **Hash chain** | Records being inserted, reordered or modified in history — any change breaks every later link |
| **Sequence number** | Records being quietly *deleted* — a gap in `device_seq` is immediately visible |
| **Blockchain anchor** | The server operator itself rewriting history (see 9.5) |

> **Improvement over a naive chain:** we chain the hash of the *whole signed record*, not just the image hash. This way, changing any field — result, time, location or operator — breaks the chain, not only changing the photo.

---

### 9.5 Blockchain Anchoring (Polygon)

Even with signatures and chains, someone with full control of the central database could theoretically rebuild a fake history. Anchoring removes that possibility by publishing a fingerprint of the records somewhere nobody controls.

**How it works**

1. The backend collects all newly synced `record_hash` values since the last anchor (e.g. every hour or every 100 records).
2. It builds a **Merkle tree** and computes the **Merkle root** (one 32-byte hash representing all of them).
3. The root is written to **Polygon** in a transaction.
4. Each record stores its **Merkle proof** and the **transaction hash**.

**What this proves:** the record existed, in exactly this form, **no later than** the block time of that transaction.

**Privacy:** only hashes go on-chain. No images, names, locations or results are ever published.

> For the prototype we use the **Polygon Amoy testnet** (no real cost). The design is chain-agnostic — a government-operated ledger or an RFC 3161 Trusted Timestamp Authority can be used instead in deployment.

---

### 9.6 QR-Verifiable Certificate

After every test, the app generates a **Field Test Certificate** (PDF) — offline.

**Certificate contents**
- Record ID, date/time (IST and UTC), location and map snippet
- Operator ID, device ID, kit type
- Result, confidence, observed vs expected colour swatches
- Thumbnail of the captured image
- `image_sha256` and `record_hash` (shortened, with full values in the QR)
- The presumptive-result disclaimer
- **QR code** linking to the verification portal

**QR payload**

```
https://<verify-domain>/v/NCB-77521?rh=<record_hash>
```

**What the verification portal checks**

| # | Check | Pass condition |
|---|---|---|
| 1 | **Record exists** | Record ID is found on the server |
| 2 | **Record hash matches QR** | Hash in QR = hash stored on server |
| 3 | **Image integrity** | SHA-256 of stored original image = `image_sha256` |
| 4 | **Signature** | ECDSA signature valid for the officer's registered device public key |
| 5 | **Chain link** | `prev_record_hash` equals the hash of the previous record from that device |
| 6 | **Sequence continuity** | No gaps in `device_seq` up to this record |
| 7 | **Anchor** | Merkle proof matches an on-chain root (or shows *Pending anchor*) |

**Possible outcomes:** ✅ `VERIFIED` · ⏳ `VERIFIED – PENDING ANCHOR` · ❌ `TAMPERED` (with the exact failed check shown in red) · ❓ `NOT FOUND / NOT YET SYNCED`

Supervisors, laboratory staff and legal teams can all use the same portal.

---

### 9.7 Offline-First Storage & Sync

**Principle:** the phone is the source of truth at the moment of capture; the network is optional.

```mermaid
stateDiagram-v2
    [*] --> SEALED: Record created and signed
    SEALED --> PENDING: Added to sync queue
    PENDING --> UPLOADING: Network available
    UPLOADING --> SYNCED: Server verified hash, signature, chain
    UPLOADING --> PENDING: Network lost / retry with backoff
    UPLOADING --> REJECTED: Verification failed
    SYNCED --> ANCHORED: Included in on-chain Merkle root
    ANCHORED --> [*]
```

- **Local DB:** SQLite (or WatermelonDB) stores records; original images are stored in the app's private file directory.
- **Sync queue:** records upload **in sequence order** with exponential backoff.
- **Idempotent uploads:** re-sending the same record is safe; the server de-duplicates by `record_hash`.
- **Server-side verification before insert:** the server re-computes the image hash, verifies the signature and checks the chain link. Invalid records are `REJECTED` and flagged to a supervisor.
- **Append-only:** there is no update or delete endpoint. Corrections are made by adding a new *annotation* record that references the original — the original is never altered.
- **Sync status badges** in the log: `PENDING` · `SYNCED` · `ANCHORED` · `REJECTED`.

---

### 9.8 Authentication, Roles & Secure Storage

#### Roles

| Role | Can do |
|---|---|
| **OFFICER** | Log in on an enrolled device, run tests, view own log, share certificates |
| **SUPERVISOR** | View all tests in their jurisdiction, dashboard, exports, enrol/revoke devices |
| **LAB** | Look up records by ID, verify, attach confirmatory lab result as annotation |
| **ADMIN** | Manage users, kit profiles and model versions |

#### Device enrolment & keys

1. A supervisor enrols a device for an officer.
2. The app generates an **ECDSA P-256 key pair** on the device.
3. The **private key** is stored in `expo-secure-store` (encrypted by the Android Keystore / iOS Keychain) and **never leaves the device**.
4. The **public key** is registered with the server and bound to `operator_id` + `device_id`.
5. If a phone is lost, the supervisor **revokes** the key; records signed after the revocation time are rejected.

#### Login

- **Online:** Officer ID + PIN → server issues a short-lived **JWT** + refresh token.
- **Offline:** PIN verified against a locally stored salted hash (PBKDF2); sessions time out after inactivity.
- Rate-limited PIN attempts with lockout.

---

### 9.9 Searchable Log & Command Dashboard

#### On the phone — Field Test Console

- Summary tiles: **Tests logged**, **Positive**, **Negative** (and Inconclusive)
- **Start a new test** — capture, classify and record in under a minute
- **Recent tests** list with record ID, location, date/time and result badge
- Search and filters: date range, kit type, result, sync status, free-text on location/record ID
- Tap a test → **Record detail** with image, result, metadata, *"Tamper-evident record generated"* badge, and certificate

#### On the web — Command Dashboard (Supervisors)

- **Map view (Leaflet):** every test as a pin — 🔴 positive, 🟢 negative, ⚪ inconclusive
- **Filters:** date range, officer, kit, result, region, verification status
- **Spatial queries (PostGIS):** e.g. *"all positives within 10 km of this checkpoint in the last 30 days"*
- **Record drawer:** full details + live verification result
- **Exports:** PDF and CSV reports for case files and reviews
- **Alerts:** rejected records, unusual inconclusive rates per device (possible camera or kit issue), revoked devices attempting to sync

---

## 🗄️ Data Model & Database Schema

```mermaid
erDiagram
    OFFICERS ||--o{ DEVICES : "enrolled on"
    OFFICERS ||--o{ TEST_RECORDS : "conducts"
    DEVICES ||--o{ TEST_RECORDS : "signs"
    ANCHORS ||--o{ TEST_RECORDS : "anchors"
    TEST_RECORDS ||--o{ ANNOTATIONS : "annotated by"

    OFFICERS {
        text operator_id PK
        text name
        text role
        text jurisdiction
        text pin_hash
    }
    DEVICES {
        text device_id PK
        text operator_id FK
        text public_key_pem
        timestamptz enrolled_at
        timestamptz revoked_at
    }
    TEST_RECORDS {
        text record_id PK
        text device_id FK
        bigint device_seq
        text operator_id FK
        text kit_type
        timestamptz captured_at
        geography location
        text image_sha256
        text result_label
        numeric confidence
        jsonb payload
        text signature
        text prev_record_hash
        text record_hash
        text sync_status
        bigint anchor_id FK
    }
    ANCHORS {
        bigint anchor_id PK
        text merkle_root
        text chain_tx_hash
        timestamptz anchored_at
    }
    ANNOTATIONS {
        bigint id PK
        text record_id FK
        text author_id
        text kind
        jsonb body
        timestamptz created_at
    }
```

### `db/schema.sql` (core)

```sql
CREATE EXTENSION IF NOT EXISTS postgis;

CREATE TABLE officers (
  operator_id   TEXT PRIMARY KEY,
  name          TEXT NOT NULL,
  role          TEXT NOT NULL CHECK (role IN ('OFFICER','SUPERVISOR','LAB','ADMIN')),
  jurisdiction  TEXT,
  pin_hash      TEXT NOT NULL,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT now()
);

CREATE TABLE devices (
  device_id       TEXT PRIMARY KEY,
  operator_id     TEXT NOT NULL REFERENCES officers(operator_id),
  public_key_pem  TEXT NOT NULL,
  enrolled_at     TIMESTAMPTZ NOT NULL DEFAULT now(),
  revoked_at      TIMESTAMPTZ
);

CREATE TABLE anchors (
  anchor_id     BIGSERIAL PRIMARY KEY,
  merkle_root   TEXT NOT NULL,
  chain         TEXT NOT NULL DEFAULT 'polygon-amoy',
  chain_tx_hash TEXT,
  anchored_at   TIMESTAMPTZ
);

CREATE TABLE test_records (
  record_id         TEXT PRIMARY KEY,
  device_id         TEXT NOT NULL REFERENCES devices(device_id),
  device_seq        BIGINT NOT NULL,
  operator_id       TEXT NOT NULL REFERENCES officers(operator_id),
  kit_type          TEXT NOT NULL,
  captured_at       TIMESTAMPTZ NOT NULL,
  received_at       TIMESTAMPTZ NOT NULL DEFAULT now(),
  location          GEOGRAPHY(POINT, 4326),
  gps_accuracy_m    NUMERIC,
  gps_mocked        BOOLEAN,
  image_sha256      CHAR(64) NOT NULL,
  image_object_key  TEXT NOT NULL,
  result_label      TEXT NOT NULL CHECK (result_label IN ('POSITIVE','NEGATIVE','INCONCLUSIVE')),
  confidence        NUMERIC(5,4),
  payload           JSONB NOT NULL,          -- full canonical record
  signature         TEXT NOT NULL,
  prev_record_hash  CHAR(64) NOT NULL,
  record_hash       CHAR(64) NOT NULL UNIQUE,
  sync_status       TEXT NOT NULL DEFAULT 'SYNCED',
  anchor_id         BIGINT REFERENCES anchors(anchor_id),
  merkle_proof      JSONB,
  UNIQUE (device_id, device_seq)
);

CREATE INDEX idx_records_location ON test_records USING GIST (location);
CREATE INDEX idx_records_captured ON test_records (captured_at DESC);
CREATE INDEX idx_records_operator ON test_records (operator_id);

CREATE TABLE annotations (
  id          BIGSERIAL PRIMARY KEY,
  record_id   TEXT NOT NULL REFERENCES test_records(record_id),
  author_id   TEXT NOT NULL REFERENCES officers(operator_id),
  kind        TEXT NOT NULL,   -- e.g. 'LAB_CONFIRMATION', 'NOTE'
  body        JSONB NOT NULL,
  created_at  TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Append-only enforcement: evidence rows can never be changed or removed.
CREATE OR REPLACE FUNCTION forbid_mutation() RETURNS trigger AS $$
BEGIN
  RAISE EXCEPTION 'test_records is append-only';
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER no_delete BEFORE DELETE ON test_records
  FOR EACH ROW EXECUTE FUNCTION forbid_mutation();

-- Only anchoring columns may be filled in after insert.
CREATE OR REPLACE FUNCTION allow_only_anchor_update() RETURNS trigger AS $$
BEGIN
  IF (NEW.record_id, NEW.payload, NEW.signature, NEW.record_hash, NEW.image_sha256)
     IS DISTINCT FROM
     (OLD.record_id, OLD.payload, OLD.signature, OLD.record_hash, OLD.image_sha256) THEN
    RAISE EXCEPTION 'evidence fields are immutable';
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER immutable_evidence BEFORE UPDATE ON test_records
  FOR EACH ROW EXECUTE FUNCTION allow_only_anchor_update();
```

---

## 🔌 API Reference

Base URL: `/api/v1` · Auth: `Authorization: Bearer <JWT>` unless marked public.

| Method | Endpoint | Role | Description |
|---|---|---|---|
| `POST` | `/auth/login` | Public | Officer ID + PIN → JWT + refresh token |
| `POST` | `/auth/refresh` | Any | Refresh access token |
| `POST` | `/devices/enrol` | SUPERVISOR | Register a device public key to an officer |
| `POST` | `/devices/{id}/revoke` | SUPERVISOR | Revoke a lost/compromised device |
| `POST` | `/records/sync` | OFFICER | Batch upload sealed records (multipart: JSON + images) |
| `GET` | `/records` | SUPERVISOR, LAB | List/search with filters: `from`, `to`, `operator`, `kit`, `result`, `status` |
| `GET` | `/records/{id}` | SUPERVISOR, LAB | Full record detail |
| `GET` | `/records/near` | SUPERVISOR | Spatial search: `lat`, `lng`, `radius_km` |
| `POST` | `/records/{id}/annotations` | LAB, SUPERVISOR | Add lab confirmation or note (never edits original) |
| `GET` | `/verify/{id}` | **Public** | Run all verification checks, returns status + per-check results |
| `GET` | `/stats` | SUPERVISOR | Dashboard aggregates |
| `POST` | `/anchors/run` | ADMIN | Trigger an anchoring batch (normally scheduled) |

**Example — verification response**

```json
{
  "record_id": "NCB-77521",
  "status": "VERIFIED",
  "checks": {
    "record_hash_matches_qr": true,
    "image_integrity": true,
    "signature_valid": true,
    "chain_link_valid": true,
    "sequence_continuous": true,
    "anchored": true
  },
  "anchor": {
    "chain": "polygon-amoy",
    "tx_hash": "0x…",
    "anchored_at": "2026-09-18T04:00:03Z"
  },
  "disclaimer": "PRESUMPTIVE FIELD TEST — subject to laboratory confirmatory testing."
}
```

---

## 🛡️ Security & Threat Model

| Threat | Mitigation | Residual risk |
|---|---|---|
| Photo replaced or edited after capture | Image hash inside signed record; server re-hashes stored original | — |
| Result/metadata edited (e.g. POSITIVE → NEGATIVE) | ECDSA signature over canonical record | — |
| Record deleted from history | `device_seq` gaps + broken hash chain | — |
| Fake record forged by another person | Requires the device private key stored in secure storage | Compromised/rooted device (see roadmap: hardware attestation) |
| Device clock changed to backdate a test | Store device time **and** GNSS time; server `received_at`; blockchain anchor proves "existed no later than" | Records not yet synced rely on device + GNSS time |
| GPS spoofing | Android mock-location flag, accuracy gate; flagged records highlighted on dashboard | Advanced spoofing on rooted devices |
| Photographing a photo/screen instead of a real kit | Reference card must be physically in-frame and calibrate correctly | Roadmap: screen/moiré detection |
| Stolen phone | PIN + inactivity lock, rate-limited attempts, remote key revocation | — |
| Central database tampering | Append-only triggers + public blockchain anchoring | — |
| Network interception during sync | TLS; records are already signed so content cannot be altered undetected | — |
| Privacy leakage | Only hashes on-chain; role-based access; images in private storage | — |

**Cryptographic choices**

| Purpose | Algorithm | Standard |
|---|---|---|
| Hashing | SHA-256 | FIPS 180-4 |
| Signing | ECDSA over P-256 with SHA-256 | FIPS 186-5 |
| Canonical JSON | JCS | RFC 8785 |
| PIN storage | PBKDF2-HMAC-SHA256 with salt | NIST SP 800-132 |
| Transport | TLS 1.2+ | — |

---

## 📁 Project Structure

```
true-trace/
├── apps/
│   ├── mobile/                     # React Native + Expo field app
│   │   ├── app/                    # Screens (expo-router)
│   │   │   ├── (auth)/login.tsx
│   │   │   ├── index.tsx           # Field Test Console
│   │   │   ├── capture/            # Kit select → timer → camera → result
│   │   │   ├── records/[id].tsx    # Record detail
│   │   │   └── log.tsx             # Searchable log
│   │   ├── src/
│   │   │   ├── camera/             # VisionCamera + frame processors, quality gates
│   │   │   ├── calibration/        # Card detection, homography, CCM, Lab/ΔE00
│   │   │   ├── classifier/         # TFLite loader, fusion + inconclusive rules
│   │   │   ├── crypto/             # SHA-256, ECDSA, JCS, hash chain
│   │   │   ├── db/                 # SQLite schema, repositories
│   │   │   ├── sync/               # Queue, backoff, status updates
│   │   │   ├── certificate/        # HTML template → PDF, QR
│   │   │   └── auth/               # PIN, JWT, secure-store
│   │   └── assets/models/          # *.tflite models
│   └── dashboard/                  # React + Vite command dashboard & verify portal
│       └── src/
│           ├── pages/Map.tsx
│           ├── pages/Records.tsx
│           └── pages/Verify.tsx
├── services/
│   └── api/                        # Node.js backend
│       ├── src/
│       │   ├── routes/             # auth, devices, records, verify, stats, anchors
│       │   ├── verify/             # Hash, signature, chain, sequence checks
│       │   ├── anchoring/          # Merkle tree + Polygon client
│       │   └── storage/            # Supabase storage client
│       └── tests/
├── packages/
│   └── shared/                     # Shared TS types, kit profiles, JCS, record schema
│       └── kits/
│           ├── marquis.json
│           ├── mecke.json
│           ├── mandelin.json
│           └── scotts.json
├── ml/
│   ├── notebooks/                  # Data exploration, training
│   ├── train.py                    # Training script
│   ├── export_tflite.py            # Quantise + export
│   └── data/README.md              # Dataset collection protocol
├── db/
│   ├── schema.sql
│   └── seed.ts                     # Demo officers, devices and ~15 sample records
├── docs/
│   ├── reference-card/             # Printable card (PDF) + measured Lab values
│   └── architecture.png
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 20+ and npm (or pnpm)
- Android Studio + an Android phone/emulator (Android 8+)
- Docker (for local PostgreSQL + PostGIS) **or** a Supabase project
- Python 3.10+ (only for training the ML model)

### 1. Clone

```bash
git clone https://github.com/<your-org>/true-trace.git
cd true-trace
npm install
```

### 2. Database

**Option A — Local (Docker):**

```bash
docker run -d --name truetrace-db \
  -e POSTGRES_PASSWORD=postgres -e POSTGRES_DB=truetrace \
  -p 5432:5432 postgis/postgis:16-3.4

psql postgresql://postgres:postgres@localhost:5432/truetrace -f db/schema.sql
```

**Option B — Supabase:** create a project, enable the PostGIS extension, and run `db/schema.sql` in the SQL editor.

### 3. Backend

```bash
cd services/api
cp .env.example .env
```

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/truetrace
JWT_SECRET=change-me
SUPABASE_URL=
SUPABASE_SERVICE_KEY=
STORAGE_BUCKET=test-images
POLYGON_RPC_URL=https://rpc-amoy.polygon.technology
ANCHOR_PRIVATE_KEY=          # testnet wallet only
ANCHOR_INTERVAL_MIN=60
VERIFY_BASE_URL=http://localhost:5173/v
```

```bash
npm run dev          # API on http://localhost:4000
npm run seed         # Demo officers, devices and sample records
```

### 4. Dashboard & verification portal

```bash
cd apps/dashboard
npm run dev          # http://localhost:5173
```

### 5. Mobile app

```bash
cd apps/mobile
cp .env.example .env         # set EXPO_PUBLIC_API_URL to your machine's LAN IP, e.g. http://192.168.1.10:4000
npx expo prebuild
npx expo run:android         # development build (native modules required)
```

### 6. Print the reference card

Print `docs/reference-card/true-trace-card.pdf` at **100% scale** on matte paper. Don't laminate with glossy film (causes glare).

### Demo login (seeded)

| Role | Operator ID | PIN |
|---|---|---|
| Officer | `NCB-2291` | `1234` |
| Supervisor | `NCB-SUP-01` | `1234` |

> Demo credentials exist only in seed data. Never use them outside local development.

---

## 🎬 Demo Walkthrough

A 5-minute script for judges:

1. **Airplane mode ON** — show the app is fully offline.
2. **Log in** as `NCB-2291`.
3. **New test** → select *Marquis* → tap *Reagent applied*.
4. **Point camera** at kit + reference card — capture button unlocks only when the card is detected and the image passes quality checks.
5. **Result** appears in seconds: label, confidence, observed vs expected swatches, presumptive disclaimer.
6. **Generate digital record** → show record ID, image hash, GPS, operator, *Tamper-evident record generated*.
7. **Open the PDF certificate** with QR code.
8. **Log** — record shows `PENDING`.
9. **Airplane mode OFF** — watch it change to `SYNCED`.
10. **Scan the QR** with another phone → verification portal shows ✅ `VERIFIED`.
11. **Tamper attempt (live):** change one byte of the stored image (or edit the result in the payload in a test copy) → re-verify → ❌ `TAMPERED — image integrity check failed`.
12. **Command Dashboard** — map with pins, filter by *Positive* in the last 7 days, open a record.
13. **Lighting demo:** capture the same reaction under warm light and white light → same calibrated result.

---

## 🧪 Testing & Evaluation

### Automated tests

| Area | What we test |
|---|---|
| Crypto | Known-answer tests for SHA-256; sign/verify round trips; JCS canonicalisation stability |
| Hash chain | Insert/modify/delete/reorder scenarios are always detected |
| Calibration | Synthetic images with known colour casts are corrected within target ΔE |
| Classifier | Fusion rules produce INCONCLUSIVE in every low-confidence / disagreement case |
| API | Rejects bad signatures, broken chains, sequence gaps, revoked devices; append-only triggers hold |
| Sync | Duplicate uploads, interrupted uploads, out-of-order retries |

```bash
npm test                 # all workspaces
npm run test:crypto      # crypto + chain only
```

### Classifier evaluation protocol

We report, per kit:
- **Confusion matrix** on a held-out test set collected on **devices not used in training**
- **Per-class recall and precision**
- **Inconclusive rate** (a high rate means the tool is being appropriately cautious, or that capture conditions need improving)
- **Cross-lighting consistency:** same reaction, different lighting → same result rate
- **Cross-device consistency:** same reaction, different phones → same result rate

---

## ✅ Feasibility & Viability

### Feasibility

| # | Aspect | Detail |
|---|---|---|
| 1 | **Technical feasibility** | Built on a reliable, open-source stack (React Native, TensorFlow Lite, Node.js, PostgreSQL) for smooth offline and online operation. |
| 2 | **Security & compliance** | SHA-256 hashing, ECDSA signing and blockchain anchoring for tamper-evident data integrity. |
| 3 | **Scalability** | Modular architecture supports expansion across officers, kits and regions with minimal reconfiguration — new kits are just JSON profiles + model updates. |
| 4 | **Integration** | Connects with law-enforcement and case-management systems via secure APIs; supports PDF/CSV/QR exports. |
| 5 | **Accessibility** | Mobile-first, offline-ready interface that runs on budget Android devices in field conditions. |

### Viability

| # | Aspect | Detail |
|---|---|---|
| 1 | **Offline-first reliability** | Uninterrupted testing in remote areas with poor or no connectivity. |
| 2 | **Tamper-evident integrity** | Hash chain + signatures give credible, verifiable records. |
| 3 | **Open-source stack** | No licensing costs; easy to deploy and maintain. |
| 4 | **No new hardware** | Works with existing kits and officers' phones; only a printed reference card is added. |
| 5 | **Demo excellence** | QR-verifiable certificates, confidence scores and a map dashboard make the value immediately visible. |

---

## 🌍 Impact & Benefits

| Impact | Description |
|---|---|
| 📶 **Remote accessibility** | Reliable testing in areas with poor or no connectivity |
| 🎯 **Standardisation** | Consistent, calibrated results across officers and environments |
| ⚖️ **Legal trust** | Tamper-evident records strengthen the documentary value of field tests |
| 🔍 **Transparency** | Confidence scores and colour-swatch comparisons make outcomes clear and explainable |
| ⚙️ **Operational efficiency** | Automatic GPS tagging, timestamps and secure storage streamline officer workflows |
| 🔗 **Integration** | Secure APIs connect with existing law-enforcement systems |
| 🤝 **Public confidence** | Builds trust in field testing, reducing disputes and unnecessary lab load |
| 🗺️ **Intelligence** | Aggregated, map-based data helps identify trafficking hotspots and trends |

**Benefits summary:** Offline Reliability · Transparency · Standardised Results · Legal Credibility · Operational Efficiency · Tamper-Proof Records

---

## 📚 Research & References

### Existing drug-testing approaches

| Approach | Strengths | Limitations |
|---|---|---|
| **Colorimetric reagent kits** *(what True Trace augments)* | Cheap, fast, portable, used widely in the field | Subjective visual reading; lighting-dependent; non-specific (false positives possible); no record |
| **Immunoassay** | Rapid, low-cost screening used in clinics and field kits | Class-level detection only; cross-reactivity causes false positives; high cut-offs cause false negatives; limited coverage of new synthetic drugs (NPS) |
| **Gas Chromatography–Mass Spectrometry (GC-MS)** | Gold standard for confirmation | Needs lab infrastructure, trained staff; hours to days |
| **Liquid Chromatography–Mass Spectrometry (LC-MS/MS)** | More sensitive, multi-analyte, high specificity | Expensive lab equipment; not field-deployable |
| **Alternative specimens** (hair, saliva, sweat, oral fluid) | Extend detection windows | Variable reliability; hair testing is slow and invasive |

**Where True Trace fits:** it doesn't replace any of these. It makes the **first, field-level presumptive step** objective and documented, so that samples sent for GC-MS/LC-MS confirmation arrive with a verifiable record of what was observed, where and when.

### References

**Drug testing & colour tests**
1. UNODC — *Rapid Testing Methods of Drugs of Abuse* (Manual for use by national law enforcement and narcotics laboratory personnel). unodc.org
2. National Institute of Justice — *NIJ Standard-0604.01: Color Test Reagents/Kits for Preliminary Identification of Drugs of Abuse*. nij.ojp.gov
3. National Institute on Drug Abuse (NIDA) — Drug testing overview. nida.nih.gov
4. SAMHSA — Mandatory Guidelines for Federal Workplace Drug Testing Programs. samhsa.gov
5. Mayo Clinic Proceedings — Urine drug screening: false positives and false negatives. mayoclinicproceedings.org
6. World Health Organization (WHO) — Guidance on drug testing. who.int
7. NCBI / PubMed Central — Reviews of drug-testing technologies. ncbi.nlm.nih.gov

**Colour science**

8. Sharma, G., Wu, W., Dalal, E. N. (2005). *The CIEDE2000 color-difference formula: Implementation notes, supplementary test data, and mathematical observations.* Color Research & Application, 30(1).
9. CIE — Colorimetry standards (CIELAB, D65 illuminant).

**Cryptography & integrity**

10. NIST FIPS 180-4 — Secure Hash Standard (SHA-256).
11. NIST FIPS 186-5 — Digital Signature Standard (ECDSA).
12. IETF RFC 8785 — JSON Canonicalization Scheme (JCS).
13. IETF RFC 3161 — Time-Stamp Protocol (TSP).

**Indian legal context**

14. Narcotic Drugs and Psychotropic Substances (NDPS) Act, 1985.
15. Bharatiya Sakshya Adhiniyam, 2023 — Section 63 (admissibility of electronic records; successor to Section 65B of the Indian Evidence Act, 1872).

> ⚖️ The legal references are provided for context only. Whether any record is admissible is decided by the courts; True Trace is designed to *support* documentation and chain of custody, not to guarantee admissibility.

---

## ⚠️ Limitations & Honest Disclaimers

- **Presumptive only.** Colorimetric tests are screening tests. Many substances can produce similar colours; results must always be confirmed in a laboratory.
- **The classifier is only as good as its data.** Accuracy depends on the size and diversity of the captured training dataset. Unmeasured kits or reactions will (by design) tend to return INCONCLUSIVE.
- **The reference card matters.** A faded, damaged, wet or wrongly printed card reduces calibration quality — the app detects this via the residual check but cannot fix it.
- **Device trust.** On a rooted/compromised phone, an attacker with the device key could create false records *before* sync. Frequent sync, anchoring and (roadmap) hardware-backed attested keys reduce this risk.
- **Time and location before sync** rely on the device's clock/GNSS. Anchoring gives a firm "existed no later than" guarantee only after sync.
- **Not legal advice.** True Trace supports documentation; admissibility is determined by courts under applicable law.

---

## 🗺️ Roadmap

- [ ] Hardware-backed, non-exportable signing keys with **Android Key Attestation**
- [ ] **RFC 3161** trusted timestamping as an alternative/complement to blockchain anchoring
- [ ] Screen/print re-capture detection (moiré and reflection analysis)
- [ ] Multi-language UI (Hindi and regional languages) with voice guidance
- [ ] Support for more kits via downloadable, signed kit profiles and models
- [ ] Federated/central model retraining pipeline with versioned model cards
- [ ] Chain-of-custody extension: sample sealing, transfer and lab hand-off events
- [ ] Integration adapters for state police / NCB case-management systems
- [ ] Hotspot analytics and trend reports on the dashboard
- [ ] Accessibility audit for low-literacy and colour-vision-deficient users

---

## 👥 Team

**Team Moggers** — Smart India Hackathon 2026

**Mentor(s):** `<name>`

---

## 📄 License

Released under the **MIT License** — see [`LICENSE`](LICENSE).

---

## 🙏 Acknowledgements

- Smart India Hackathon 2026 and the problem-statement owner for SIH26231
- The open-source communities behind React Native, Expo, OpenCV, TensorFlow Lite, PostgreSQL, PostGIS, Supabase, Leaflet and Polygon

---

<div align="center">

**TRUE TRACE** — *Standardised. Recorded. Verifiable.*

Made with ❤️ by **Team Moggers**

</div>
