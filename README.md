# 🧠 Centaur Test Experiment

<p align="center">
  <b>
    Neurosymbolic Testing of Deep Learning Libraries using Constraint Learning and Fuzzing
  </b>
</p>

---

## 📌 Overview

**Centaur Test Experiment** is a research prototype exploring how **neurosymbolic techniques** can improve the robustness testing of deep learning libraries such as **PyTorch** and **TensorFlow**.

This project is based on the **Centaur framework**, which combines:

- Rule-based constraint learning
- SMT solving using **Z3**
- First-order logic constraints
- Automated invariant inference
- Symbolic input generation
- Tensor fuzz testing

The objective is to automatically generate **valid and diverse tensor inputs** for deep learning APIs and detect:

- crashes,
- invalid input handling,
- numerical inconsistencies,
- missing API constraints.

This repository contains my work, experiments, results, and analysis performed as part of the **Centaur Research Starter Task**.

---

# 🎯 Research Motivation

Deep learning frameworks provide thousands of APIs with complex input requirements.

Tensor operations often depend on:

- Tensor dimensions
- Shape compatibility
- Data types
- Parameter ranges
- Broadcasting rules
- Device constraints

Traditional fuzz testing relies on random input generation. However, random generation often produces invalid inputs and explores only a limited subset of possible behaviors.

Centaur improves this process by learning **API invariants** and using symbolic reasoning to generate meaningful test cases.

---

# 🔬 Centaur Pipeline

The complete Centaur workflow contains three main stages:

```text
                         API Rules
                             |
                             v
              +----------------------------+
              |   Invariant Inference      |
              |  (Constraint Learning)     |
              +----------------------------+
                             |
                             v
              +----------------------------+
              |    Z3 Abstract Generator   |
              |       (SMT Solving)        |
              +----------------------------+
                             |
                             v
              +----------------------------+
              | Concrete Input Generation  |
              |        + Fuzzing            |
              +----------------------------+
                             |
                             v
              +----------------------------+
              | Crash Detection and        |
              | Validity Analysis          |
              +----------------------------+
```

---

# 📂 Repository Structure

```text
centaur-test-exp/
│
├── README.md
├── Task1Results/
│   └── review-centaur-icse.pdf
└── Task2Results/
    └── TorchResults.csv
```

---

# 📁 Task 1: Paper Review

## Location

```
Task1Results/
```

## Description

The first task consisted of reading and reviewing:

> **Centaur: Testing Deep Learning Libraries via Neurosymbolic Constraint Learning**

The review covers:

- Research motivation
- Problem definition
- Technical contribution
- Methodology
- Strengths
- Weaknesses
- Limitations
- Possible improvements

## Files

```
review-centaur-icse.pdf
```

---

# 📁 Task 2: Experimental Evaluation

## Location

```
Task2Results/
```

The second task consisted of running the Centaur pipeline on multiple PyTorch APIs.

The workflow contains three steps:

---

## Step 1 — Invariant Inference

Centaur learns API-specific constraints from predefined rules.

Example:

```bash
python -m learner.invariant_inference \
torch.abs 30 0 torch 1 42
```

Generated output:

```text
invariants_torch/
└── torch.abs/
```

The output contains the inferred invariants for the selected API.

---

## Step 2 — Abstract Input Generation

The inferred invariants are converted into SMT constraints.

The Z3 solver generates abstract tensor models satisfying these constraints.

Example:

```bash
python -m generator.z3 \
torch.abs 30 100 torch 42 0
```

Generated output:

```text
corpus_torch/
└── torch_abs/
```

---

## Step 3 — Concrete Input Generation and Fuzzing

The generated abstract models are converted into real tensor inputs and executed against the target API.

Example:

```bash
python -m generator.harness_z3 \
torch.abs 100 1000 torch 42
```

Generated artifacts:

- Tensor inputs
- Execution results
- Crash reports
- Invalid executions

---

# 📊 Experimental Results

Results are stored in:

```
Task2Results/TorchResults.csv
```

The CSV contains:

| Column | Description |
|---|---|
| API Name | Tested PyTorch API |
| Rules | Number of predefined rules |
| Invariants | Number of inferred invariants |
| Abstract Inputs | Number of generated symbolic models |
| Concrete Inputs | Number of generated tensors |
| Crashes | Number of detected crashes |
| Validity Rate | Ratio of valid executions |

Example:

```csv
API,Rules,Invariants,Abstract Inputs,Concrete Inputs,Crashes,Validity Rate
torch.abs,...
torch.reshape,...
torch.transpose,...
```

---

# 📁 Task 3: Invariant Analysis

## Location

```
Task3Results/
```

## Status

⚠️ **Not completed**

The objective was to analyze the quality of generated invariants.

For each API, the analysis should include:

- Natural language interpretation of invariants
- Comparison with official API documentation
- Precision evaluation
- Constraint refinement
- Additional invariants to improve validity rate

Expected file:

```text
Task3Results/
└── analysis.md
```

---

# 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Python 3.12 | Main implementation language |
| PyTorch | Deep learning API testing |
| TensorFlow | Deep learning framework testing |
| Z3 SMT Solver | Constraint solving |
| Conda | Environment management |
| First Order Logic | Constraint representation |
| EBNF Grammar | Rule representation |

---

# ⚙️ Environment Setup

## Create Conda Environment

```bash
conda create -n cent -y python=3.12

conda activate cent
```

---

## Install Dependencies

```bash
pip install -r requirements_coverage.txt
```

---

# 🚀 Running Experiments

Clone the original Centaur repository:

```bash
git clone https://github.com/ncsu-swat/centaur
```

Navigate into the repository:

```bash
cd centaur
```

---

## 1. Invariant Inference

```bash
python -m learner.invariant_inference \
<API_NAME> 30 0 torch 1 42
```

---

## 2. Generate Abstract Inputs

```bash
python -m generator.z3 \
<API_NAME> 30 100 torch 42 0
```

---

## 3. Run Fuzzing

```bash
python -m generator.harness_z3 \
<API_NAME> 100 1000 torch 42
```

---

# 📚 Key Concepts

## SMT Solving

**Satisfiability Modulo Theories (SMT)** solves logical constraints over mathematical structures.

In Centaur:

1. API requirements are transformed into logical constraints.
2. Z3 searches for satisfying assignments.
3. Valid tensor configurations are generated.

---

## Invariants

An invariant represents a condition that should always hold for valid API execution.

Example:

```python
input_A.shape[-1] == input_B.shape[-2]
```

This constraint ensures matrix multiplication compatibility.

---

## Fuzz Testing

Fuzz testing automatically generates inputs to discover:

- crashes,
- invalid behaviors,
- edge cases.

Unlike random fuzzing, Centaur generates **constraint-aware inputs** using learned invariants.

---

# 📈 Research Insights

Through this experiment, I explored:

- How symbolic reasoning complements deep learning testing.
- How API specifications can become executable constraints.
- How SMT solvers can guide automated test generation.
- The trade-off between constraint precision and input diversity.

---

# ⚠️ Limitations

Current limitations:

- Task 3 invariant analysis was not completed.
- Experiments were limited to selected PyTorch APIs.
- Some APIs require additional manually defined constraints.
- Generated invariants may require further refinement.

---

# 📌 Future Work

Possible improvements:

- Complete invariant precision analysis.
- Test additional PyTorch and TensorFlow APIs.
- Automatically compare invariants with API documentation.
- Use LLMs for automated rule discovery.
- Improve invariant refinement methods.
- Integrate coverage feedback into generation.

---

# 🙏 Acknowledgements

This work was completed as part of the **Centaur Research Starter Task**.

Special thanks to:

- **Dr. Saikat Dutta**
- **NCSU SWAT Lab**
- The authors of the Centaur framework

for providing the opportunity to explore neurosymbolic approaches for deep learning library testing.

---

# 📄 References

- Centaur: Testing Deep Learning Libraries via Neurosymbolic Constraint Learning
- Z3 SMT Solver Documentation
- PyTorch Documentation
- TensorFlow Documentation
