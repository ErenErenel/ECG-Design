# ECG Instrumentation & Heart Rate Variability Analysis

An analog ECG front-end (instrumentation amplifier + filtering) built around the
**OP484** quad op-amp, together with a Python analysis of the acquired signal for
beat-by-beat heart rate and heart rate variability (HRV).

Developed for the **EE 495 ECG Lab** at Northwestern University.

## Analog Front-End

A Lead-I ECG is acquired with RA and LA as the differential inputs and RL as the
right-leg reference ground. The signal chain runs on a single OP484 (four op-amps,
±5 V supply):

- **Instrumentation amplifier (3 op-amps)** — total gain ≈ 25
  - First stage (differential): G₁ = (2R₂ + R₁)/R₁ = 5, with R₂ = 20 kΩ, R₁ = 10 kΩ
  - Second stage (difference amp): G₂ = R₆/R₄ = 5, with R₆ = 100 kΩ, R₄ = 20 kΩ
- **Gain + filtering stage (4th op-amp)** — additional gain and approximate
  1–100 Hz band-limiting (high-pass + low-pass) to suppress baseline drift and
  high-frequency interference.

Bench verification with a 10 mV differential sine drove an output near 0.25 V,
confirming the ×25 instrumentation-amp gain across the 1–1000 Hz range.

## Heart Rate & HRV Analysis

A ~1-minute Lead-I recording was digitized at **fₛ = 1000 Hz** and analyzed in Python:

- **R-peak detection** — threshold-based (750 mV) with a 0.25 s refractory period and
  recovery of missed beats from abnormally long RR intervals. **72 R-peaks** detected.
- **Beat-by-beat heart rate** — computed from RR intervals (HRᵢ = 60 / RRᵢ),
  starting around 74.6 bpm.
- **HRV spectral analysis** — the mean HR is removed and an FFT-based power spectrum
  of the HR signal shows most variability at low frequencies (≤ 2 Hz, with a bump near
  ~0.18 Hz) associated with autonomic / respiratory modulation.

## Repository Structure

```
ECG-Design/
├── ECG PCB Files/          KiCad schematic, layout, and project for the ECG board
│   ├── ECG.kicad_sch
│   ├── ECG.kicad_pcb
│   └── ECG.kicad_pro
├── EE495 ECG Lab.pdf       Lab report: instrumentation amp design + ECG acquisition
└── Heart Rate Variability Analysis.pdf
                            HR and HRV analysis of the recorded ECG
```

Open `ECG PCB Files/ECG.kicad_pro` in **KiCad 7+** to view the schematic and board.

## License

Released under the [MIT License](LICENSE).
