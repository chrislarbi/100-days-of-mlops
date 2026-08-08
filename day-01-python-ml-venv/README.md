# Day 01 — Set Up a Python ML Virtual Environment

## The real-world problem

A data science team needs an isolated, reproducible Python environment with core ML libraries — so that every team member and every deployment runs the exact same dependency versions, avoiding “works on my machine” failures.

## How I approached it

The first decision is isolation tool: `venv` (built-in), `virtualenv` (third-party), or `conda` (heavy, manages non-Python deps). For a pure Python ML stack on a server, `venv` is the simplest — no extra install needed. The workflow is: create the venv, activate it (so `pip` points to the venv's copy), install the four required packages, then freeze the full dependency tree to `requirements.txt`. The key gotcha is making sure you're *inside* the activated venv before installing and freezing — otherwise packages go to the system Python and the requirements file captures the wrong thing.

## Key concepts

- **Virtual environment** — Isolated Python installation preventing dependency conflicts between projects
- **`venv` vs `virtualenv` vs `conda`** — Built-in vs third-party vs heavy; choice depends on whether you need non-Python system packages
- **`requirements.txt`** — The reproducibility contract; exact package versions anyone can use to recreate the environment
- **`pip freeze`** — Captures the full dependency tree including transitive dependencies, not just explicit installs
- **ML core stack** — numpy (numerical), pandas (data), scikit-learn (ML), matplotlib (visualization)

## Solution

```bash
# Navigate to working directory
cd /root/code

# Create the virtual environment named ml-env
python3 -m venv ml-env

# Activate the venv — makes pip/python point to the venv's copies
source ml-env/bin/activate

# Install the required ML libraries inside the venv
pip install numpy pandas scikit-learn matplotlib

# Freeze the full dependency tree to requirements.txt
pip freeze > /root/code/requirements.txt

# Deactivate when done
deactivate
```

## How to verify this actually works

```bash
# Verify venv directory exists
ls -d /root/code/ml-env

# Activate and check Python path
source /root/code/ml-env/bin/activate
which python
# Expected: /root/code/ml-env/bin/python

# Test all four imports
python -c "import numpy; import pandas; import sklearn; import matplotlib; print('All imports OK')"

# Verify requirements.txt contains the packages
cat /root/code/requirements.txt | grep -iE "numpy|pandas|scikit-learn|matplotlib"
```

## Common mistakes here

- **Installing before activating** — packages go to the system Python, venv stays empty
- **Freezing outside the venv** — captures system packages instead of the venv's ML libraries
- **`python` vs `python3`** — on some systems `python` points to Python 2; always use `python3`
- **Wrong requirements.txt path** — saving inside `ml-env/` instead of `/root/code/`

## What I learned

<!-- Fill this in yourself after completing the task -->
- _What I now understand about why virtual environments exist beyond just "best practice":_
- _How `pip freeze` differs from just listing what I installed explicitly:_
- _One thing I'll do differently next time:_
