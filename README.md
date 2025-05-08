# pykeflow

**Programmatic GitHub Actions Workflow Generator**

Pykeflow is a Python library and CLI tool that lets you define GitHub Actions workflows using Python objects, serialize them to clean YAML, and round-trip them back for validation or editing. It supports multiline commands, inline lists, and ensures compatibility with GitHub's workflow formatting requirements.

---

## ✨ Features

- 🧱 Python-first workflow creation using `Workflow`, `Job`, and `Step` classes
- 📜 Clean, readable YAML output using `ruamel.yaml`
- 🔁 Round-trip support (`from_yaml()` and `to_yaml()`)
- 🧪 CLI tool to generate reusable workflows like `tests.yml`
- 🔧 Handles YAML quirks like:
  - Multiline `run` blocks (as `|` literals)
  - `on:` as inline list (`[push, pull_request]`)
  - Preserves quotes and formatting

---

## 📦 Installation

```bash
pip install pykeflow
```

Or install from source:

```bash
git clone https://github.com/ArchooD2/pykeflow.git
cd pykeflow
pip install .
```

---

## 🧰 Usage

### CLI

Generate a test workflow with:

```bash
python -m pykeflow --tests
```

This will output `.github/workflows/tests.yml` containing:

```yaml
name: Run Tests
on: [push]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - name: Install dependencies
        run: |
          pip install pytest
          pip install .
      - name: Run tests
        run: pytest
```

---

## 🧩 Python API

```python
from pykeflow import Workflow, Job, Step

steps = [
    Step(name="Say hello", run="echo Hello"),
    Step(name="Run multiple", run="echo Hi\necho Bye")
]

job = Job(id="test", runs_on="ubuntu-latest", steps=steps)
wf = Workflow(name="My Workflow", on=["push"], jobs=[job])

print(wf.to_yaml())
wf.save(".github/workflows/my-workflow.yml")
```

---

## 📄 License

This project is licensed under the [Mozilla Public License 2.0 (MPL 2.0)](https://www.mozilla.org/en-US/MPL/2.0/).

---
