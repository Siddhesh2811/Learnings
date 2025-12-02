# SAP ECC Technical Stack – Complete Overview

## 1. Introduction
SAP ECC (ERP Central Component) is SAP’s traditional on-premise ERP system.  
The *ECC Stack* consists of the application modules, technical foundation, integration components, database, OS, and enhancement layers such as support packages and business functions.

---

# 2. SAP ECC Stack Architecture Overview

## 2.1 Application Layer (ECC Modules)
ECC contains multiple functional modules used by business operations:

- FI – Financial Accounting
- CO – Controlling
- MM – Materials Management
- SD – Sales & Distribution
- PP – Production Planning
- QM – Quality Management
- PM – Plant Maintenance
- WM – Warehouse Management
- HCM – Human Capital Management

These run on the **ABAP application server**.

---

## 2.2 SAP NetWeaver Technical Stack
SAP ECC is built on **SAP NetWeaver**, which provides the core technical platform.

### Components:
- SAP Basis  
- ABAP Stack  
- Java Stack (optional)  
- ICM – Internet Communication Manager  
- Work Processes: Dialog, Update, Spool, Background, Enqueue  
- Transport System (tp, R3trans)

---

## 2.3 Database Layer
ECC supports multiple databases:

- Oracle  
- IBM DB2  
- Microsoft SQL Server  
- SAP MaxDB  
- SAP HANA (for late ECC versions)

The DB stores all configuration, master, and transaction data.

---

## 2.4 Operating System Layer
SAP ECC can run on:

- Linux (SUSE, RHEL)
- Windows Server
- Solaris
- AIX

---

## 2.5 Integration Layer
ECC integrates with SAP / Non-SAP systems using:

- IDocs  
- BAPI / RFC  
- Web Services (SOAP/REST)  
- SAP PI/PO  
- ALE/EDI

---

# 3. Support Packages (SP), Support Package Stacks (SPS)

## 3.1 What Are Support Packages?
Support Packages are SAP-delivered **bug fixes**, **security patches**, and **stability improvements**.

- They fix known SAP issues  
- Improve performance  
- Enhance security  
- Required before installing Enhancement Packages

### Types of Support Packages:
- **SP (Support Package)** – basic patch for a specific component  
- **SPS (Support Package Stack)** – bundle of compatible SPs  
- **Kernel Patches** – executable updates (disp+work, tp, r3trans)  
- **SAP Notes** – individual corrections

---

# 4. Software Components

## 4.1 Definition
Software Components are the **technical building blocks** of an SAP ECC system.  
Each component has its own:
- Release version  
- Support Package level  
- Dependencies  

## 4.2 Common SAP ECC Software Components
| Component | Description |
|----------|-------------|
| SAP_BASIS | Core SAP system services |
| SAP_ABA | ABAP runtime libraries |
| SAP_APPL | Logistics modules (MM/SD/PP/QM/PM) |
| SAP_HR | Human Resources component |
| SAP_UI | User Interface technologies |
| EA-APPL | Enterprise Add-On for Logistics |
| ST-PI | Support Tools Plug-In |
| ST-A/PI | Basis monitoring/add-on |
| PI_BASIS | Integration basis (for PI/PO systems) |

These are visible in **System → Status → Component Version**.

---

# 5. Business Functions (Switch Framework)

## 5.1 Definition
Business Functions are optional **functional enhancements** delivered via Enhancement Packages (EHP).  
Activated using transaction **SFW5**.

## 5.2 Types of Business Functions
- **Reversible** – can be activated/deactivated  
- **Non-Reversible** – once activated cannot be turned off  

## 5.3 Purpose
- Introduce new functionalities  
- Enhance business processes  
- Deliver country-specific compliance  
- Add industry-specific functions  

## 5.4 Example Business Functions
| Business Function | Purpose |
|------------------|---------|
| LOG_MM_SIT | Stock-in-Transit enhancements |
| FIN_GL_CI_1 | New General Ledger capabilities |
| LOG_SD_CI_2 | SD process improvements |
| HCM_TIM_CALE | HR Time mgmt for specific countries |

---

# 6. Summary

| Layer / Concept | Description |
|------------------|-------------|
| **ECC Stack** | Full technical & functional layers of SAP ECC |
| **Support Packages** | Bug fixes, security patches, stability improvements |
| **Software Components** | Technical modules like SAP_BASIS, SAP_ABA, SAP_APPL |
| **Business Functions** | Optional enhancements activated via SFW5 |

---

# 7. Conclusion
The SAP ECC Stack is a combination of:
- Functional modules  
- NetWeaver platform  
- Database & operating system  
- Integration components  
- Support packages  
- Business functions  
- Software components  

Together, these elements define the complete technical architecture and functional capability of an ECC system.

