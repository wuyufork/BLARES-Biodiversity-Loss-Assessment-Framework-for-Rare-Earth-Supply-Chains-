# BLARES (Biodiversity Loss Assessment Framework for Rare Earth Supply Chains)

This repository provides the **Vensim** system-dynamics model for BLARES and a **Python** implementation generated via **PySD** for reproducible runs and scenario analysis.

The materials here are intended as a supplement to the associated manuscript and enable readers to:
- open and run the original model in **Vensim** (`BLARES.mdl`);
- run the same model from **Python** using the PySD-translated model (`BLARES.py`);
- reproduce key outputs used in the paper (via `model.py` and `run_example.py`), including:
  - `Annual biodiversity loss`
  - `Biodiversity loss caused by emissions`
  - `Biodiversity loss caused by land use change`

---

## Repository contents

- `BLARES.mdl`  
  Original **Vensim** model file (source of equations and structure).

- `BLARES.py`  
  **Auto-generated** Python translation of `BLARES.mdl` created using PySD.  
  (If you edit `BLARES.mdl`, regenerate this file; see below.)

- `model.py`  
  A **human-readable wrapper** around `BLARES.py` that:
  - lists core stock–flow equations in a readable format,
  - highlights key parameters and scenario switches,
  - provides a clean API to run the model and return the key outputs used in the paper.

- `run_example.py`  
  Minimal script to run the translated model and export results to `BLARES_output.csv`.

- `requirements.txt`  
  Python dependencies for running the model with PySD.

---

## Option A — Run in Python (recommended for reproducibility)

### 1) Create a Python environment and install dependencies

```bash
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows (PowerShell)
# .\.venv\Scripts\Activate.ps1

pip install -r requirements.txt
```

### 2) Run the example script

```bash
python run_example.py
```

This writes:
- `BLARES_output.csv` — full time series of all variables saved by the run.

### 3) Run only the paper’s key outputs

```python
from model import BLARESRunner

m = BLARESRunner("BLARES.py")
df = m.run_key_outputs()
print(df.tail())
```

The returned dataframe includes the three key outputs:
- `Annual biodiversity loss`
- `Biodiversity loss caused by emissions`
- `Biodiversity loss caused by land use change`

### 4) Scenario / parameter overrides (using Vensim variable names)

You can override parameters or scenario switches by passing a dictionary whose keys are **Vensim variable names**:

```python
from model import BLARESRunner

m = BLARESRunner("BLARES.py")
df = m.run_key_outputs(params={
    "a": 0,   # example: turn off one trade component (model-specific switch)
    "b": 1,
    "c": 1,
    "REE allocation factor": 0.04
})
```

> Note: The available parameters/switches and their meanings are documented in the manuscript Supplementary Materials
> (equations and parameters tables) and summarized in `model.py`.

---

## Option B — Run in Vensim (original model)

1) Install Vensim (PLE/Pro/DSS).  
   Vensim downloads: https://vensim.com/download/

2) Open `BLARES.mdl` in Vensim.

3) Check time settings (if needed):  
   In Vensim, go to **Model Settings → Time Bounds** and ensure `INITIAL TIME`, `FINAL TIME`, and `TIME STEP` match the manuscript setup.

4) Run the simulation and inspect key outputs using Vensim graphs/tables:
   - `Annual biodiversity loss`
   - `Biodiversity loss caused by emissions`
   - `Biodiversity loss caused by land use change`

---

## Regenerating the Python translation (if you modify `BLARES.mdl`)

`BLARES.py` is generated from `BLARES.mdl` using PySD. If you change the Vensim model, regenerate the translated Python model:

```python
import pysd
model = pysd.read_vensim("BLARES.mdl")  # regenerates the translation
# PySD will create/overwrite a translated .py file next to the .mdl
```

PySD documentation (Vensim translation): https://pysd.readthedocs.io/

---

## Notes on consistency between Vensim and Python runs

- The Python results are intended to reproduce the Vensim model behavior using the translated equations.
- Small numerical differences can occur depending on solver settings and time step.
- If Vensim reports `Lookup out of bounds` warnings, it typically indicates that a lookup (graphical function)
  is being evaluated outside its defined x-range. Ensure lookup ranges cover the simulation time/input ranges.

---

## How to cite

If you use this model/code, please cite the associated manuscript and (optionally) this repository.

---
