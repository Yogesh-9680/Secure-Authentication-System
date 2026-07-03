"""
Secure Authentication System
Single-file console project
"""

import hashlib
import random

users = {
    "admin": hashlib.sha256("admin123".encode()).hexdigest()
}

failed_attempts = {}

def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

def register():
    print("\n=== Register ===")
    username = input("New username: ").strip()
    if username in users:
        print("User already exists.")
        return
    password = input("New password: ")
    users[username] = hash_password(password)
    print("Registration successful.")

def login():
    print("\n=== Login ===")
    username = input("Username: ").strip()

    if failed_attempts.get(username, 0) >= 3:
        print("Account temporarily locked (3 failed attempts).")
        return

    password = input("Password: ")

    if username in users and users[username] == hash_password(password):
        print("Login successful!")
        failed_attempts[username] = 0
        dashboard(username)
    else:
        failed_attempts[username] = failed_attempts.get(username, 0) + 1
        print("Invalid credentials.")
        print("Failed attempts:", failed_attempts[username])

def change_password(username):
    old = input("Old password: ")
    if users[username] != hash_password(old):
        print("Incorrect old password.")
        return
    new = input("New password: ")
    users[username] = hash_password(new)
    print("Password updated successfully.")

def otp_demo():
    otp = random.randint(100000, 999999)
    print("Generated OTP (demo):", otp)
    entered = input("Enter OTP: ")
    if entered == str(otp):
        print("OTP verified.")
    else:
        print("Invalid OTP.")

def dashboard(username):
    while True:
        print(f"""
=== Welcome {username} ===
1. Change Password
2. OTP Verification Demo
3. Logout
""")
        ch = input("Choice: ")
        if ch == "1":
            change_password(username)
        elif ch == "2":
            otp_demo()
        elif ch == "3":
            print("Logged out.")
            break
        else:
            print("Invalid choice.")

def main():
    while True:
        print("""
==== Secure Authentication System ====
1. Register
2. Login
3. Exit
""")
        ch = input("Choice: ")
        if ch == "1":
            register()
        elif ch == "2":
            login()
        elif ch == "3":
            print("Goodbye!")
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
