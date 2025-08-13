# ExpLab

**ExpLab** is a lightweight experiment management and versioning tool designed for local or cloud-simulated workflows.  
It automatically versions the results of each run, organizes outputs by experiment, and provides convenient methods for logging, reading, and managing files.  

ExpLab is ideal for:
- Data science experiments
- Machine learning model runs
- Simulation workflows
- Reproducible research

---

## ✨ Key Features

- **Automatic result versioning**: Every time you run an experiment, a new versioned folder is created.
- **File version tracking**: Within a single experiment run, logged files can have their own version history.
- **Structured storage**: Keeps your project organized into **data**, **dictionary**, **results**, and **code**.
- **Logging utilities**: Easily log messages and output files.
- **Data access**: Quickly read the latest or specific versions of your data and logged files.
- **Cloud-like simulation**: Works with a consistent folder structure to mimic remote experiment storage.

---

## 📁 Folder Structure

ExpLab expects your project to follow a structure like this:

```

project\_xyz/               # Project root
├── Data/                   # Project data files
├── Dictionary/             # Data dictionary files
├── Results/                 # Experiment outputs (versioned by ExpLab)
│   ├── test/                # Experiment name = notebook/script name
│   │   ├── v1/              # Version 1 of this experiment
│   │   │   ├── output.log
│   │   │   ├── test\_image\_v1.png
│   │   │   ├── test\_result\_v1.csv
│   │   │   └── ...
│   │   ├── v2/
│   │   │   ├── output.log
│   │   │   └── ...
│   │   └── ...
├── xyz/                     # Cloned Git repo containing code
│   ├── cleaning/
│   ├── model\_evaluation/
│   ├── tuning/
│   └── ...
└── README.md

````

> **Note:** ExpLab automatically creates the experiment folder inside `Results/` using the notebook or script name.

---

## 🔄 Versioning Logic

- **Experiment versions**: Increment each time you re-run the experiment from scratch.
- **File versions**: Increment when you log the same file multiple times in the same experiment run.
- **Overwrite control**: Use `overwrite_existing=True` when creating an experiment if you want to overwrite files instead of versioning them.

---

## 🚀 Quick Start

### 1️⃣ Install
*(Once packaged)*:
```bash
pip install explab
````

Or clone the repo and use locally:

```bash
git clone https://github.com/<your-username>/explab.git
```

### 2️⃣ Import and Initialize

```python
import os, re
import experiments  # Your ExpLab module

# Get current notebook name automatically (VS Code/Jupyter)
curr_notebook = re.search('(.+).ipynb', os.path.basename(globals()['__vsc_ipynb_file__']))[1]

# Create an experiment instance
expr = experiments.experiment(curr_notebook, overwrite_existing=False)
```

---

## 📝 Logging

### Log a message

```python
expr.log("This is a logged message.")
```

### Log a file

```python
file_name_excel = expr.log_file('Test_excel_file.xlsx')
writer = pd.ExcelWriter(file_name_excel)
```

If you log the same file multiple times in one experiment run, ExpLab appends a file version (`_v2`, `_v3`, ...).

---

## 📂 Reading Data

### Read project data

```python
df = expr.read_data('my_data')  # Automatically finds the latest version
```

* No need to specify file type (`.csv` or `.xlsx`).
* Automatically uses `pandas.read_csv` or `pandas.read_excel`.

---

## 📂 Reading Logged Files

```python
result_df = expr.read_logged_file(
    'test_result',
    version='latest',
    sheet_name=0
)
```

* Reads the latest or specified version.
* Supports CSV and Excel.
* For Excel, you can pass `sheet_name`.

---

## 🌟 Integration with ProjLab

If you also use **ProjLab** (project scaffolding tool):

* **ProjLab** sets up the folder structure (`Data/`, `Results/`, `Dictionary/`, `code/`).
* **ExpLab** runs inside that structure to manage experiments and result versioning.
* Together they give you a **reproducible, version-controlled local lab environment**.

---

## 📌 Example Workflow

1. Use **ProjLab** to create a new `poc` project with `expLab` integration.
2. Work in a Jupyter notebook `train_model.ipynb`.
3. Log intermediate and final outputs via ExpLab.
4. ExpLab automatically organizes them into `Results/train_model/vX`.
5. Share only the `Results` and `Data` folders to reproduce the run elsewhere.

---

## 📜 License

MIT License — you are free to use, modify, and distribute this tool.
