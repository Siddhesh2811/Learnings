# Pyenv Installation Guide (Custom Path)

This guide explains how to install **Pyenv** in a custom directory (`/app/data/Python_env`) and set it up for managing multiple Python versions.

---

## 🔧 Prerequisites

Switch to the application user:
```bash
su - appadm
```

Update system packages:
```bash
sudo dnf groupinstall -y "Development Tools"
```

Install required dependencies:
```bash
sudo dnf install -y \
    make gcc gcc-c++ \
    zlib-devel bzip2-devel readline-devel sqlite-devel \
    openssl-devel tk-devel libffi-devel xz-devel \
    git curl
```

---
## ⚙️ Configure Environment

Edit your **`.bashrc`**:
```bash
mkdir /app/data/Python_env
cd
vi .bashrc
```

Add the following lines at the end:
```bash
export PYENV_ROOT="/app/data/Python_env"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

Reload your shell:
```bash
source .bashrc
```

---

## 📦 Install Pyenv

Move to the application data directory and install Pyenv:
```bash
git clone https://github.com/pyenv/pyenv.git $PYENV_ROOT
git clone https://github.com/pyenv/pyenv-virtualenv.git $PYENV_ROOT/plugins/pyenv-virtualenv

```

---


## 🐍 Install Python Versions

Use Pyenv to install a specific Python version:

```bash
pyenv install <python-version>
```

Example:
```bash
pyenv install 3.10.12
```

---

## ✅ Verify Installation

Check the installed versions:
```bash
pyenv versions
```

Set a global version:
```bash
pyenv global 3.10.12
```

Confirm the active Python:
```bash
python --version
```

---

## 📖 Useful Pyenv Commands

Here are some common commands for managing Python versions and environments:

### 🔍 Version Management
- List installed versions:
  ```bash
  pyenv versions
  ```
- List all available versions:
  ```bash
  pyenv install --list
  ```
- Install a new version:
  ```bash
  pyenv install <version>
  ```
- Uninstall a version:
  ```bash
  pyenv uninstall <version>
  ```

### 🌍 Environment Settings
- Set a global (default) version:
  ```bash
  pyenv global <version>
  ```
- Set a version for the current shell session only:
  ```bash
  pyenv shell <version>
  ```
- Set a version for a specific project (creates `.python-version`):
  ```bash
  pyenv local <version>
  ```

### 🧩 Virtual Environments
- Create a new virtual environment:
  ```bash
  pyenv virtualenv <version> <env-name>
  ```
- List virtual environments:
  ```bash
  pyenv virtualenvs
  ```
- Activate a virtual environment:
  ```bash
  pyenv activate <env-name>
  ```
- Deactivate a virtual environment:
  ```bash
  pyenv deactivate
  ```
- Remove a virtual environment:
  ```bash
  pyenv uninstall <env-name>
  ```

---

## 📌 Notes

- Always install dependencies before building Python versions.  
- Use `pyenv virtualenv` to create isolated environments.  
- Keep `.bashrc` updated with Pyenv configuration for persistence.  
- The `pyenv shell` command is useful for quick testing without changing global or local settings.

---

