# REVERSE_ENGINEERING_REPORT — EEG-model-personalization

## A. General diagnosis

### Architecture identified
Machine learning project for EEG (electroencephalogram) model personalization. Architecture:

- `models.py`: CNN 1D model definitions (CNN_1D_model, CNN_1D_BN_model, KAN_CNN_1D_model, KAN_CNN_1D_BN_model)
- `data_loader.py`: functions `read_data()` and `load_data()` for loading .npy data and creating sliding windows
- `data_recorder.py`: script for recording EEG data via LSL (Lab Streaming Layer)
- `Personalization.ipynb`: notebook for individual training, fine-tuning, and transfer learning
- `Results.ipynb`: notebook for results analysis (summary, LaTeX, Pareto chart)
- `.gitmodules`: references tfkan as submodule (https://github.com/ZPZhou-lab/tfkan/)
- `.gitignore`: standard Python + Jupyter template

### Technologies and dependencies
- Python (version not declared)
- Dependencies inferred: tensorflow==2.5.0, numpy==1.19.5, scikit-learn, matplotlib, jupyter, tfkan
- No `requirements.txt`
- No Pipfile

### Maintenance state
Last commit: `f94ba6f Update .gitmodules`. Default branch: main. Repository: https://github.com/fipu-lab/EEG-model-personalization.

### Existing testing mechanisms
No automated tests found.

### Investigation limitations
- TensorFlow not installed; notebooks cannot be executed
- EEG data (.npy) not in repository (ignored via data/ in .gitignore)
- tfkan submodule not initialized in clone
- REA 6.7.0 not operational; static analysis only

## B. Problems found

| ID | Title | Severity | Status | Location |
|----|-------|----------|--------|----------|
| REA-eeg-001 | Missing requirements.txt | P3 | CONFIRMED | repo root |
| REA-eeg-002 | Missing seed parameter for reproducibility | P3 | CONFIRMED | `data_loader.py` |
| REA-eeg-003 | Notebook clones tfkan via git inside cell | P2 | PROBABLE | `Personalization.ipynb:7-8` |
| REA-eeg-004 | data_recorder.py uses interactive input() | P3 | CONFIRMED | `data_recorder.py:7` |

### Details of confirmed problems

**REA-eeg-001 (P3, CONFIRMED)**: No `requirements.txt` exists in the repository. Dependencies can only be inferred from imports and a notebook comment (`# !pip install numpy==1.19.5 tensorflow==2.5.0 keras==2.5.0rc0 scikit-learn==1.1.2 matplotlib`).

**REA-eeg-002 (P3, CONFIRMED)**: `data_loader.py` has no `seed` parameter to control `np.random.shuffle()` randomness in `load_data()`, making results non-reproducible across runs.

Evidence: `data_loader.py:36-38` uses `np.random.shuffle(ind)` without seed.

**REA-eeg-004 (P3, CONFIRMED)**: `data_recorder.py:7` uses `input("Upisi direktorij osoba/akcija: ")` to capture the object name, making the script interactive and impossible to run in automated mode.

### Details of the probable problem

**REA-eeg-003 (P2, PROBABLE)**: `Personalization.ipynb:7-8` clones the tfkan repository via `git clone` inside a notebook cell. This breaks reproducibility if the remote repository is unavailable or changes.

## C. Recurring problems
No recurring public issues found.

## D. Undocumented errors
All items in section B are problems not previously documented.

## E. Performance
Training uses EarlyStopping with patience of 4 epochs and batches of 64. No performance-critical operations besides ML training itself.

## F. Technical improvements
- Add requirements.txt with pinned versions (REA-eeg-001, implemented)
- Add seed parameter for reproducibility (REA-eeg-002, implemented)
- Replace git clone in notebook with submodule or pinned dependency (REA-eeg-003)
- Replace input() with CLI argument or environment variable (REA-eeg-004)

## G. Execution plan
Independent corrections:
1. REA-eeg-001, REA-eeg-002 — requirements.txt and seed (low risk, implemented)
2. REA-eeg-003 — tfkan dependency (medium effort)
3. REA-eeg-004 — interactive input() (low effort)

## H. Expected outcome
Reproducible environment, deterministic results with seed, non-interactive data recorder execution.

**Tools used:** git, gh, PowerShell, Python 3.12 (py_compile), static notebook inspection.  
**Limitation:** TensorFlow not installed; notebooks not executed.  
**Environment:** Windows, PowerShell 5.1, `C:\Users\User\audit-repos\EEG-model-personalization` (clone of upstream, branch main).
