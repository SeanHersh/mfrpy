# Instructions for Running mfrpy Tests

## Quick Start

### 1. Install mfrpy in a virtual environment

**Important:** Create the virtual environment OUTSIDE the cloned repo to avoid file conflicts.

```bash
# Navigate to the PARENT directory of the cloned repo
cd ..

# Create the virtual environment in a separate directory
python -m venv mfrpy-env

# Activate the virtual environment
# On macOS/Linux:
source mfrpy-env/bin/activate

# On Windows:
mfrpy-env\Scripts\activate

# Now navigate back into the mfrpy repo
cd mfrpy

# Install mfrpy in development mode
pip install -e .

# This will automatically install all dependencies:
# - python-igraph
# - sympy
# - tabulate
```

**Why outside the repo?** Creating a virtual environment inside a Git repository can cause issues:
- The venv module may create files that conflict with existing repo files
- Some systems have issues with `.gitignore` handling
- It keeps the working directory clean for Git operations

### 2. Run all tests

```bash
# From the mfrpy directory
python -m unittest discover mfrpy.test -v
```

### 3. Run specific test files

```bash
# Preprocessing tests (prime, updates, expand, involution)
python -m unittest mfrpy.test.test_preprocessing -v

# MFR processing tests (get_mfrs)
python -m unittest mfrpy.test.test_mfr_processing -v

# Original update_expand tests
python -m unittest mfrpy.test.test_update_expand -v
```

### 4. Run a single test

```bash
# Example: Run just the prime function tests
python -m unittest mfrpy.test.test_preprocessing.TestPrime -v
```

---

## About test_bug_fixes.py

**Answer to your question:** `test_bug_fixes.py` was the old test file organized by bug fix number. It has been **replaced** by the new test files organized by function:

- `test_preprocessing.py` - Tests for preprocessing functions (prime, updates, expand, involution)
- `test_mfr_processing.py` - Tests for MFR computation (get_mfrs)
- `test_setup.py` - Common setup code

The old `test_bug_fixes.py` file should be removed (it's redundant). All tests from it have been moved to the new files, organized by function rather than bug fix number.

---

## Troubleshooting Common Issues

### Issue: "NameError: name '__file__' is not defined"

**Cause:** Running tests in an interactive shell (like Sublime Text's Python console) where `__file__` doesn't exist.

**Solution:** The test setup code now handles this automatically. If you still see this error:

1. Make sure you're running tests from the command line, not in an interactive shell
2. Or install mfrpy first: `pip install -e .` (then `__file__` handling isn't needed)

### Issue: "ModuleNotFoundError: No module named 'mfrpy'"

**Solution:** Install mfrpy in development mode:
```bash
pip install -e .
```

### Issue: Many tests are skipped

**Cause:** Missing dependencies (igraph, mfrpy, or examplegraphs)

**Solution:** 
```bash
# Install all dependencies
pip install -e .

# This installs:
# - python-igraph (for graph operations)
# - sympy (for symbolic math)
# - tabulate (for table formatting)
```

### Issue: Warnings about missing example graphs

**This is normal.** Some tests use example graphs from `mfrpy.examplegraphs`, which may not be available. Those tests will be skipped automatically.

---

## Test Organization

### New Structure (Current)

Tests are organized by **function**:

- **Preprocessing functions:**
  - `prime()` - Prepares synergy values
  - `updates()` - Creates update table
  - `expand()` - Expands graph with composite nodes
  - `involution()` - Converts between edge-synergy formats

- **MFR computation:**
  - `get_mfrs()` - Computes minimal functional routes

### Old Structure (Removed)

Tests were organized by **bug fix number**:
- `TestBugFix1_CompositeInitialization`
- `TestBugFix2_EmptyListHandling`
- etc.

This made it hard to find tests for specific functions.

---

## Expected Test Results

When everything is installed correctly, you should see:

```
Ran 36 tests in X.XXXs

OK
```

All 36 tests should pass. If some are skipped, that's okay - they're marked with `@unittest.skipUnless` and will skip if dependencies aren't available.

---

## Running Tests in Different Environments

### Command Line (Recommended)
```bash
python -m unittest discover mfrpy.test -v
```

### Python Interactive Shell
```python
import unittest
loader = unittest.TestLoader()
suite = loader.discover('mfrpy.test')
runner = unittest.TextTestRunner(verbosity=2)
runner.run(suite)
```

### pytest (if installed)
```bash
pytest mfrpy/test/ -v
```

---

## Verification Checklist

- [ ] Virtual environment created OUTSIDE the repo (in parent directory)
- [ ] Virtual environment activated
- [ ] Navigated into the mfrpy directory
- [ ] mfrpy installed: `pip install -e .`
- [ ] All dependencies installed (check with `pip list`)
- [ ] Tests run: `python -m unittest discover mfrpy.test -v`
- [ ] All tests pass (some may be skipped if example graphs unavailable)

---

## Contact

If you encounter any issues not covered here, please let me know:
- What command you ran
- The full error message
- Your Python version: `python --version`
- Whether you're using a virtual environment
