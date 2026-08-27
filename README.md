# Cybersecurity Lab Report: Password Cracking & PDF Hash Extraction

---

## Executive Summary

This report combines and details the technical workflows, methodologies, and findings from two hands-on cybersecurity project modules focused on **PDF Password Cracking and Hash Extraction**:
1. **Module 1**: Offline Password Cracking using **John the Ripper (JTR)** CLI and **Johnny GUI** on Windows [cite: 1].
2. **Module 2**: Browser-based Password Cracking using **Networkwalks Online Hash Calculator & Password Cracker** tools [cite: 2].

Both modules demonstrate the security principles behind password hashing, dictionary attacks, and standard hash format structures used by offline recovery tools [cite: 1, 2].

---

## Image Overview & Evidentiary Proofs

Below are the key visual artifacts and verification steps captured across both laboratory modules:

### 1. Extracted Flag & Unlocked Content
Upon successfully cracking the document password (`password1` / `good-luck`), the encrypted PDF (`My Locked PDF1.pdf`) reveals the captured flag [cite: 1, 2]:

![Captured Flag Page](/images/input_file_0.png)
*Figure 1: PDF unlocked successfully, revealing Flag 1: `nw{networkwalks_flag1_jtr_270521_1}`.* [cite: 1]

---

### 2. Module 1 Workflow (John the Ripper & Johnny GUI)

#### Setup & Configuration
Johnny GUI acts as a frontend for John the Ripper (Jumbo release) [cite: 1]. The `john.exe` binary path must be configured in Settings [cite: 1]:

![Johnny GUI Settings](input_file_1.png)
*Figure 2: Configuring `john.exe` path in Johnny GUI Settings.* [cite: 1]

#### Online Hash Extraction (pdf2john Format)
Using an online extraction utility (`onlinehashcrack.com`), the encrypted PDF's hash structure is extracted into the standard `$pdf$4*4*128*...` hash format required by JTR/Hashcat [cite: 1]:

![PDF Hash Extractor Output](input_file_2.png)
*Figure 3: Extracting the PDF encryption hash via Online HashCrack.* [cite: 1]

#### Hash Recovery Result in Johnny GUI
Loading `hash1.txt` into Johnny GUI and starting a dictionary attack cracks the password [cite: 1]:

![Johnny Cracked Result](input_file_3.png)
*Figure 4: Password cracked in Johnny GUI (`good-luck`).* [cite: 1]

---

### 3. Module 2 Workflow (Networkwalks Browser Tools)

#### Networkwalks Hash Calculator
Uploading `My Locked PDF1.pdf` directly parses the encrypted header locally in the browser to extract the `$pdf$...` hash string [cite: 2]:

![Networkwalks Hash Calculator](input_file_4.png)
*Figure 5: Networkwalks Hash Calculator generating the crackable `$pdf$` hash.* [cite: 2]

#### Networkwalks Online Password Cracker
Pasting the extracted hash into the browser-based dictionary cracker executes real-time wordlist testing at ~9 pw/s until matching `password1` [cite: 2]:

![Networkwalks Password Cracker Match](input_file_5.png)
*Figure 6: Dictionary attack match identified: `password1`.* [cite: 2]

---

## Module 1: Password Cracking with JTR & Johnny GUI

### Overview
John the Ripper (JTR) is a multi-platform password cracking tool used by penetration testers and security auditors to test password strength and recover lost credentials across various hashes and encrypted file formats (PDF, ZIP, Office documents) [cite: 1]. Johnny provides an intuitive GUI wrapper around the JTR command-line core [cite: 1].

### Step-by-Step Execution Guide

#### Step 1: Tool Installation & Setup
1. Download John the Ripper (Jumbo edition) from [Openwall](https://www.openwall.com/john/) [cite: 1].
2. Download and install Johnny GUI [cite: 1].
3. Launch Johnny GUI, navigate to **Settings**, and browse to select `john.exe` located inside the `run` folder of your JTR installation directory (e.g., `JTR_John CLI Win x64/john-1.9.0-jumbo-1-win64/run/john.exe`) [cite: 1].

#### Step 2: Hash Extraction from Encrypted PDF
1. Download the target encrypted file `My Locked PDF1.pdf` [cite: 1].
2. Open the [Online HashCrack PDF Extractor](https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php) [cite: 1].
3. Upload `My Locked PDF1.pdf` to convert the PDF encryption headers into a crackable hash string formatted for `pdf2john` [cite: 1].
4. Copy the complete output string starting with `$pdf$...` [cite: 1].
5. Paste the hash into a new text document in Notepad and save it as `hash1.txt` [cite: 1].

#### Step 3: Cracking with Johnny GUI
1. Re-open **Johnny**.
2. Click **Open password file** -> **Open password file (HASHOUS format)** and choose `hash1.txt` [cite: 1].
3. Click **Start new attack** [cite: 1].
4. Once completed, the cracked password (`good-luck`) is displayed in the main table [cite: 1].
5. Open `My Locked PDF1.pdf` using the recovered password to access the protected content and capture the flag (`nw{networkwalks_flag1_jtr_270521_1}`) [cite: 1].

---

## Module 2: Password Cracking with Networkwalks Web Tools

### Overview
Module 2 demonstrates browser-native hash parsing and dictionary-based recovery using web applications built on client-side WebAssembly/JavaScript execution [cite: 2].

### Step-by-Step Execution Guide

#### Step 1: Local Hash Generation
1. Navigate to the [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/) [cite: 2].
2. Drag and drop or browse to select `My Locked PDF1.pdf` [cite: 2].
3. The browser tool parses the PDF structure locally without uploading raw sensitive data to a remote server, producing a hash string:
   ```text
   $pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
   ``` [cite: 2]

#### Step 2: Running the Browser Dictionary Attack
1. Copy the complete `$pdf$...` hash string [cite: 2].
2. Open the [Networkwalks Password Cracker](https://networkwalks.com/password-cracker/) [cite: 2].
3. Paste the full hash string into the PDF Hash input field [cite: 2].
4. Click **START CRACKING** to initiate the attack against the integrated wordlist [cite: 2].
5. Upon reaching a dictionary match (`password1`), the tool displays the cleartext password on screen [cite: 2].

#### Step 3: Unlocking & Flag Submission
1. Open Adobe Acrobat or any standard PDF viewer [cite: 1, 2].
2. Enter `password1` when prompted [cite: 2].
3. Capture Flag 1 from the document body [cite: 1, 2].

---

## Key Technical Concepts & Comparative Analysis

### Encryption vs. Hashing

| Feature | Encryption | Hashing |
| :--- | :--- | :--- |
| **Type** | Two-way mathematical function | One-way cryptographic function [cite: 1, 2] |
| **Process** | Plaintext + Key $
ightarrow$ Ciphertext $
ightarrow$ Plaintext [cite: 1, 2] | Plaintext $
ightarrow$ Hash Digest (Fixed Length) [cite: 1, 2] |
| **Primary Goal** | Protect confidentiality during transmission/storage [cite: 1, 2] | Validate integrity and securely verify passwords [cite: 1, 2] |
| **Reversibility** | Reversible with valid decryption key [cite: 1, 2] | Irreversible by design (requires guessing/brute-force) [cite: 1, 2] |

### Comparison of Methods

| Metric / Feature | Module 1: JTR & Johnny GUI | Module 2: Networkwalks Tools |
| :--- | :--- | :--- |
| **Execution Environment** | Local OS (Windows CLI/GUI) [cite: 1] | Web Browser (Client-side JS) [cite: 2] |
| **Wordlist Flexibility** | Unlimited (custom rule sets, RockYou, custom lists) | Pre-configured built-in wordlists (e.g., Top 100/1000) [cite: 2] |
| **Performance / Speed** | High (Multi-threading, GPU/OMP support) [cite: 1] | Moderate (Browser JS execution limited to ~10-100 pw/s) [cite: 2] |
| **Setup Complexity** | Requires binary installation and path linking [cite: 1] | Zero setup required; instant execution [cite: 2] |

---

## Conclusion & Security Recommendations

This practical lab underscores how vulnerable short, common passwords (such as `password1` or `good-luck`) are to dictionary-based attacks [cite: 1, 2]. 

### Key Takeaways & Mitigation
1. **Password Length & Complexity**: An 8-character lowercase password can be cracked within minutes, whereas a 12+ character password with mixed characters requires centuries under standard compute models [cite: 2].
2. **Key Derivation Standards**: Modern PDF encryption standards (e.g., AES-256 with high iteration counts) significantly increase the compute cost per hash attempt, slowing down brute-force attacks.
3. **Multi-Factor Authentication (MFA)**: Defense-in-depth strategies must be implemented rather than relying solely on single-factor password protection.
