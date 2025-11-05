# Secure Login App (SQL Injection Mitigation Demo)

A simple Python web application built with the **Flask** framework that demonstrates how to **prevent** a classic SQL Injection (SQLi) attack by using secure coding practices.

This project boots up a local web server with a login page. The backend code correctly handles user input to query a **SQLite** database, neutralizing the SQLi vulnerability.

---

### Skills Demonstrated

* Python
* Flask (Web Framework)
* SQLite
* Web Security Fundamentals
* OWASP Top 10 (A03: Injection)
* Parameterized Queries (Prepared Statements)

---

### Project Overview

The core of this project is in `app.py`, which handles a user login request.

#### The Vulnerability (What *Not* to Do)

A vulnerable application would build an SQL query by directly formatting user input into a string:

> **VULNERABLE CODE:**
> `query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"`
>
> An attacker could enter `' OR '1'='1'` as the username, making the final query `...WHERE username = '' OR '1'='1'`, which is always true, bypassing the login.

#### The Solution (Implemented in This Code)

This application uses **parameterized queries** (also known as prepared statements). This technique separates the SQL command from the user data, treating the user input as *data only*, never as part of the executable command.

> **SECURE CODE (from this project):**
> `query = "SELECT * FROM users WHERE username = ? AND password = ?"`
> `cursor.execute(query, (username, password))`
>
> With this method, an attacker's input of `' OR '1'='1'` is treated as a literal (and nonsensical) username, not as a command. The query will safely fail to find a match.

---

### How to Run

1.  **Clone this repository** (or ensure `app.py` is in its own folder).

2.  **Create and activate a virtual environment:**
    ```bash
    # Create the venv
    python -m venv venv

    # Activate on macOS/Linux
    source venv/bin/activate
    
    # Activate on Windows
    .\venv\Scripts\activate
    ```

3.  **Install dependencies:**
    This project only requires Flask (`sqlite3` is built-in to Python).
    ```bash
    pip install Flask
    ```

4.  **Run the application:**
    ```bash
    python app.py
    ```
    *This command will also automatically initialize the `users.db` file and add mock data.*

5.  **Test the app:**
    Open your web browser and go to `http://127.0.0.1:5000`.
    * **Test a valid login:** `alice` / `password123`
    * **Test a failed login:** `bob` / `wrongpassword`
    * **Test the SQLi attack:** Try logging in with the username `' OR '1'='1'` and any password. You will see "Invalid credentials," proving the attack was **unsuccessful**.
