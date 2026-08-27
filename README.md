# Cybersecurity Lab Report: Password Cracking & PDF Hash Extraction

---

## Executive Summary

This report combines and details the technical workflows, methodologies, and findings from two hands-on cybersecurity project modules focused on **PDF Password Cracking and Hash Extraction**:
1. **Module 1**: Offline Password Cracking using **John the Ripper (JTR)** CLI and **Johnny GUI** on Windows.
2. **Module 2**: Browser-based Password Cracking using **Networkwalks Online Hash Calculator & Password Cracker** tools.

Both modules demonstrate the security principles behind password hashing, dictionary attacks, and standard hash format structures used by offline recovery tools.

---

## Image Overview & Evidentiary Proofs

Below are the key visual artifacts and verification steps captured across both laboratory modules:

### 1. Extracted Flag & Unlocked Content
Upon successfully cracking the document password (`password1` / `good-luck`), the encrypted PDF (`My Locked PDF1.pdf`) reveals the captured flag:

![Captured Flag Page](/images/input_file_0.png)
*Figure 1: PDF unlocked successfully, revealing Flag 1: `nw{networkwalks_flag1_jtr_270521_1}`.*

---

### 2. Module 1 Workflow (John the Ripper & Johnny GUI)

#### Setup & Configuration
Johnny GUI acts as a frontend for John the Ripper (Jumbo release). The `john.exe` binary path must be configured in Settings:

![Johnny GUI Settings](/images/Screenshot 2026-08-27 131348.png)
*Figure 2: Configuring `john.exe` path in Johnny GUI Settings.*

#### Online Hash Extraction (pdf2john Format)
Using an online extraction utility (`onlinehashcrack.com`), the encrypted PDF's hash structure is extracted into the standard `$pdf$4*4*128*...` hash format required by JTR/Hashcat:

![PDF Hash Extractor Output](/images/Screenshot 2026-08-27 131816.png)
*Figure 3: Extracting the PDF encryption hash via Online HashCrack.*

#### Hash Recovery Result in Johnny GUI
Loading `hash1.txt` into Johnny GUI and starting a dictionary attack cracks the password:

![Johnny Cracked Result](/images/Screenshot 2026-08-27 132033.png)
*Figure 4: Password cracked in Johnny GUI (`good-luck`).*

---

### 3. Module 2 Workflow (Networkwalks Browser Tools)

#### Networkwalks Hash Calculator
Uploading `My Locked PDF1.pdf` directly parses the encrypted header locally in the browser to extract the `$pdf$...` hash string:

![Networkwalks Hash Calculator](/images/Screenshot 2026-08-27 132543.png)
*Figure 5: Networkwalks Hash Calculator generating the crackable `$pdf$` hash.*

#### Networkwalks Online Password Cracker
Pasting the extracted hash into the browser-based dictionary cracker executes real-time wordlist testing at ~9 pw/s until matching `password1`:

![Networkwalks Password Cracker Match](/images/Screenshot 2026-08-27 132653.png)
*Figure 6: Dictionary attack match identified: `password1`.*

---

## Module 1: Password Cracking with JTR & Johnny GUI

### Overview
John the Ripper (JTR) is a multi-platform password cracking tool used by penetration testers and security auditors to test password strength and recover lost credentials across various hashes and encrypted file formats (PDF, ZIP, Office documents). Johnny provides an intuitive GUI wrapper around the JTR command-line core.

### Step-by-Step Execution Guide

#### Step 1: Tool Installation & Setup
1. Download John the Ripper (Jumbo edition) from [Openwall](https://www.openwall.com/john/).
2. Download and install Johnny GUI.
3. Launch Johnny GUI, navigate to **Settings**, and browse to select `john.exe` located inside the `run` folder of your JTR installation directory (e.g., `JTR_John CLI Win x64/john-1.9.0-jumbo-1-win64/run/john.exe`).

#### Step 2: Hash Extraction from Encrypted PDF
1. Download the target encrypted file `My Locked PDF1.pdf`.
2. Open the [Online HashCrack PDF Extractor](https://www.onlinehashcrack.com/tools-pdf-hash-extractor.php).
3. Upload `My Locked PDF1.pdf` to convert the PDF encryption headers into a crackable hash string formatted for `pdf2john`.
4. Copy the complete output string starting with `$pdf$...`.
5. Paste the hash into a new text document in Notepad and save it as `hash1.txt`.

#### Step 3: Cracking with Johnny GUI
1. Re-open **Johnny**.
2. Click **Open password file** -> **Open password file (HASHOUS format)** and choose `hash1.txt`.
3. Click **Start new attack**.
4. Once completed, the cracked password (`good-luck`) is displayed in the main table.
5. Open `My Locked PDF1.pdf` using the recovered password to access the protected content and capture the flag (`nw{networkwalks_flag1_jtr_270521_1}`).

---

## Module 2: Password Cracking with Networkwalks Web Tools

### Overview
Module 2 demonstrates browser-native hash parsing and dictionary-based recovery using web applications built on client-side WebAssembly/JavaScript execution.

### Step-by-Step Execution Guide

#### Step 1: Local Hash Generation
1. Navigate to the [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/).
2. Drag and drop or browse to select `My Locked PDF1.pdf`.
3. The browser tool parses the PDF structure locally without uploading raw sensitive data to a remote server, producing a hash string:
   ```text
   $pdf$4*4*128*-1060*1*16*55d1a5c14175da449753199e44971d32*32*777fd021a7f3c5ae598c0c6495c7f76e00000000000000000000000000000000*32*ceecdac74b19b5a62688d3b3524e1374c955cbb9cc3c45316494d9446ef81af1
