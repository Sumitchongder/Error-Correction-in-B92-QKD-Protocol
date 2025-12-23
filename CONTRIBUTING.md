# Contributing to B92 QKD and Error Correction Comparison

Thank you for your interest in contributing to this project 🔐⚛️  
Contributions are welcome from researchers, students, and practitioners in quantum computing, cryptography, and information theory.

---

## 🧪 Types of Contributions

We welcome:

- Bug fixes and performance improvements
- New error-correction schemes (e.g., Polar codes, Turbo codes)
- Improved noise or eavesdropper models
- Visualization and benchmarking enhancements
- Documentation improvements
- Reproducibility or validation experiments

---

## 🛠 Development Setup

### Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate   # macOS / Linux
venv\Scripts\activate      # Windows
````

### Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 📐 Coding Standards

Please follow these guidelines:

* Follow **PEP 8** for Python
* Write clear, modular, and well-documented code
* Add docstrings to all public functions and classes
* Avoid hard-coded constants (use parameters/configs)
* Ensure numerical experiments are reproducible
* Use deterministic random seeds where applicable

---

## 🧪 Experimental Contributions

If adding or modifying experiments:

* Clearly describe:

  * Channel model
  * Noise parameters
  * QBER estimation method
  * Error-correction assumptions
* Report metrics such as:

  * Key rate
  * Leakage
  * Residual error
* Include plots with captions
* Validate results across multiple Monte Carlo trials

---

## 🔄 Pull Request Process

1. Create a new branch:

   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Commit with descriptive messages:

   ```bash
   git commit -m "Add LDPC belief propagation decoder"
   ```

3. Push to your fork and open a Pull Request

4. In your PR, include:

   * Summary of changes
   * Motivation and scientific relevance
   * Any assumptions or limitations
   * References (if applicable)

---

## 📜 Licensing

By contributing, you agree that your contributions will be licensed under the **Apache 2.0 License**, consistent with this project.

---

## 🙏 Acknowledgement

All meaningful contributions will be acknowledged.
Substantial research contributions may be cited in future reports, papers, or releases.

Thank you for advancing secure quantum communication 🚀

