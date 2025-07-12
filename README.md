# QISEA — Quantum Instruction Set Efficiency Analyzer (EDA Edition)

Welcome to QISEA — a data science-focused project that dives deep into quantum circuits and how different compilers handle them.
No fancy machine learning here — just pure exploratory data analysis, clean visualizations, and rule-based insights into how efficient or bloated your quantum circuits really are.

---

## What’s the Goal?

To analyze, visualize, and compare quantum circuits written in QASM, Cirq, and Quil — figuring out:

* How efficient different compilers are
* Which circuits are too heavy
* What patterns we can find in gate usage
* Trigger smart alerts based on rules (like too many CX gates)

---

## What You’ll Build

By the end of this, you’ll have:

* A dataset of quantum circuits with important metrics like depth, gates, and width
* A suite of visualizations and dashboards to explore those circuits
* A real-time alert plugin that warns you about inefficient designs
* A compiler performance report with suggestions based on pure EDA

---

## Tools and Tech Stack

You’ll need:

* Python 3.10 or higher
* Jupyter Notebook or VSCode
* Dash for interactive dashboards
* Libraries:

  ```bash
  pip install qiskit cirq pyquil pandas numpy matplotlib seaborn plotly dash networkx
  ```

---

## Project Structure (Folders Explained)

```
qisea/
├── data/             <- raw and processed quantum circuit files
├── extractor/        <- scripts that analyze QASM, Cirq, and Quil files
├── notebooks/        <- Jupyter notebooks for EDA and comparisons
├── visualizations/   <- output plots, graphs, and network diagrams
├── dashboards/       <- Dash-based interactive dashboard app
├── plugins/          <- VSCode alert plugin (optional)
├── reports/          <- final analysis reports in PDF or Markdown
└── main.py           <- CLI launcher (if needed)
```

---

## Step-by-Step Walkthrough

### 1. Generate Circuits

Script: `extractor/generate_circuits.py`

* Uses Qiskit, Cirq, and PyQuil to generate standard circuits like GHZ
* Saves information such as:

  * Gate counts
  * Depth
  * Qubit count
  * Type of gates used
* Output saved to: `data/compiled_circuits.csv`

Example:

```python
from qiskit import QuantumCircuit

def generate_ghz(n=3):
    qc = QuantumCircuit(n)
    qc.h(0)
    for i in range(n-1):
        qc.cx(i, i+1)
    return qc
```

---

### 2. Extract Features and Metrics

Script: `extractor/extract_features.py`

It extracts important metrics like:

* Total gate count
* Depth
* Count of specific gates (CX, T, H)
* Instruction variability and reuse

Example:

```python
def compute_features(qc):
    return {
        "total_gates": qc.size(),
        "depth": qc.depth(),
        "num_qubits": qc.num_qubits,
        "cx_count": qc.count_ops().get('cx', 0),
    }
```

---

### 3. Exploratory Data Analysis

Notebook: `notebooks/eda_quantum_metrics.ipynb`

Uses Seaborn, Matplotlib, and Plotly to create:

* Histograms of gate usage
* Scatter plots of depth vs qubit count
* Heatmaps for gate types
* Sankey diagrams for gate flow
* Bar plots for comparisons

Insight examples:

* Cirq circuits tend to have higher depth
* Qiskit uses narrower but deeper circuits

---

### 4. Compiler Efficiency Comparison

Notebook: `notebooks/compiler_comparison.ipynb`

You’ll:

* Compile the same circuit with Qiskit, Cirq, and PyQuil
* Analyze changes in:

  * Depth
  * CX gate count
  * Total gate count
  * Width
* Visualize with:

  * Bar plots
  * Radar charts
  * Efficiency score

Sample formula:
`Efficiency = (Depth * CX_Count) / Num_Qubits`

---

### 5. Visualize Gate Flow as a Graph

Notebook: `visualizations/gate_flow_network.ipynb`

Use NetworkX to build circuit flow graphs:

* Nodes are gates
* Edges show dependencies
* Gates are colored based on type

Example:

```python
import networkx as nx
nx.draw(G, with_labels=True)
```

Helps to identify circuit bottlenecks and complexity.

---

### 6. Add Rule-Based Real-Time Alerts

Folder: `plugins/vscode-alert`

This plugin issues alerts during circuit design. Examples:

* If CX gates are more than twice the number of qubits
* If the circuit is deeper than five times the qubit count
* If more than 50 percent of gates are two-qubit types

Example logic:

```ts
if (cx_count > qubit_count * 2) {
  showAlert("High CX density — may reduce fidelity.")
}
```

---

### 7. Build the Dashboard

Script: `dashboards/qisea_dashboard.py`

This Dash web app gives an interactive view with:

* A circuit selector
* Feature summary tab
* Gate composition pie chart
* Compiler comparison plots
* Circuit graph view

Run with:

```bash
python dashboards/qisea_dashboard.py
```

---

### 8. Generate the Final Report

File: `reports/final_analysis.pdf`

The report includes:

* Best compiler for different circuit types
* Which circuits are inefficient
* Patterns in gate usage
* Actionable recommendations for quantum developers

---

## Final Outcome

By the end of this project, you’ll have:

* A data-driven platform for quantum circuit analysis
* An interactive dashboard
* A rule-based real-time alert plugin
* No machine learning used — just clear, interpretable results

---

## What Next

| If you want to...     | Then do this...                                  |
| --------------------- | ------------------------------------------------ |
| Add machine learning  | Use a classifier to automate inefficiency alerts |
| Use more circuits     | Import benchmarks from open datasets             |
| Try on real hardware  | Connect with IBM Qiskit Runtime or AWS Braket    |
| Publish the dashboard | Deploy using Render, Netlify, or Heroku          |

---

## About This Project

This project was built as part of a Quantum Data Science minor by Ayush Jha.
It focuses on clarity, real insights, and hands-on exploratory analysis — no black boxes, just code and logic.

Have a good day bro :))
