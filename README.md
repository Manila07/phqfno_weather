# PHQFNO: Hybrid Quantum-Classical Fourier Neural Operator

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/pytorch-2.0+-ee4c2c.svg)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Overview

This project investigates whether a compact parameterized quantum circuit layer can improve the Fourier Neural Operator (FNO) for predicting two-dimensional field evolution. We implement and compare three architectures from scratch in PyTorch:

1. **Classical FNO** - baseline with spectral convolution layers
2. **PHQFNO** - hybrid model with quantum-like circuit layer  
3. **FNO+MLP** - control model with classical MLP replacing quantum layer

The project encompasses quantum computing validation (Bell-state simulations), neural operator implementation, and systematic model comparison on a synthetic PDE-like dataset.

## Key Results

| Model | Test MSE | Relative L² | Parameters | Quantum Layer |
|-------|----------|-------------|------------|---------------|
| **FNO** | 0.007219 | 8.49% | 595,201 | No |
| **PHQFNO** | 0.007990 | 8.94% | 595,761 | Yes (4-qubit, 2-layer) |
| **FNO+MLP** | 0.007463 | 8.64% | 597,313 | No (MLP control) |

**Key finding:** The quantum-like layer does not degrade performance relative to the classical baseline (only 10.68% higher MSE), but also does not improve it on this synthetic task. The global-pooling bottleneck—not the circuit structure—is the dominant architectural factor.

## Methodology

### Classical FNO Architecture
- 4 spectral convolution layers with 12 Fourier modes
- 32 hidden channels, GELU activation
- Input/output: 64×64 single-channel fields

### Quantum-Like Layer
- **Qubits:** 4 (emulating parameterized quantum circuit)
- **Circuit depth:** 2 variational layers
- **Gates:** Rx, Ry rotations + CNOT entanglement
- **Implementation:** Pure PyTorch (Qiskit blocked by system policies)
- **Output:** 16-dimensional feature vector (2⁴ measurement outcomes)

### Hybrid PHQFNO
1. FNO backbone processes input field
2. Global average pooling → 32-dimensional feature vector
3. Quantum-like layer maps to 16 features
4. Projection back to spatial grid + final prediction head

## Installation

```bash
# Clone repository
git clone https://github.com/Manila07/phqfno_weather.git
cd phqfno_weather

# Create virtual environment
python -m venv .venv
.\.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # Linux/Mac

# Install dependencies
pip install -r requirements.txt
