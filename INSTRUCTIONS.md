# How to Run the Tests

## Setup

### 1. Create a virtual environment

Make sure to create the virtual environment OUTSIDE the repo folder. This avoids some issues with venv overwriting files.

```bash
# Go to the parent folder
cd ..

# Create venv here
python -m venv mfrpy-env

# Activate it
# Mac/Linux:
source mfrpy-env/bin/activate

# Windows:
mfrpy-env\Scripts\activate

# Go back into the repo
cd mfrpy

# Install the package
pip install -e .
```

### 2. Run the tests

```bash
python -m unittest discover mfrpy.test -v
```

You should see something like:

```
Ran 36 tests in X.XXXs

OK
```

### 3. Run specific tests

```bash
# Just preprocessing tests
python -m unittest mfrpy.test.test_preprocessing -v

# Just MFR tests
python -m unittest mfrpy.test.test_mfr_processing -v

# A single test class
python -m unittest mfrpy.test.test_preprocessing.TestPrime -v
```

---

## Test Files

The tests are split into a few files:

- `test_preprocessing.py` - tests for prime(), updates(), expand(), involution()
- `test_mfr_processing.py` - tests for get_mfrs()
- `test_setup.py` - shared setup code
- `test_update_expand.py` - original tests from before

The old `test_bug_fixes.py` file was removed since all those tests got moved to the new files.

---

## Common Issues

### "__file__ is not defined"

This happens if you run the tests in an interactive shell (like Sublime's Python console). The fix is already in the code, but if it still happens:

1. Run tests from the command line instead
2. Or just install the package first with `pip install -e .`

### "No module named 'mfrpy'"

You need to install it:

```bash
pip install -e .
```

### Tests are getting skipped

Probably missing dependencies. Run `pip install -e .` to install everything.

---

## Quick Checklist

- [ ] Virtual environment created outside the repo
- [ ] Activated the venv
- [ ] Ran `pip install -e .`
- [ ] Ran the tests

Let me know if anything doesn't work.
