# gh-actions

Configure GitHub Actions for Python

- https://github.com/ymyzk/tox-gh-actions
- https://tox.readthedocs.io/en/latest/index.html

## Python Configuration (v0.0.1)

- `.github/workflows/Python-check.yaml`

```yml
name: Python-check

on:
  push:
    branches:
      - master
      - 'dev*'
      - 'fix*'
      - 'issue*'
      - 'python*'
      - 'doc*'
      - 'gh-actions'
      - 'githubactions'
    paths:
      - '**.py'
      - '**.ini'
  pull_request:
    branches:
      - master
    paths:
      - '**.py'
      - '**.ini'

jobs:
  build:
    runs-on: ${{ matrix.platform }}
    strategy:
      matrix:
        platform: [ubuntu-latest, macos-latest, windows-latest]
        python-version: [3.6, 3.7, 3.8]

    steps:
    - uses: actions/checkout@v1
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v1
      with:
        python-version: ${{ matrix.python-version }}
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install tox tox-gh-actions
    - name: Test with tox
      run: tox
      env:
        PLATFORM: ${{ matrix.platform }}
```

- `tox.ini`

```ini
[tox]
envlist = py{36,37,38}-{linux,macos,windows}
toxworkdir={toxinidir}/python/dalex/.tox
temp_dir={toxinidir}/python/dalex/.tmp
setupdir={toxinidir}/python/dalex/
skip_missing_interpreters=true

[gh-actions]
python =
    3.6: py36
    3.7: py37
    3.8: py38

[gh-actions:env]
PLATFORM =
    ubuntu-latest: linux
    macos-latest: macos
    windows-latest: windows

[testenv]
changedir = {toxinidir}/python/dalex/test
commands = discover
deps =
    discover
    scikit-learn
```
