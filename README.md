# 🧠 Centaur Test Experiment

<p align="center">
  <b>Neurosymbolic Testing of Deep Learning Libraries using Constraint Learning and Fuzzing</b>
</p>

---

## 📌 Overview

**Centaur Test Experiment** is a research prototype exploring how **neurosymbolic techniques** can improve the robustness testing of deep learning libraries such as **PyTorch** and **TensorFlow**.

The project is based on the **Centaur framework**, which combines:

- **Rule-based constraint learning**
- **SMT solving (Z3)**
- **First-order logic constraints**
- **Automated invariant inference**
- **Symbolic input generation**
- **Tensor fuzz testing**

The main objective is to automatically generate **valid and diverse tensor inputs** for deep learning APIs and detect:

- crashes,
- invalid input handling,
- numerical inconsistencies,
- missing API constraints.

This repository contains my implementation work, experimental results, and analysis performed as part of the **Centaur Research Starter Task**.

---

# 🎯 Research Motivation

Deep learning frameworks contain thousands of APIs with complex input requirements.

For example, tensor operations may depend on:

- tensor dimensions,
- shape compatibility,
- data types,
- parameter ranges,
- broadcasting rules,
- device constraints.

Traditional fuzz testing randomly generates inputs, resulting in many invalid cases and poor exploration of meaningful edge cases.

Centaur addresses this limitation by learning **API invariants** and using symbolic reasoning to guide test generation.

---

# 🔬 Centaur Pipeline

The complete testing pipeline consists of three main stages:
