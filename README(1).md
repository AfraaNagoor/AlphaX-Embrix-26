# Offline Acoustic Detection of Hidden Insect Infestation in Stored Grains

## Overview

Stored grain can appear healthy from the outside even when insects are active inside it. This project develops a low-cost, offline system to detect hidden insect activity by sensing the tiny mechanical vibrations produced inside stored grain.

The prototype uses two stainless-steel sensing probes at different grain depths. Piezoelectric sensors attached to the probes capture vibration signals, while an external microphone records ambient noise. The signals are processed using digital signal processing and TinyML on an ARIES IoT V2.0 controller.

The system classifies the observed condition as:

- Normal
- Insect Activity
- External Disturbance

No cloud connection is required for operation.

---

## Problem

Hidden stored-product insect infestation can be difficult to identify through visual inspection alone. Manual inspection is difficult to scale, sampling can miss localized activity, and early insect-generated vibrations may be very small.

A further challenge is that external events such as handling, impacts, machinery, and surrounding activity can also produce vibrations.

Therefore, the key challenge is:

> Detect insect-related activity inside grain while distinguishing it from normal conditions and external disturbances.

---

## Proposed Solution

The system follows this sensing and processing chain:

**Insect Activity → Grain Vibration → Stainless-Steel Probe → Piezoelectric Sensor → Signal Conditioning → ADC → Digital Signal Processing → Feature Extraction → TinyML → Classification**

Two probes provide vibration information from different grain depths. An external microphone provides an ambient reference to help identify disturbances originating outside the grain.

---

## How the System Works

### 1. Probe and Piezoelectric Sensing

Stainless-steel probes are inserted horizontally into the grain container at two different heights. When insect movement produces mechanical vibration in the grain, the vibration travels through the probe.

The piezoelectric sensor converts this mechanical vibration into an electrical signal.

### 2. Signal Conditioning

The piezo output is conditioned before ADC acquisition so that the embedded controller can measure the signal reliably.

### 3. Ambient Noise Reference

The external microphone records surrounding environmental noise. This signal is used as a reference when analysing possible external disturbances.

### 4. Digital Signal Processing

The acquired signals are filtered and divided into analysis windows. Useful time-domain and frequency-domain features are then extracted.

Example features include RMS, peak amplitude, variance, crest factor, zero-crossing rate, spectral centroid, spectral bandwidth, dominant frequency, and band energy.

### 5. TinyML Classification

A lightweight machine-learning model analyses the extracted features and classifies the event as:

**Normal | Insect Activity | External Disturbance**

### 6. Local Output

The classification result is presented locally using the OLED display, LED, and buzzer.

---
## Project Demonstration

[Watch the Project Demonstration](https://drive.google.com/file/d/1xCJO7SRnJ9L2kEtbfsUy3xUVL-je-eVf/view?usp=sharing)

## System Architecture

```text
                    STORED GRAIN
                         |
              +----------+----------+
              |                     |
        Upper Probe            Lower Probe
              |                     |
           Piezo 1                Piezo 2
              |                     |
              +----------+----------+
                         |
                 Signal Conditioning
                         |
                         v
                   ARIES IoT V2.0
                         |
              +----------+----------+
              |                     |
        ADC Acquisition        Ambient Mic
              |                     |
              +----------+----------+
                         |
                 Digital Filtering
                         |
                 Feature Extraction
                         |
                       TinyML
                         |
          +--------------+--------------+
          |              |              |
        NORMAL        INSECT       EXTERNAL
                      ACTIVITY     DISTURBANCE
                         |
                  OLED / LED / Buzzer
```

---

## Why Two Probes?

A single sensing point provides limited information. Two probes placed at different grain depths allow the system to observe vibration activity from two separate regions of the sample.

This provides simple depth-aware sensing without requiring a large number of sensors.

The two probes are intended to improve spatial information within the test container; they do not guarantee complete coverage of a large storage facility.

---

## Why an External Microphone?

Piezoelectric sensors can respond to both useful vibration and unwanted external disturbance.

For example:

```text
Insect -> Grain -> Probe -> Piezo

External impact -> Container / Grain -> Probe -> Piezo
```

The external microphone provides a reference for the surrounding acoustic environment. Comparing the signals helps the system distinguish internal activity from external disturbances and reduce false detections.

---

## Hardware

| Component | Purpose |
|---|---|
| ARIES IoT V2.0 | Embedded processing and control |
| Piezoelectric sensors | Convert mechanical vibration into electrical signals |
| Stainless-steel probes | Transfer vibration from grain to the piezo sensors |
| External microphone | Capture ambient environmental noise |
| Signal-conditioning circuit | Prepare sensor signals for ADC acquisition |
| OLED display | Display the detected condition |
| LED | Visual indication |
| Buzzer | Audible indication |
| Grain container | Holds the test grain and sensing probes |

---

## Software and Technologies

### Embedded System

- Embedded C/C++
- ADC signal acquisition
- Digital signal processing
- Feature extraction
- TinyML inference
- OLED and GPIO control

### Model Development

- Python
- NumPy
- SciPy
- Pandas
- Scikit-learn
- SoundFile
- Librosa

---

## Machine Learning Workflow

```text
Recorded Sensor Data
        |
Signal Preprocessing
        |
Windowing
        |
Feature Extraction
        |
Dataset Preparation
        |
Model Training
        |
Model Evaluation
        |
TinyML Optimization
        |
Embedded Inference
```

The A-SPID stored-product insect acoustic dataset is used during the initial research and model-development stage.

For the final prototype, recordings from the project's own probe, piezo, and ambient-microphone setup are important because probe coupling, grain packing, container geometry, sensor response, and environmental conditions can change the observed signals.

---

## Classification Classes

### Normal
No significant insect-related activity is detected.

### Insect Activity
The signal contains characteristics consistent with insect-related activity.

### External Disturbance
The event is more consistent with environmental or externally generated disturbance.

---

## Key Challenges and Mitigation

| Challenge | Approach |
|---|---|
| Weak vibration signals | Rigid probe coupling and proper signal conditioning |
| Environmental noise | Digital filtering and ambient reference |
| Variation in grain conditions | Diverse recordings and validation |
| Limited embedded resources | Lightweight features and TinyML model optimization |

---

## Project Objectives

- Detect hidden insect-related activity using vibration sensing.
- Obtain signals from two different grain depths.
- Use an ambient reference to identify external disturbances.
- Extract useful signal features using DSP.
- Classify events using TinyML.
- Perform inference locally on ARIES IoT V2.0.
- Provide a practical, low-cost prototype for stored-grain monitoring.

---

## Innovation

The project integrates:

- Dual-depth mechanical sensing
- Piezoelectric vibration detection
- External ambient-noise reference
- DSP-based feature extraction
- Offline TinyML classification
- Low-cost embedded implementation

The innovation lies in the system-level integration and practical implementation rather than claiming that acoustic insect detection itself is a new concept.

---

## Expected Impact

The system is intended to support earlier identification of hidden insect activity, reduce dependence on visual inspection, provide low-cost monitoring for small-scale storage and experimental setups, and enable offline operation where cloud connectivity is undesirable or unavailable.

The prototype is a detection aid and is not intended to replace complete warehouse inspection or certified pest-management procedures.

---

## Research Progression and References

The development of this project is supported by research spanning acoustic propagation, bioacoustic detection, low-cost sensing, signal processing, and noise-robust insect detection.

### 1997
**Hickling, Wei and Hagstrum**  
*Studies of Sound Transmission in Various Types of Stored Grain for Acoustic Detection of Insects*  
[ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0003682X96000412)

**Key contribution:** Understanding acoustic propagation and attenuation in different grains.

### 2016
**Eliopoulos, Potamitis and Kontodimas**  
*Estimation of Population Density of Stored Grain Pests via Bioacoustic Detection*  
[ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0261219416300618)

**Key contribution:** Combining acoustic sensing with machine learning for infestation-density estimation.

### 2020
**Mankin, Jetter, Rohde and Yasir**  
*Stored-Product Performance of a Low-Cost Acoustic Insect Detector System*  
[Oxford Academic](https://academic.oup.com/jee/article-abstract/113/6/3004/5910423)

**Key contribution:** Low-cost microcontroller-based acoustic insect detection.

### 2021
**Mankin et al.**  
*Automated Applications of Acoustics for Stored Product Insect Detection, Monitoring, and Management*  
[MDPI](https://www.mdpi.com/2075-4450/12/3/259)

**Key contribution:** Review of acoustic sensing, automation, signal processing, and insect monitoring.

### 2024
**Kadyrov et al.**  
*Vibro-Acoustic Signatures of Various Insects in Stored Products*  
[MDPI](https://www.mdpi.com/1424-8220/24/20/6736)

**Key contribution:** Multi-sensor vibro-acoustic characterization and signal normalization.

### 2026
**Kadyrov et al.**  
*Acoustic Detection of Insects in Stored Products in the Presence of Strong Ambient Noise*  
[MDPI](https://www.mdpi.com/1424-8220/26/5/1511)

**Key contribution:** Improved insect detection under strong ambient noise.
---

## Suggested Repository Structure

```text
Offline_Insect_Detection/
|
├── README.md
├── data/
├── scripts/
├── models/
├── firmware/
├── hardware/
├── results/
└── docs/
```

The exact structure can be expanded as the implementation grows.

---

## Project Status

**Prototype and machine-learning development in progress.**

Current development areas include sensor integration, probe construction, signal conditioning, vibration/acoustic analysis, feature extraction, baseline machine-learning development, TinyML deployment, and prototype validation.

---

## Disclaimer

This is an experimental prototype. Detection performance depends on probe coupling, grain condition, insect activity, sensor characteristics, analog circuitry, environmental noise, and training data.

A classification result should be interpreted as the output of the sensing system and should not by itself be treated as proof of a specific insect species or complete infestation coverage.

---
