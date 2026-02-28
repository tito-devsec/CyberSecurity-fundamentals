# 1. Foundations of Web Security

**Author:** Tito Oscar Mwaisengela
**Category:** Cybersecurity Fundamentals

---

## 🌐 HTTP/HTTPS Fundamentals

The Hypertext Transfer Protocol (HTTP) is the foundation of data communication for the World Wide Web. Understanding its request/response lifecycle is critical for security analysis.

### Request/Response Lifecycle

```text
+----------+          +----------+          +----------+
|          |  HTTP    |          |  Query   |          |
| Browser  | Request  |  Server  | -------->| Database |
| (Client) | -------->| (Logic)  | <--------| (Data)   |
|          | <--------|          |  Result  |          |
+----------+  HTTP    +----------+          +----------+
              Response
```

1.  **Client Request:** The browser sends an HTTP request (GET, POST, etc.) containing headers and potentially a body.
2.  **Server Processing:** The web server (e.g., Nginx, Apache) receives the request and passes it to the application logic (e.g., Python, Node.js).
3.  **Database Interaction:** The application logic may query a database to retrieve or store data.
4.  **Server Response:** The server sends an HTTP response back to the client, including a status code (e.g., 200 OK, 404 Not Found) and the requested content.

---

## 🔑 Authentication vs. Authorization

| Concept | Definition | Question Answered |
| :--- | :--- | :--- |
| **Authentication (AuthN)** | Verifying the identity of a user or system. | "Who are you?" |
| **Authorization (AuthZ)** | Determining what an authenticated user is allowed to do. | "What can you do?" |

### Authentication Flow

```text
User ----(Credentials)----> [Auth Service] ----(Verify)----> [Database]
                                 |
                                 +----(Success)----> [Issue Token/Session]
                                 |
                                 +----(Failure)----> [Error Message]
```

---

## 🍪 Session Management

Session management is the process of securely handling multiple requests from the same user. Since HTTP is stateless, sessions are used to maintain state.

*   **Session ID:** A unique identifier stored in a cookie or URL.
*   **Security Risks:** Session Hijacking, Session Fixation, and Insecure Session Termination.
*   **Mitigation:** Use `HttpOnly`, `Secure`, and `SameSite` cookie flags.

---

## 🔐 Cryptographic Basics

Cryptography is the practice of securing information from unauthorized access.

*   **Symmetric Encryption:** Same key for encryption and decryption (e.g., AES).
*   **Asymmetric Encryption:** Public key for encryption, private key for decryption (e.g., RSA).
*   **Hashing:** One-way transformation of data into a fixed-size string (e.g., SHA-256). Unlike encryption, hashing cannot be reversed.

---

## 🛡️ Threat Modeling Concepts

Threat modeling is a structured approach to identifying and prioritizing potential security threats.

### The CIA Triad

The CIA Triad is the core model for information security:

1.  **Confidentiality:** Ensuring that data is accessible only to those authorized to have access.
2.  **Integrity:** Ensuring that data is accurate and has not been tampered with.
3.  **Availability:** Ensuring that systems and data are accessible when needed.

### Attack Surface Mapping

```text
[ External Network ] <---( Firewall )---> [ DMZ / Web Server ] <---( Internal Firewall )---> [ Internal Network / DB ]
         |                                         |                                               |
    (Attacker)                                (Vulnerability)                                 (Sensitive Data)
```

Mapping the attack surface involves identifying all possible points where an attacker can enter or extract data from a system.
