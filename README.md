
# Secure File Reader: Defending Against Directory Traversal Attacks

Imagine a bouncer at a VIP club with a strict guest list—if your name isn’t on it, you don’t get in, no matter how confidently you walk up. This project, **Secure File Reader**, applies the same principle to file access in software. It only allows reading files located inside a specific, trusted directory (the "VIP folder"). Any attempt to access files outside this directory is immediately blocked, preventing a common security threat called a directory traversal attack.

---

## What is a Directory Traversal Attack?

A directory traversal attack occurs when an attacker tries to access files outside the intended folder by manipulating file path inputs, often using special characters like `../` to move up directories. For example, instead of requesting `report.txt`, an attacker might input `../../etc/passwd` to try accessing sensitive system files.

If an application does not properly verify the true location of requested files, it may unintentionally expose critical data, posing serious security risks.

---

## Project Overview

The Secure File Reader ensures that only files inside a predefined safe directory (`safe_files`) can be accessed. It does this by:

- Defining a **base directory** (`BASE_DIR`) which acts as the trusted "VIP folder".
- Resolving the user-provided filename to an absolute path.
- Verifying that the resolved path starts with the `BASE_DIR` path.
- Blocking any access if the requested file lies outside the safe directory.

This approach effectively stops directory traversal attempts, acting as a vigilant gatekeeper.

---

## How It Works: Code Explanation

```

import os

# Define the safe base directory

BASE_DIR = os.path.abspath("safe_files")

# Ensure base directory exists

os.makedirs(BASE_DIR, exist_ok=True)

def read_file_securely(user_input_filename):
try:
\# Construct the absolute path for the requested file
requested_path = os.path.abspath(os.path.join(BASE_DIR, user_input_filename))

        # Check if requested path is inside the trusted base directory
        if not requested_path.startswith(BASE_DIR):
            raise Exception("⚠️ Unauthorized access attempt blocked!")
    
        # Read and return file content
        with open(requested_path, 'r') as file:
            content = file.read()
            return content
    
    except FileNotFoundError:
        return "❌ File not found."
    except Exception as e:
        return f"❌ Error: {str(e)}"
```

### Example Usage

```

if __name__ == "__main__":
print("📂 Secure File Reader")
filename = input("Enter the filename to read (within 'safe_files/' folder): ")
output = read_file_securely(filename)
print("\n🔎 File Content:\n", output)

```

---

## Why This Matters

Web servers and many applications serve files from a specific directory, and many allow user input to specify which file to read. Without robust checks, attackers can exploit this to access sensitive files anywhere on the system.

This project demonstrates how **strict boundaries and verification** in code prevent such vulnerabilities. By always resolving paths and confirming they lie within an approved directory, you close the door to unauthorized access—a critical security best practice.

---

Securing software begins with assuming that every input could be an attack — and that mindset is what the Secure File Reader is built on. Through a simple yet powerful logic, we’ve demonstrated how directory traversal attacks can be blocked by validating and restricting file access to a safe, predefined location. By resolving paths properly and verifying them against a trusted base directory, the Secure File Reader acts as a gatekeeper, protecting your application from unauthorized file access. While the concept might seem basic, it echoes a critical cybersecurity principle: trust nothing, verify everything. Whether you’re building a personal project or an enterprise application, remember — good security isn’t just about firewalls and encryption. Sometimes, it’s about writing thoughtful code that closes the door before an attacker even knocks.


## Getting Started

1. Create a folder named `safe_files` in your project directory.
2. Place some text files inside this folder for testing.
3. Run the script and input filenames located only within `safe_files`.
4. Attempt to input dangerous paths like `../secret.txt` to see the protection in action.

---

## Summary

- Secure File Reader uses Python’s `os.path.abspath()` to resolve full file paths.
- Verifies that file access requests remain within a trusted base directory.
- Blocks directory traversal attacks by rejecting any requests outside this safe zone.
- Serves as a practical example of secure coding in file system interactions.

---

*Security begins with the mindset: trust nothing, verify everything.*  
*Just like a VIP club’s bouncer, your code must carefully check who gets in.*

---

*Happy Secure Coding!*  
*— Vai (Vaibhavi Tilak)*
```



