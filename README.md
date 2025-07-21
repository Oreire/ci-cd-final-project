# CI/CD Tools and Practices Final Project Template

This repository contains the template to be used for the Final Project for the Coursera course **CI/CD Tools and Practices**.

## Usage

This repository is to be used as a template to create your own repository in your own GitHub account. No need to Fork it as it has been set up as a Template. This will avoid confusion when making Pull Requests in the future.

From the GitHub **Code** page, press the green **Use this template** button to create your own repository from this template.

Name your repo: `ci-cd-final-project`.

## Setup

After entering the lab environment you will need to run the `setup.sh` script in the `./bin` folder to install the prerequisite software.

```bash
bash bin/setup.sh
```

Then you must exit the shell and start a new one for the Python virtual environment to be activated.

```bash
exit
```

## Tasks


## License

Licensed under the Apache License. See [LICENSE](/LICENSE)

## Author

Skills Network

## <h3 align="center"> © IBM Corporation 2023. All rights reserved. <h3/>

## My Own Documentation:

## 📄 CI Workflow Overview

This GitHub Actions workflow automates **linting** and **unit testing** for a Python-based service within a containerised CI environment. It runs on code changes pushed to or pulled into the `main` branch.

### 🚀 Trigger Conditions

The workflow is triggered:
- On a **push** to the `main` branch
- On a **pull request** targeting the `main` branch

```yaml
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
```

---

### 🧪 Job: `lint-and-test`

This job performs **code quality checks** and **test execution** in a reproducible, containerised setup.

#### 🖥️ Environment

- **Runner OS**: `ubuntu-latest`
- **Container image**: `python:3.9-slim` — lightweight Python base for runtime isolation

```yaml
runs-on: ubuntu-latest
container:
  image: python:3.9-slim
```

#### 🔧 Workflow Steps

1. **Checkout Code**
   - Retrieves the repository’s content for analysis
   ```yaml
   - name: Checkout
     uses: actions/checkout@v3
   ```

2. **Install Dependencies**
   - Upgrades `pip`
   - Installs Python packages listed in `requirements.txt`
   - Adds linting (`flake8`) and testing (`nose`) tools
   ```yaml
   - name: Install dependencies
     run: |
       python -m pip install --upgrade pip
       pip install -r requirements.txt
       pip install flake8 nose
   ```

3. **Lint with Flake8**
   - Runs Flake8 on `service/` directory
   - Flags syntax errors, complexity issues, and style violations
   ```yaml
   - name: Lint with flake8
     run: |
       flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
       flake8 service --count --max-complexity=10 --max-line-length=127 --statistics
   ```

4. **Run Unit Tests with Nose**
   - Executes tests with verbosity, spec-style formatting, and coverage metrics
   - Targets the `app` package for coverage analysis
   ```yaml
   - name: Run unit tests with nose
     run: |
       nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
   ```

### ✅ Summary

This CI workflow:
- Ensures code pushed to or merged into `main` meets syntax and style standards
- Validates application functionality via unit tests
- Runs in a **containerised** Python environment for portability and reproducibility

Perfect for reinforcing DevSecOps best practices — automation, consistency, and fast feedback loops.

