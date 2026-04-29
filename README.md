# Equine & Livestock Diagnostic Pipeline: AcTBeCalf Analysis

## 📌 Project Overview
This project focuses on building a robust data-cleaning and feature-engineering pipeline for high-frequency (25Hz) accelerometer data collected from 30 pre-weaned calves. 

The goal is to provide **decision-ready data** for a diagnostic technology startup, transforming raw G-force readings into standardized "Health Reports" that veterinarians can use to detect illness and stress early.

## 📊 The Dataset
*   **Source:** AcTBeCalf (Teagasc Moorepark Research Farm).
*   **Volume:** ~2.46 million records.
*   **Frequency:** 25 Hz (25 readings per second).
*   **Variables:** 3-axis acceleration (accX, accY, accZ), CalfID, DateTime, and annotated Behavior.

## 🛠️ Data Cleaning & Engineering Steps

### 1. Temporal Integrity (Gap Analysis)
*   **The Problem:** Sensors in farm environments suffer from signal loss.
*   **The Fix:** Identified 686,562 gaps in the data stream. Implemented **Sessionization** where any gap >1 hour creates a new `unique_session_id` to prevent mathematical errors across dates.

### 2. Physics-Based Feature Engineering
To make the data independent of sensor orientation (collar slip), the following were calculated:
*   **Vector Magnitude ($SMV$):** $\sqrt{x^2 + y^2 + z^2}$
*   **Energy Expenditure:** $|SMV - 1.0g|$ (removing static gravity to isolate dynamic movement).

### 3. Behavioral Mapping
Simplified 50 detailed research behaviors into 4 Diagnostic Categories:
*   **Active:** Normal movement, feeding, and social play.
*   **Not Active:** Rest, stationary maintenance, and sleep.
*   **Abnormal:** Stereotypies or behaviors indicating redirected motivation/stress.
*   **Other:** Ambiguous markers or physiological functions.

### 4. Signal Smoothing
Applied rolling mean filters (Window size 10 & 20) to extract cleaner behavioral signatures:
*   **Active:** Shows sustained peaks (>0.05 energy).
*   **Abnormal:** Displays a "Jigsaw" pattern—repetitive, rhythmic energy indicating stress-induced loops.

## 📈 Diagnostic Findings
| Category | Energy Signature | Startup Action |
| :--- | :--- | :--- |
| **Active** | High Amplitude Bouts | Track "Vigor Score" for growth performance. |
| **Not Active** | Near-Zero Flatline | Monitor for "Lethargy" (early sign of pneumonia). |
| **Abnormal** | Rhythmic/Jigsaw | Trigger "Welfare Alert" for environmental stress. |

## 🚀 How to Use
1.  **Dependencies:** `pandas`, `numpy`, `matplotlib`, `seaborn`.
2.  **Run:** Execute the Jupyter Notebook to process raw `AcTBeCalf.csv`.
3.  **Output:** Generates `Clean_AcTBeCalf.csv` which is optimized for Machine Learning classification.

## 🎓 Conclusion
This pipeline demonstrates that by applying physics and session-based logic, we can transform "noisy" farm data into reliable clinical insights, reducing false alarms for animal owners and veterinarians.
