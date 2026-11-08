# CORRECTIONS.md — EEG-model-personalization

Project: fipu-lab/EEG-model-personalization
Base branch analyzed: main
Destination fork: https://github.com/admeduardovieira/EEG-model-personalization

## Corrections implemented

| ID | Title | Severity | Status | Risk | Files involved |
|----|-------|----------|--------|------|----------------|
| REA-eeg-001 | Missing requirements.txt | P3 | IMPLEMENTED | Low | `requirements.txt` (new) |
| REA-eeg-002 | No seed parameter for reproducibility | P3 | IMPLEMENTED | Low | `data_loader.py` |

## Details of implemented corrections

### REA-eeg-001 — Missing requirements.txt
**Problem:** No `requirements.txt` exists. Dependencies only inferable from notebook comments and imports.
**Fix:** Added `requirements.txt` with pinned versions: tensorflow==2.5.0, numpy==1.19.5, scikit-learn==1.1.2, matplotlib, jupyter, tfkan.
**Validation:** File exists with expected dependencies.

### REA-eeg-002 — No seed parameter for reproducibility
**Problem:** `data_loader.py` uses `np.random.shuffle()` without seed, making results non-reproducible.
**Fix:** Added optional `seed` parameter to `read_data()` and `load_data()` that calls `np.random.seed(seed)` before shuffling.
**Validation:** `py_compile` passes for data_loader.py.

## Corrections not implemented (require more investigation)

### REA-eeg-003 — Notebook clones tfkan via git
Not implemented: replacing `git clone` in the notebook cell requires understanding the intended tfkan integration. Suggested: use the git submodule already declared in `.gitmodules` or pin a specific tfkan release.

### REA-eeg-004 — data_recorder.py uses input()
Not implemented: replacing `input()` with CLI arguments or environment variables requires understanding the intended usage flow. Suggested: add `argparse` or use `os.environ`.

## Publication plan
- Fork: `admeduardovieira/EEG-model-personalization` (created).
- Branch `fix/rea-eeg-001-002-repro` for corrections.
- Commit identity: `admeduardovieira <admeduardovieira@users.noreply.github.com>`.
- PR #1: https://github.com/fipu-lab/EEG-model-personalization/pull/1 (open, base main).

Переклад технічний, виконаний автоматизовано; рекомендується перегляд носієм мови перед використанням якнормативного посилання.
