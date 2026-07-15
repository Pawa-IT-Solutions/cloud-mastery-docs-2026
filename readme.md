# Cloud Mastery Docs ☁️

This repository contains the source code for the Cloud Mastery documentation website, built with MkDocs and the Material theme.

---

## 🚀 Getting Started

Follow these steps to set up a local development environment.

### ✅ Prerequisites

Before you begin, ensure you have the following installed on your system:
* **Python** (version 3.10 or higher)
* **Git**

---

### 🖥️ Installation & Setup

1.  **Clone the Repository**

    Choose HTTPS or SSH and run the command in your terminal:

    ```bash
    # HTTPS
    git clone [https://github.com/Pawa-IT-Solutions/cloud-mastery-docs-2026.git](https://github.com/Pawa-IT-Solutions/cloud-mastery-docs-2026.git)

    # SSH
    git clone git@github.com:Pawa-IT-Solutions/cloud-mastery-docs-2026.git
    ```
    Then, navigate into the project directory:
    ```bash
    cd cloud-mastery-docs
    ```

2.  **Create and Activate a Virtual Environment**

    This keeps the project's dependencies isolated.

    * **On macOS / Linux:**
        ```bash
        python3 -m venv venv
        source venv/bin/activate
        ```

    * **On Windows:**
        ```bash
        py -m venv venv
        .\venv\Scripts\activate
        ```
    Your terminal prompt should now show `(venv)`.

3.  **Install All Dependencies**

    This single command reads the `requirements.txt` file and installs everything you need, including `mkdocs` and all plugins.

    ```bash
    pip install -r requirements.txt
    ```

---

### 🛠️ Running the Local Development Server

To preview your site and see live updates as you save your changes, run:

```bash
mkdocs serve