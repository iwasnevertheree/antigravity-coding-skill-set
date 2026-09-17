# Input Validation, Sanitization & Injection Prevention

Guidelines for securing data ingress points and eliminating injection vectors across applications.

---

## 1. Input Validation Principles

1. **Allow-List over Deny-List**: Validate that inputs conform to known good formats (e.g. alphanumeric strings, valid UUIDs, positive integers), rather than attempting to filter known bad characters.
2. **Type Enforcement**: Parse external untrusted input into strictly-typed domain objects immediately upon ingress.
3. **Length & Range Bounds**: Always enforce maximum payload sizes, string lengths, and numeric ranges to prevent buffer exhaustion and DoS.

---

## 2. Injection Prevention Matrix

| Injection Vector | Vulnerable Pattern | Secure Remediation |
|---|---|---|
| **SQL Injection** | String concatenation: `f"SELECT * FROM users WHERE id = '{uid}'"` | Parameterized query / Prepared statements: `cursor.execute("SELECT * FROM users WHERE id = %s", (uid,))` |
| **Command Injection** | Shell execution: `os.system(f"convert {filename} output.png")` | Executable array without shell: `subprocess.run(["convert", filename, "output.png"], shell=False)` |
| **Path Traversal** | Direct path join: `open(base_dir + "/" + user_path)` | Path canonicalization & boundary check: `resolved = Path(base_dir, user_path).resolve(); assert resolved.is_relative_to(base_dir)` |
| **Cross-Site Scripting (XSS)** | Raw HTML interpolation: `innerHTML = user_content` | Safe DOM text insertion or auto-escaping template engines: `textContent = user_content` |
| **Server-Side Request Forgery (SSRF)** | Fetching user-supplied URLs directly | URL parsing, scheme allow-list (`https://`), internal private IP filtering (reject `127.0.0.1`, `10.0.0.0/8`, `169.254.169.254`). |

---

## 3. Boundary Sanitization vs. Escaping

- **Sanitization (Ingress)**: Strip or normalize invalid characters upon entering the system if the data must be stored in a canonical format.
- **Escaping (Egress)**: Context-aware encoding applied at the point of output (e.g., HTML-escaping for web rendering, SQL-escaping via parameterization for database drivers).
- **Rule**: Never rely on ingress sanitization alone when egress context encoding is required.
