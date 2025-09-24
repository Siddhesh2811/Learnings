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
sudo apt update
```

Install required dependencies:
```bash
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev git libgdbm-dev libnss3-dev libedit-dev libdb-dev uuid-dev
```

---

## 📦 Install Pyenv

Move to the application data directory and install Pyenv:
```bash
cd /app/data
curl https://pyenv.run | bash
```

---

## ⚙️ Configure Environment

Edit your **`.bashrc`**:
```bash
cd
vi .bashrc
```

Add the following lines at the end:
```bash
PYENV_ROOT="/app/data/Python_env"
PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

Reload your shell:
```bash
source .bashrc
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

## 📌 Notes

- Always install dependencies before building Python versions.  
- Use `pyenv virtualenv` to create isolated environments.  
- Keep `.bashrc` updated with Pyenv configuration for persistence.

---
