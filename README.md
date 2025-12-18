# 🔐 B92 Quantum Key Distribution (QKD) and Comparison Between Error Correction Schemes

<p>
  <img src="https://img.shields.io/badge/Made%20with-Python-blue?style=for-the-badge&logo=python&logoColor=white" alt="Made with Python" />
  <img src="https://img.shields.io/badge/Powered%20by-Qiskit-6929C4?style=for-the-badge&logo=qiskit&logoColor=white" alt="Powered by Qiskit" />
  <img src="https://img.shields.io/badge/License-Apache%202.0-red?style=for-the-badge&logo=apache&logoColor=white" alt="License" />
  <img src="https://img.shields.io/badge/Topic-Quantum%20Cryptography-green?style=for-the-badge&logo=atom&logoColor=white" alt="Topic Quantum" />
</p>
<br />

## 📌 Overview
This repository implements and analyzes the **B92 Quantum Key Distribution (QKD) protocol** using **Qiskit**, followed by classical post‑processing routines for error correction and privacy amplification. The project compares multiple error-correction schemes, such as **Cascade**, **Hamming (15,11)**, and **LDPC**, and evaluates their impact on secure key generation under realistic noise and eavesdropping models.

The work builds upon the foundational **B92 protocol proposed by Charles H. Bennett (1992)** [PhysRevLett.68.3121](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.68.3121), extending it with modern simulation, error correction, and privacy amplification techniques.

---

## 🎯 Scope and Goals
This notebook demonstrates:
- **Quantum stage (B92 protocol)**:
  - Generation, transmission, measurement, and sifting of raw key using Qiskit simulation.

* **B92 uses two non-orthogonal quantum states:**

```math
\text{Bit 0} \;\rightarrow\; |0\rangle
```

```math
\text{Bit 1} \;\rightarrow\; |+\rangle
```

  - Bob performs measurements in Z or X basis; inconclusive results are discarded.
- **Channel model and eavesdropper**:
  - Depolarising and bit‑flip noise channels.
  - Optional intercept‑resend eavesdropper to illustrate Quantum Bit Error Rate (QBER).
- **Classical post‑processing**:
  - Error estimation (QBER).
  - Error correction:
    - **Cascade** (interactive parity‑based protocol).
    - **Hamming (15,11)** block code (single‑error correction).
    - **LDPC** iterative belief‑propagation decoding.
  - Privacy amplification using **Toeplitz hash matrices** (universal hashing).
- **Comparison metrics**:
  - **Key rate**: final secure key bits / initial signals.
  - **Residual error rate**: probability of mismatched final keys.
  - **Leakage**: bits revealed over public channel during error correction.
  - **Computation/time cost**: rough performance measurement.
- **Outputs**:
  - Plots and tables comparing performance vs channel QBER for each error‑correction method.

---

## 📖 B92 Protocol Summary
The **B92 protocol** is a simplified QKD scheme using only two non‑orthogonal quantum states. It allows Alice and Bob to share a secret key securely, even in the presence of an eavesdropper.

---

## 🔄 Protocol Steps

### **Alice’s Encoding**

* Generates random bits from {0,1}

**Encoding rule:**

```math
\text{Bit 0} \;\rightarrow\; |0\rangle
```

```math
\text{Bit 1} \;\rightarrow\; |+\rangle
```

### **Bob’s Measurement**

* Randomly chooses one of the following bases:

```math
\text{Z-basis: } |0\rangle,\; |1\rangle
```

```math
\text{X-basis: } |+\rangle,\; |-\rangle
```

### **Interpretation Rules**

```math
\text{Z-basis result = 1} \;\Rightarrow\; \text{Conclusive → Alice sent } |+\rangle \text{ (bit = 1)}
```

```math
\text{X-basis result = 1} \;\Rightarrow\; \text{Conclusive → Alice sent } |0\rangle \text{ (bit = 0)}
```

All other outcomes are **inconclusive** and discarded.

### **Sifting**

* Only **conclusive outcomes** form the sifted key.
* A subset is revealed publicly to estimate the **QBER**.

<img width="2588" height="1637" alt="Image" src="https://github.com/user-attachments/assets/deb7bc21-3321-41ef-aa1e-10fec07283b5" />

---

## 📉 QBER Calculation

The **Quantum Bit Error Rate (QBER)** quantifies the fraction of erroneous bits among the **conclusive detections**.

```math
\mathrm{QBER} = \frac{N_{\mathrm{error}}}{N_{\mathrm{conclusive}}}
```

Where:

* N_error: number of conclusive detections where Bob’s bit ≠ Alice’s bit
* N_conclusive: total number of conclusive detections


---

## 🧮 Error Correction: Algorithms Overview



### 1. Cascade

<p align="center">
<img width="700" height="376" alt="Image" src="https://github.com/user-attachments/assets/961398f5-f0be-4045-b4cb-dd0412400c63" />
</p>

- **Parity‑check protocol**:
  - Split key into blocks, exchange parities, identify mismatches, perform binary search to locate and correct errors, then do multiple passes with different interleavings.
- **Leakage**: parities revealed ~ several times the number of errors.
- **Strengths**: very low residual error, robust at low QBER.
- **Weaknesses**: high leakage, interactive communication required.

### 2. Hamming (15,11)

<p align="center">
  <img width="500" height="700" alt="Image" src="https://github.com/user-attachments/assets/a0aac5d4-a747-42e5-b418-5e28b8eb5ce9" />
</p>

- **Block code**:
  - Encode 11 data bits into 15‑bit codeword; single‑bit error correction per block.
- **Leakage**: none beyond syndrome information shared (4 bits per 15‑bit block).
- **Strengths**: simple, fast, low overhead.
- **Weaknesses**: only corrects single‑bit errors; performance degrades rapidly as QBER increases.

### 3. LDPC (Low Density Parity Check)

<p align="center">
  <img width="700" height="416" alt="Image" src="https://github.com/user-attachments/assets/29fedb9e-c06d-448b-965b-a67c9b509f1f" />
</p>

- **Iterative belief‑propagation decoder**:
  - Implemented with a small regular LDPC parity‑check matrix on the binary symmetric channel (BSC).
  - Probabilities derived from observed QBER.
- **Leakage**: low if code is well matched to channel.
- **Strengths**: scalable, efficient, good throughput.
- **Weaknesses**: requires careful code design (Tanner graphs, blocklengths).

![Image](https://github.com/user-attachments/assets/12a6e366-cfdf-4f58-b6ad-d1228c3c4e7b)

---

## 🔐 Privacy Amplification Using Toeplitz Hashing

A **Toeplitz matrix** `T` of size ( m \times n ) is fully specified by
**( m + n - 1 )** random bits (its first row and first column).

The final secret key is computed as:

```math
\text{key} = T \cdot s \pmod 2
```

where:

* `T` → Toeplitz hash matrix
* `s` → corrected key of length ( n )
* All operations are in **GF(2)** (bitwise XOR)

---

## 🔢 Choosing the Final Key Length

We conservatively choose the final privacy-amplified key length ( m ) as:

```math
m = n\bigl(1 - H_2(\text{QBER})\bigr)
    - \text{leakage}
    - \text{security\_overhead}
```

The binary entropy function is:

```math
H_2(p) = -p \log_2(p) - (1 - p)\log_2(1 - p)
```

For demonstration, we set:

```math
\text{security\_overhead} = 20\ \text{bits}
```


---

## 🧪 Comparative Experiments
We run Monte Carlo trials across different channel error rates (`p_depolar` values). For each error‑correction method we measure:
- Sifted key length.
- Leakage.
- Residual errors after correction.
- Final key length after privacy amplification.
- Whether final keys match.

### Plots
- **Final key rate** = final_key_len / N_signals vs QBER.
- **Residual error** vs QBER.
- **Leakage** vs QBER.

Multiple trials per point are averaged for stability.

---

## 📊 Interpretation
- **Cascade**: Produces near‑zero residual errors but at the cost of higher leakage and multiple rounds. Best when reliability is essential and interactive communication is acceptable.
- **Hamming (15,11)**: Simple and fast but limited to single‑bit error correction. Performance degrades quickly as QBER increases.
- **LDPC**: Achieves good performance if code is well matched to channel. Our small LDPC demonstrates moderate performance; production systems require carefully‑designed Tanner graphs and longer blocklengths.

**Which to pick?**
- For low‑to‑moderate QBER (< 2–3%) and when interactivity is acceptable, **Cascade** is robust.
- For systems requiring low leakage and high throughput, invest in well‑designed **LDPC** or long‑block **Polar codes**.
- For ultra‑simple systems, **Hamming** is fine for tiny keys but not scalable.

---

## 🏗️ System Architecture
The framework is structured into four layers:

<p align="center">
<img width="500" height="600" alt="Image" src="https://github.com/user-attachments/assets/8b9fc300-a66c-4ed6-a9e6-3ab0fb1d725b" />
</p>

1. **Quantum Layer (Qiskit)**  
   - Implements B92 protocol with realistic noise and eavesdropper models.  
   - Produces raw and sifted keys.  

2. **Error Estimation Layer**  
   - Computes QBER from test fractions of the sifted key.  
   - Provides statistical reliability for security checks.  

3. **Classical Post‑Processing Layer**  
   - Error correction via Cascade, Hamming, and LDPC.  
   - Privacy amplification using Toeplitz hashing.  
   - Outputs secure final keys with minimized leakage.  

4. **Evaluation Layer**  
   - Benchmarks key rate, residual error, leakage, and computation cost.  
   - Generates plots and tables for comparative analysis.  

---

## 🔬 Research Directions
This project opens multiple avenues for deeper exploration:
- **Advanced error correction**: Explore turbo codes, polar codes, and quantum LDPC codes.  
- **Noise models**: Extend to amplitude damping, phase damping, and realistic fiber/optical channel models.  
- **Finite‑key analysis**: Incorporate statistical bounds for small key sizes.  
- **Composable security**: Integrate modern security proofs for composable QKD.  

---

## 📜 References
#### B92 Quantum Key Distribution
- https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.68.3121
- https://en.wikipedia.org/wiki/B92_protocol
- https://tqsd.github.io/QuNetSim/examples/QKD_B92.html
- https://github.com/rafapirotto/QKD-protocols

#### Cascade Error Correction
- https://cascade-python.readthedocs.io/en/latest/protocol.html
- https://arxiv.org/pdf/2307.00576
- https://link.springer.com/content/pdf/10.1007/s12596-025-02818-0.pdf

#### Hamming Code Error Correction
- https://www.geeksforgeeks.org/computer-networks/hamming-code-in-computer-network/
- https://link.springer.com/article/10.1007/s12596-025-02818-0
- https://arxiv.org/pdf/2507.03529

#### LDPC Error Correction
- https://imt-atlantique.hal.science/hal-03337135/document
- https://tsapps.nist.gov/publication/get_pdf.cfm?pub_id=910418
- https://github.com/marcoavesani/QKD_LDPC_python
---

## 📜 License
Apache 2.0 License — free to use and adapt with attribution.
