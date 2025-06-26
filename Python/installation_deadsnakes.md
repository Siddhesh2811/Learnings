
# 🐍 Installing Python Using Deadsnakes PPA on Ubuntu 22.04

This guide explains how to install a specific version of Python (e.g., **Python 3.12**) using the **Deadsnakes PPA**, safely and side-by-side with the system Python on Ubuntu 22.04.

---

## ✅ Prerequisites

- Ubuntu 22.04
- Sudo privileges
- Internet connection

---

## 📦 Step-by-Step Installation

### 1. Update System Packages

```bash
sudo apt update && sudo apt upgrade -y
```

---

### 2. Install Required Utilities

```bash
sudo apt install -y software-properties-common
```

---

### 3. Add the Deadsnakes PPA

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
```

> You may be prompted to press `Enter` to confirm adding the PPA.

---

### 4. Install Python (e.g., Python 3.12)

```bash
sudo apt install -y python3.12 python3.12-venv python3.12-dev
```

| Package           | Description                                      |
|-------------------|--------------------------------------------------|
| `python3.12`      | The Python 3.12 interpreter                      |
| `python3.12-venv` | Enables creation of virtual environments         |
| `python3.12-dev`  | Header files and tools for building extensions   |

---

### 5. Verify Python Installation

```bash
python3.12 --version
```

Expected output:

```
Python 3.12.x
```

---

## 🧪 Create a Virtual Environment (Recommended)

```bash
python3.12 -m venv ~/myenv
source ~/myenv/bin/activate
```

Install dependencies inside the virtual environment:

```bash
pip install flask requests
```

To deactivate:

```bash
deactivate
```

---

## ⚙️ Optional: Set Python 3.12 as Default for Current User

> ⚠️ Do **not** modify system-wide Python alternatives on Ubuntu.  
> Use aliases for convenience **only in your shell**:

```bash
echo "alias python=python3.12" >> ~/.bashrc
echo "alias pip=pip3.12" >> ~/.bashrc
source ~/.bashrc
```

---

## 🧹 Uninstalling (Optional)

To remove Python 3.12:

```bash
sudo apt remove python3.12 python3.12-venv python3.12-dev
```

To remove the Deadsnakes PPA:

```bash
sudo add-apt-repository --remove ppa:deadsnakes/ppa
sudo apt update
```

---

## 🔗 Resources

- [Deadsnakes PPA on Launchpad](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa)
- [Official Python Downloads](https://www.python.org/downloads/)

---

## 📌 Notes

- Installing Python via Deadsnakes **does not interfere** with Ubuntu’s system Python.
- Always use **virtual environments** to isolate project dependencies in production and development.

---

## 👨‍💻 Author

This guide was created to help developers and system administrators install modern versions of Python safely and cleanly on Ubuntu using the Deadsnakes PPA.
