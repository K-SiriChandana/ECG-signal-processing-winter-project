# Analog Circuit Design for ECG Signal Processing

An LTspice-based simulation project for designing an analog front-end that amplifies weak ECG signals while reducing common sources of noise such as baseline wander, power-line interference, and high-frequency noise.

---

## Project Overview

Electrocardiogram (ECG) signals are extremely weak, typically ranging from **0.5 mV to 5 mV**. The objective of this project is to design an analog signal conditioning circuit capable of:

- Amplifying low-amplitude ECG signals
- Rejecting common-mode noise
- Filtering baseline wander
- Suppressing power-line interference (50/60 Hz)
- Reducing high-frequency noise
- Improving the Signal-to-Noise Ratio (SNR)

The complete circuit was simulated using **LTspice**.

---

## Features

- ECG waveform generation using Piecewise Linear (PWL) source
- Instrumentation amplifier implementation
- Multi-stage amplification
- High-pass and low-pass filtering
- White noise injection for testing
- Power-line interference simulation
- FFT analysis
- Noise analysis
- Frequency response analysis

---

## ECG Signal Generation

A synthetic ECG waveform was generated using a Piecewise Linear (PWL) voltage source.

Typical ECG signal:

- Amplitude: **0.5–5 mV**
- Heart Rate: **60 BPM**
- Cycle Time: **1 second**

Example PWL waveform:

```text
PWL(
0m 0
50m 0.1
80m 0.3
100m 0.1
120m 0
140m -0.1
150m 1.0
160m -0.4
200m 0
300m 0.2
350m 0.4
400m 0.2
1000m 0
)
```

---

## Noise Sources

The following real-world noise sources were added to evaluate circuit performance.

### 1. Power-Line Interference

50 Hz sine wave:

```text
SINE(0 0.05 50)
```

Simulates AC mains interference.

---

### 2. Baseline Wander

Very low-frequency drift:

```text
SINE(0 0.2 0.1)
```

Represents patient movement and electrode motion.

---

### 3. White Noise

```text
white(0.1)
```

Introduced using a behavioral voltage source to simulate random electronic noise.

---

## Amplification Strategy

Instead of applying a very high gain in a single stage, the amplification is divided into multiple stages.

### Stage 1

Instrumentation Amplifier

Gain:

```
10–20
```

Purpose:

- Initial amplification
- High common-mode rejection
- Prevent saturation

---

### Stage 2

Operational Amplifier

Gain:

```
100–200
```

Purpose:

- Further amplify filtered ECG signal

---

## Why Multi-Stage Amplification?

Using multiple amplification stages provides several advantages:

- Improved stability
- Reduced noise amplification
- Better bandwidth
- Higher Signal-to-Noise Ratio
- Prevents early saturation
- Allows filtering between stages

---

## Power Supply

The simulation used:

```
+5V
-5V
```

Recommended supplies:

- ±5 V
- 0–5 V with virtual ground

Higher supply voltages were avoided because they:

- Increase power consumption
- Increase offset errors
- Increase thermal effects
- May degrade SNR

---

## Instrumentation Amplifier

The instrumentation amplifier was selected because it provides:

- High input impedance
- Excellent common-mode rejection
- Accurate amplification of small differential signals
- Better noise immunity

---

## Filtering

The circuit filters:

### High-Pass Filter

Removes:

- Baseline wander
- DC offset

---

### Low-Pass Filter

Removes:

- High-frequency noise
- Electronic interference

Typical ECG bandwidth:

```
0.05 Hz – 150 Hz
```

---

## Noise Analysis

Noise analysis was performed using LTspice.

Example command:

```text
.noise V(out) V1 dec 10 1 1Meg
```

Independent noise contributions were analyzed separately before combining results.

---

## FFT Analysis

FFT analysis was used to compare:

- Signal before amplification
- Signal after amplification and filtering

### Observations

Before filtering:

- Higher noise floor
- Strong high-frequency components
- More interference

After filtering:

- Reduced noise floor
- Improved frequency response
- Better suppression of frequencies outside the ECG band
- Cleaner output spectrum

---

## Gain Calculations

### Non-Inverting Amplifier

Gain equation:

```text
Gain = 1 + (Rf / Rg)
```

Example:

```
Gain = 100

Rg = 1 kΩ

Rf = 99 kΩ
```

---

### Inverting Amplifier

Gain equation:

```text
Gain = -Rf / Rin
```

Example:

```
Rin = 1 kΩ

Rf = 100 kΩ
```

---

## Notch Filter

A notch filter was designed to remove **50 Hz power-line interference**.

Frequency equation:

```text
f = 1 / (2πRC)
```

---

## LTspice Simulation

The following analyses were performed:

- Transient Analysis
- FFT Analysis
- Noise Analysis
- Frequency Response
- Signal Comparison at Different Nodes

Simulation settings:

- Stop Time: **2 seconds**
- Time Step: **1 ms**

---

## Results

The designed analog front-end successfully:

- Amplified weak ECG signals
- Reduced power-line interference
- Suppressed high-frequency noise
- Improved Signal-to-Noise Ratio (SNR)
- Produced a cleaner ECG waveform suitable for further processing

---

## Future Improvements

- Add adaptive filtering
- Implement digital signal processing
- Hardware implementation using instrumentation amplifier ICs
- Real-time ECG acquisition
- PCB implementation
- Integration with microcontrollers for biomedical monitoring

---

## Tools Used

- LTspice
- Operational Amplifiers
- Instrumentation Amplifier
- Behavioral Voltage Sources
- FFT Analysis
- Noise Analysis

---

## Project Structure

```
ECG-Signal-Processing/
│
├── README.md
├── Circuit/
│   ├── ECG_Circuit.asc
│   └── Components.lib
│
├── Simulations/
│   ├── Transient/
│   ├── FFT/
│   ├── Noise/
│   └── Frequency_Response/
│
├── Images/
│   ├── Circuit.png
│   ├── FFT_Before.png
│   ├── FFT_After.png
│   └── Output_Waveforms.png
│
└── Documentation/
    └── Project_Report.pdf
```

---

## License

This project is intended for educational and research purposes in analog circuit design and biomedical signal processing.
