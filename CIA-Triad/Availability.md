# 🔐 LAB 2 — Avaliability
🎯 Lab Goal

Understand, implement, and verify data integrity by detecting unauthorized modification of data, configurations, and source code using real tools and techniques.
showcases 
1. Hashing
2. File integrity monitoring
3. access control
4. git integrity

 🧪 Hashing & Checksums (Basic Integrity)
 🛠 Tools
      - sha256sum
      - md5sum
step one: we created a plain text and coverted in to a hash value using sha256sum
   - echo "This is a sensitive file" > data.txt
   - sha256sum data.txt > hash.txt
(this this hashing algorithm only work unidirectional, a single bite change to the file will creates automatical different hash value.
sttep two: make chnages to the original file, save it and run the hash function
 - nano data.txt
 - Just only add s at the end of the sentence and save it data1.txtx (another file)
 - sha256sum data1.txt > hash1.txt
step three: compare the two hash value (hash.txt and hash1.txt) and see the difference.
  - cat hash.txt
  - cat hash1.txt
    
### lesson
No two different file can have the same hash value, this will tell us that, any modification or alteration to our original data will easily be determined using hash function

🧪 File Integrity Monitoring
🛠 Tools
- AIDE (Advanced Intrusion Detection Environment)
  
