# Real-Time Audio Distortion Modeling via Reservoir Computing

> A comprehensive state-of-the-art research review evaluating low-latency, computationally efficient Reservoir Computing (RC) architectures for real-time non-linear audio signal emulation.

[![Document: PDF Available](https://img.shields.io/badge/Document-PDF%20Report-blue.svg)](Reservoir_Computing_Audio_Distortion_State_of_the_Art.pdf)
[![LaTeX: KOMA-Script](https://img.shields.io/badge/LaTeX-KOMA--Script-brightgreen.svg)](#tech-stack)
[![Academic: UNamur](https://img.shields.io/badge/Institution-University%20of%20Namur-red.svg)](#authors--acknowledgments)

---

## Executive Summary

Non-linear audio distortion emulation (such as vacuum-tube guitar amplifiers and non-linear dynamic range processing) requires capturing complex, memory-dependent non-linearities while satisfying strict real-time constraints:
* **Audio Standard:** 44.1 kHz sampling rate (processing budget of $\approx 22.67\,\mu\text{s}$ per sample).
* **Perceptual Latency:** Hard deadline strictly below $15\,\text{ms}$ to prevent human auditory delay perception.

This research paper provides a systematic literature review and comparative analysis of **Reservoir Computing (RC)** architectures. By moving beyond classical recurrent neural networks (RNNs) that suffer from costly backpropagation-through-time (BPTT), RC fixes internal recurrent weights and trains only a linear readout layer. We evaluate how modern delay-based and layered architectures overcome the classical $\mathcal{O}(N^2)$ computational bottleneck to enable real-time edge audio processing.

---

## Architecture & Key Findings

```
                            +-------------------------------------------+
                            |       Input Audio Signal u(n)             |
                            +-------------------------------------------+
                                                  |
                                                  v
                     +---------------------------------------------------------+
                     |                 Reservoir Architecture                  |
                     |                                                         |
                     |  [Classical ESN / LSM]       [Delay-Line / Cascaded]    |
                     |  - Dense internal matrix     - Time-multiplexed nodes   |
                     |  - Complexity: O(N^2)        - Complexity: O(N)         |
                     |  - Latency: Exceeds budget   - Latency: Down to 0.11 ms |
                     +---------------------------------------------------------+
                                                  |
                                                  v
                            +-------------------------------------------+
                            |          Trained Linear Readout           |
                            |       (Ridge Regression / No BPTT)        |
                            +-------------------------------------------+
                                                  |
                                                  v
                            +-------------------------------------------+
                            |      Emulated Distorted Audio y(n)        |
                            +-------------------------------------------+
```

### Evaluated Paradigms & Evolution
1. **Foundational Models (ESN & LSM):**
   * *Echo State Networks (Jaeger)* and *Liquid State Machines (Maass)* prove that random, fixed recurrent dynamics can project dynamic temporal series into linearly separable spatial representations.
   * *Limitation:* The dense recurrent weight matrix leads to $\mathcal{O}(N^2)$ algorithmic time complexity, preventing live execution at 44.1 kHz.

2. **Delay-Line Reservoir Computers (DLRC) & Cascaded Topologies:**
   * Replaces dense reservoirs with a single non-linear dynamical node coupled to a delayed feedback loop using time-multiplexing.
   * **Cascaded DLRC** and **deep Time-Delay Reservoir Computers (deepTRC)** reduce algorithmic complexity to strictly linear $\mathcal{O}(N)$ scaling with the number of virtual nodes.
   * **Key Benchmark:** Validated guitar amplifier emulation at 44.1 kHz with an algorithmic latency as low as **0.11 ms**, well within the 15 ms perceptual limit.

3. **Next-Generation Reservoir Computing (NG-RC):**
   * Employs non-linear vector autoregression (NVAR) to eliminate random matrix initialization.
   * Drastically reduces warm-up transients, accelerating startup and runtime inference for continuous time-series.

---

## Tech Stack & Specifications

* **Document Engine:** LaTeX2e (`pdflatex`, TeX Live)
* **Document Class:** KOMA-Script (`scrartcl`)
* **Bibliography Management:** BibTeX with ACM Reference Standard (`ACM-Reference-Format.bst`)
* **Core Domains:** Digital Signal Processing (DSP), Reservoir Computing, Non-linear Dynamical Systems, Time-Series Forecasting

---

## Project Structure

```
├── abstract.tex                 # Abstract and acknowledgments
├── ACM-Reference-Format.bst     # ACM bibliographic formatting style
├── images/                      # Diagrams, schematics, and benchmark plots
├── intro.tex                    # Scientific background and problem context
├── main.tex                     # Master document driver and page structure
├── methodo.tex                  # Systematic literature review methodology
├── problem_statement.tex        # Research questions and constraints formulation
├── references.bib               # Primary peer-reviewed references
├── references_web.bib           # Online and secondary references
├── Reservoir_Computing_...pdf   # Compiled, ready-to-read research report
└── state_of_art.tex             # Comprehensive comparative state-of-the-art review
```

---

## Getting Started

### Prerequisites

Ensure you have a modern TeX distribution installed with `pdflatex`, `bibtex`, and `latexmk`:

```bash
# Debian / Ubuntu
sudo apt-get install texlive-latex-extra texlive-fonts-recommended latexmk

# Fedora
sudo dnf install texlive-scheme-medium latexmk

# macOS (Homebrew)
brew install --cask mactex-no-gui
```

### Compilation

You can compile the document with a single command using `latexmk`:

```bash
latexmk -pdf main.tex
```

Alternatively, run the manual compilation pipeline:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

### Cleanup

To clean auxiliary build files while keeping the compiled document:

```bash
latexmk -c
```

---

## Authors & Acknowledgments

* **Author:** **Timothy Lefebvre**
* **Scientific Supervisor:** **Marvyn Gulina**

This state-of-the-art research review was carried out within the **Faculty of Computer Science** at the **University of Namur (UNamur)**, Belgium.
