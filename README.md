# Hash-Based Data Loss Prevention (DLP) System

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Flask](https://img.shields.io/badge/Flask-2.x-lightgrey)
![SQLite](https://img.shields.io/badge/SQLite-Database-green)

A highly sophisticated, enterprise-grade Data Loss Prevention (DLP) web application designed to prevent the unauthorized exfiltration of sensitive organizational data. Built with Python, Flask, and Bootstrap 5, it utilizes a dual-tier cryptographic detection engine (SHA3-256 and SimHash) to secure data without storing plain-text copies.

## 🚀 Key Features
- **Dual-Tier Detection Engine:** Uses exact (`SHA3-256`) and fuzzy (`SimHash`) cryptographic matching.
- **Zero-Knowledge Architecture:** Raw sensitive data is never stored; only abstract mathematical fingerprints reside on the server.
- **Role-Based Access Control (RBAC):** Strict separation between standard users (simulations/emails) and administrators (dashboard/registration).
- **Live Monitoring Dashboard:** Real-time logging of all `Allowed` and `Blocked` data transfer attempts.
- **Context-Triggered Piecewise Hashing:** Prevents data leakage even if an attacker slightly modifies a file (e.g., changing a single word).

---

## 🛠️ Technology Stack
* **Backend:** Python, Flask
* **Frontend:** HTML5, Bootstrap 5 CSS, Jinja2 Templating
* **Database:** SQLite (Unified Dual-Database Architecture)
* **Cryptographic Algorithms:** `SHA3-256` (Exact Match), `SimHash` (Fuzzy Match / Locality-Sensitive Hashing)

---

## 🏗️ Core Architecture & Detection Engine

### Tier 1: Exact Cryptographic Match (SHA3-256)
When text or a file is registered, a one-way mathematical fingerprint is generated. If a user attempts to send an exact, byte-for-byte replica of a registered file, the SHA3-256 hash immediately matches, and the transfer is blocked.

### Tier 2: Fuzzy Logic Match (SimHash)
An attacker might try to bypass Tier 1 logic by changing a single word in a document. To prevent this, the system calculates a Locality-Sensitive Hash (LSH). It checks the *Hamming Distance* between the incoming data's SimHash and the stored SimHashes. If the incoming data retains **70% or higher similarity** (a Hamming Distance of ≤ 19 bits across the 64-bit fingerprint), it intelligently flags it as a modified duplicate and blocks the transfer.

### Database Schemas
The application utilizes two separate database boundaries to maintain strict security:
1. **`user_pass_cred.db` / `admin_user_pass.db`:** Handles robust user identity management. Distinct databases ensure normal users and administrators are kept strictly separate.
2. **`database.db`:** The Master DLP Database containing the `sensitive_hashes` registry and the immutable `logs` ledger tracking all simulation and email attempts.

---

## ⚙️ Installation & Setup

### Prerequisites
- Python 3.8 or higher installed
- `pip` package manager

### Steps
1. **Clone the repository:**
   ```bash
   git clone https://github.com/AatifAli64/hash-based-dlp-system.git
   cd hash-based-dlp-system
   ```

2. **Create a virtual environment (optional but recommended):**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```
   *(Note: Ensure required cryptographic libraries like `simhash` are installed)*

4. **Initialize the Database:**
   *(Ensure the SQLite databases are set up by running the initialization script if provided, or simply running the app to auto-create them)*

5. **Run the Application:**
   ```bash
   python app.py
   ```
   Access the web app at `http://localhost:5000`.

---

## 💻 Usage & User Journeys

### For Standard Users
- **Simulate Leak (`/simulate`):** An educational sandbox portal where users can safely test the strictness of the DLP algorithms without consequences.
- **Send Email (`/send_email`):** A functional outbound communication proxy. The DLP core intercepts the transmission and blocks the email if a Tier 1 or Tier 2 match triggers.

### For Administrators
- **Register Data (`/register`):** Upload specific highly-restrictive files to add them to the `sensitive_hashes` database.
- **Logs Dashboard (`/logs`):** A live feed depicting all `Allowed` and `Blocked` transactions happening globally across the network.
- **Admin Panel (`/admin`):** A specialized diagnostic hub to inspect raw cryptographic hash registries.

---

## 🛡️ Security & Threat Model

### Targeted Attack Vectors
1. **The Careless Insider:** Accidental pasting of proprietary internal memos into a public email body to a client.
2. **The Malicious Exfiltrator:** Compromised employees attempting to upload restricted financial PDFs to unauthorized servers.
3. **The Obfuscator:** Attackers modifying a file (e.g., changing headers or text spacing) to trick simple hash-matching systems. (Resolved via Tier 2 Fuzzy Hashing).

### Implemented Safeguards
- **Zero-Knowledge Storage:** Raw sensitive data is never stored, mitigating the risk of the DLP database itself being breached.
- **Credential Protection:** All passwords are incrementally hashed to prevent credential stuffing.
- **Access Control:** Server-side sessions explicitly lock endpoints ensuring strict role-based access control.

---

*Developed as a robust Information Security project to demonstrate modern Enterprise Data Loss Prevention techniques.*
