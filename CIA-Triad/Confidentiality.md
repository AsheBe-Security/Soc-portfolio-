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
  
<img width="900" height="700" alt="Screenshot 2026-02-04 080153" src="https://github.com/user-attachments/assets/bd323aee-d885-443c-bde2-7688959a4618" />
 
Step 2: Create Sensitive Directory & File
- sudo mkdir -p /secure/hr_data
- sudo nano /secure/hr_data/employee_records.txt
     - A sample text where provided (Salary profile for the employee)
       
  <img width="960" height="486" alt="Screenshot 2026-02-04 080848" src="https://github.com/user-attachments/assets/96999b50-85b4-4224-afa1-941eaee88c0d" />

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
          - Since we are not the owner of the user, permission to the specified file were denied.

  <img width="961" height="597" alt="Screenshot 2026-02-04 082835" src="https://github.com/user-attachments/assets/d9412db8-00e4-4d60-81cb-70802e6ea17c" />
  

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
         
      <img width="961" height="597" alt="Screenshot 2026-02-04 082835" src="https://github.com/user-attachments/assets/cd5d5624-5b8c-425f-8ce2-8c464506f451" />

    Authorized user
       - gpg --pinentry-mode loopback -d employee_records.txt.gpg
     (problem occured for the agent reading the decryption passphrase when using the code
        - gpg -d wmployee_records.txt.gpg, so loopback was used to force the agen to run the passphrase in the background rather to prompt it)
         
<img width="953" height="533" alt="Screenshot 2026-02-06 075657" src="https://github.com/user-attachments/assets/1f03eda2-1619-449f-8f55-f25c92017f21" />

Another brute force was done to decrypt the file when repreated effort failed
  - gpg --pinentry-mode loopback --passphrase "YourPassphraseHere" -o employee_records.txt -d employee_records.txt.gpg
    
<img width="1023" height="341" alt="Screenshot 2026-02-04 123353" src="https://github.com/user-attachments/assets/2cc66d56-298b-4af3-a80f-889985f23798" />

The decrypted file can also be saved in to another readable file or plain text using this command
 - gpg --pinentry-mode loopback -o employee_records.txt -d employee_records.txt.gp
