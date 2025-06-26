
# pyenv Installation Guide (Non-root User) 🚀

This guide helps you install and manage multiple Python versions using `pyenv` on a Linux VM **without using the root user**.

---

## 🛑 1. Do NOT Use Root User
- `pyenv` is designed for per-user installations.
- Using `root` can mess up file permissions or system Python.

---

## 🔄 2. Update Package List
```bash
sudo apt update
```
- Updates your system’s package list.

---

## 📦 3. Install Build Dependencies
```bash
sudo apt install -y \
make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm \
libncurses5-dev libncursesw5-dev xz-utils tk-dev \
libffi-dev liblzma-dev git libgdbm-dev libnss3-dev \
libedit-dev libdb-dev uuid-dev
```
- These are required to build and run Python from source.

---

## 🌐 4. Install pyenv
```bash
curl https://pyenv.run | bash
```
- Installs `pyenv`, `pyenv-virtualenv`, and other helpers.

---

## ⚙️ 5. Add pyenv to Shell Config
Add the following lines to `~/.bashrc` or `~/.bash_profile`:
```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init - bash)"
eval "$(pyenv virtualenv-init -)"
```

---

## 🔄 6. Reload Shell
```bash
source ~/.bashrc
```
- Applies the changes without restarting the session.

---

## 🐍 7. List Python Versions
```bash
pyenv versions
```
- Shows installed and currently active Python versions.

---

## 📥 8. Install a Python Version
```bash
pyenv install 3.10.17
```
- Downloads and compiles Python 3.10.17.

---

## 📦 9. Create a Virtual Environment
```bash
pyenv virtualenv 3.10.17 myenv
```
- Creates a virtualenv named `myenv` using Python 3.10.17.

---

## 🚀 10. Activate Virtual Environment
```bash
pyenv activate myenv
```
- Switches to the environment `myenv`. All Python commands now use this environment.

---

✅ You're now ready to use multiple Python versions with isolated environments!
