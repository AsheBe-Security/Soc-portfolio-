# 🔐 LAB 1 — CONFIDENTIALITY
## Access control
🎯 Objective

- Implement strict access control using Linux permissions
- Enforce least privilege for users
- Protect sensitive data using encryption at rest
- Validate confidentiality through attack simulations.

🧩 Architecture Overview

- Sensitive HR data stored on Linux server
- Only hr_user is authorized to access data
- Unauthorized users attempt access
- Encryption ensures protection even if access controls fail

(Add a simple diagram here later — draw.io is perfect)
step one - creating a protected directory having sensitive file 
- sudo adduser hr_user
- sudo adduser attacker_user
Step 2: Create Sensitive Directory & File
- sudo mkdir -p /secure/hr_data
- sudo nano /secure/hr_data/employee_records.txt
     - A sample text where provided (Salary profile for the employee)
Step 3: Apply Access Control (Permissions)
 - Assign ownership
     - sudo chown -R hr_user:hr_user /secure/hr_data
- Restrict permissions
     - sudo chmod 700 /secure/hr_data
     - sudo chmod 600 /secure/hr_data/employee_records.txt
Step 4: Validate Access Control
 - Authorized access
    - su - hr_user
    - cat /secure/hr_data/employee_records.txt
    - Access to the specified file was provided once we provide our credentials to the ownership of that user
- Unauthorized access attempt
    - su - attacker_user
    - cat /secure/hr_data/employee_records.txt
          - Since we are not the owner of the user, permission to the specified file were denied

**Attack Simulation 1 — Unauthorized File Access**
Threat: Insider or compromised account attempts to read sensitive data
Result: Blocked by Linux permissions

## Encryption Implementation
Step one - Encrypt Sensitive File (GPG) 
  - gpg -c /secure/hr_data/employee_records.txt
     - employee_records.txt.gpg
Step two - Remove the plain text file
  - shred -u /secure/hr_data/employee_records.txt
Step three - Test Encrypted File Access
    Unauthorised use
       - gpg employee_records.txt.gpg
          (without the passphrase, we won't be able to view the file)
    Authorized user
