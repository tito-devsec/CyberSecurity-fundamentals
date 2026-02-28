# 4. Vulnerable Lab Environment (Docker)

**Author:** Tito Oscar Mwaisengela
**Category:** Cybersecurity Fundamentals

---

## 🛡️ Hands-on Lab Environment

This lab environment is designed to provide a safe and controlled space for practicing the identification and exploitation of common web vulnerabilities. It uses Docker to ensure a consistent and isolated setup.

### Project Structure

```text
labs/
├── vulnerable-app/
│   ├── app.py (Vulnerable Flask App)
│   ├── Dockerfile
│   ├── requirements.txt
│   └── templates/
│       └── index.html
└── docker-compose.yml
```

---

## 🛠️ Lab Setup Instructions

### Prerequisites
*   **Docker:** [Install Docker](https://docs.docker.com/get-docker/)
*   **Docker Compose:** [Install Docker Compose](https://docs.docker.com/compose/install/)

### Installation Steps
1.  **Clone the repository:**
    ```bash
    git clone https://github.com/tito-devsec/CyberSecurity-fundamentals.git
    cd CyberSecurity-fundamentals/labs
    ```
2.  **Build and start the lab:**
    ```bash
    docker-compose up --build
    ```
3.  **Access the lab:**
    Open your browser and navigate to `http://localhost:5000`.

### Docker Commands
*   **Start the lab:** `docker-compose up -d`
*   **Stop the lab:** `docker-compose down`
*   **Reset the environment:** `docker-compose down -v && docker-compose up -d`
*   **View logs:** `docker-compose logs -f`

---

## 🛡️ Vulnerable Services

The lab application is vulnerable to several common web security issues:

### 1. SQL Injection (SQLi)
*   **Vulnerability:** The login form directly concatenates user input into a SQL query.
*   **Location:** `app.py` - `login()` function.
*   **How to Patch:** Use parameterized queries (prepared statements).

### 2. Broken Access Control (IDOR)
*   **Vulnerability:** The user profile page uses a predictable ID in the URL without verifying ownership.
*   **Location:** `app.py` - `profile()` function.
*   **How to Patch:** Verify the authenticated user's ID against the requested resource ID.

### 3. Server-Side Request Forgery (SSRF)
*   **Vulnerability:** The "Fetch URL" feature allows requests to any URL, including internal ones.
*   **Location:** `app.py` - `fetch_url()` function.
*   **How to Patch:** Use a whitelist of allowed domains and protocols.

### 4. Security Misconfiguration
*   **Vulnerability:** The application runs in debug mode and exposes detailed error messages.
*   **Location:** `app.py` - `app.run(debug=True)`.
*   **How to Patch:** Disable debug mode in production and use generic error messages.

### 5. Weak Authentication
*   **Vulnerability:** The application uses weak, easily guessable passwords and lacks rate limiting.
*   **Location:** `app.py` - `login()` function.
*   **How to Patch:** Enforce strong password policies and implement rate limiting on login attempts.

---

## 🛡️ Secure Version of Container

To apply secure patches, follow the instructions in `secure-code-examples/` and rebuild the Docker image:

```bash
# Example: Applying a patch and rebuilding
cp ../secure-code-examples/app_secure.py vulnerable-app/app.py
docker-compose up --build
```
