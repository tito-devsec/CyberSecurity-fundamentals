# 2. OWASP Top 10 (2021) – Deep Technical Breakdown

**Author:** Tito Oscar Mwaisengela
**Category:** Cybersecurity Fundamentals

---

## 🛡️ A01: Broken Access Control

Broken access control is a critical vulnerability where an application fails to properly enforce restrictions on what authenticated users are allowed to do.

### Technical Explanation
Access control enforcement should occur on the server side, where the application can verify the user's identity and permissions. When this enforcement is missing or flawed, an attacker can access unauthorized functionality or data.

### Root Cause Analysis
*   **Insecure Direct Object Reference (IDOR):** Trusting user-supplied input (e.g., a user ID in a URL) to access resources without verifying ownership.
*   **Privilege Escalation:** A user with low privileges gaining access to administrative functions.
*   **Missing Function Level Access Control:** Failing to protect sensitive API endpoints or administrative pages.

### Real-World Attack Scenario
An attacker logs into their account and sees the URL `https://example.com/api/v1/user/123`. By changing the ID to `124`, they can view another user's profile without authorization.

### Vulnerable Code Example (Node.js)
```javascript
// Vulnerable: Directly using user-supplied ID without verification
app.get('/api/v1/user/:id', (req, res) => {
    const userId = req.params.id;
    db.query('SELECT * FROM users WHERE id = ?', [userId], (err, result) => {
        if (err) throw err;
        res.json(result);
    });
});
```

### Secure Code Example (Node.js)
```javascript
// Secure: Verifying user ownership before granting access
app.get('/api/v1/user/:id', authenticateToken, (req, res) => {
    const userId = req.params.id;
    const authenticatedUserId = req.user.id;

    if (userId !== authenticatedUserId) {
        return res.status(403).json({ message: "Forbidden: You do not have access to this resource." });
    }

    db.query('SELECT * FROM users WHERE id = ?', [userId], (err, result) => {
        if (err) throw err;
        res.json(result);
    });
});
```

### Developer Mitigation Patterns
*   **Deny by Default:** Start with no access and explicitly grant permissions.
*   **Centralized Access Control:** Use a single, well-tested module for all access control decisions.
*   **Audit and Log:** Log all access control failures to detect and respond to attacks.

### CVSS v3.1 Scoring Example
*   **Vector:** `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:H/A:H`
*   **Base Score:** 8.8 (High)
*   **Severity:** High

---

## 🛡️ A03: Injection (SQL Injection)

Injection vulnerabilities occur when an application sends untrusted data to an interpreter as part of a command or query.

### Technical Explanation
SQL Injection (SQLi) is a type of injection where an attacker can interfere with the queries that an application makes to its database. This can allow them to view data they are not normally able to retrieve, or even modify or delete data.

### SQL Injection Flow

```text
[ Attacker ] --( Malicious Input )--> [ Web Application ] --( Malicious Query )--> [ Database ]
                                                                                      |
[ Attacker ] <--( Sensitive Data )-- [ Web Application ] <--( Query Result )-- [ Database ]
```

### Vulnerable Code Example (PHP)
```php
// Vulnerable: Directly concatenating user input into a SQL query
$username = $_POST['username'];
$password = $_POST['password'];
$query = "SELECT * FROM users WHERE username = '$username' AND password = '$password'";
$result = mysqli_query($conn, $query);
```

### Secure Code Example (PHP)
```php
// Secure: Using parameterized queries (prepared statements)
$username = $_POST['username'];
$password = $_POST['password'];
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
$stmt->bind_param("ss", $username, $password);
$stmt->execute();
$result = $stmt->get_result();
```

### Developer Mitigation Patterns
*   **Parameterized Queries:** Always use prepared statements with parameterized queries.
*   **Input Validation:** Validate all user input against a whitelist of allowed characters and formats.
*   **Principle of Least Privilege:** Ensure the database user has only the minimum necessary permissions.

### CVSS v3.1 Scoring Example
*   **Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H`
*   **Base Score:** 9.8 (Critical)
*   **Severity:** Critical

---

## 🛡️ A10: Server-Side Request Forgery (SSRF)

SSRF is a vulnerability where an attacker can induce the server-side application to make requests to an unintended location.

### SSRF Internal Network Diagram

```text
[ Attacker ] --( Request to Internal IP )--> [ Web Server ] --( Internal Request )--> [ Internal DB / Metadata ]
                                                                                      |
[ Attacker ] <--( Internal Data )-- [ Web Server ] <--( Internal Response )-- [ Internal DB / Metadata ]
```

### Vulnerable Code Example (Python)
```python
# Vulnerable: Directly using user-supplied URL in a request
import requests
from flask import request

@app.route('/fetch_url')
def fetch_url():
    url = request.args.get('url')
    response = requests.get(url)
    return response.text
```

### Secure Code Example (Python)
```python
# Secure: Using a whitelist of allowed domains and protocols
import requests
from flask import request, abort
from urllib.parse import urlparse

ALLOWED_DOMAINS = ['example.com', 'api.example.com']

@app.route('/fetch_url')
def fetch_url():
    url = request.args.get('url')
    parsed_url = urlparse(url)

    if parsed_url.scheme not in ['http', 'https']:
        abort(400, "Invalid protocol")

    if parsed_url.netloc not in ALLOWED_DOMAINS:
        abort(400, "Domain not allowed")

    response = requests.get(url)
    return response.text
```

### Developer Mitigation Patterns
*   **Whitelist-based Filtering:** Only allow requests to a predefined list of trusted domains and protocols.
*   **Network Segmentation:** Isolate the web server from sensitive internal resources.
*   **Disable Unused Protocols:** Disable support for protocols like `file://`, `gopher://`, and `dict://`.

### CVSS v3.1 Scoring Example
*   **Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:N/A:N`
*   **Base Score:** 8.6 (High)
*   **Severity:** High
