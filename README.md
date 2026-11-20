# Week3_networkwalks

# Week 3 – Password Cracking Project (Cybersecurity & Ethical Hacking)

This repository documents the Week 3 project tasks: recovering the password of a protected
PDF file (My Locked PDF1.pdf) using two different approaches.

## Modules

| Module | Tool(s) | Folder |
|---|---|---|
| Project Module 1 | John the Ripper (JTR) + Johnny GUI | module1-jtr/ |
| Project Module 2 | Networkwalks Hash Calculator + Password Cracker (web tools) | module2-networkwalks-tools/ |

## Background

Password cracking is used by security professionals to test password strength and to
demonstrate why weak passwords are a risk. Protected files (PDF, ZIP, Office docs) store
their password as a hash — a one-way scrambled representation of the password. The
general workflow in both modules is the same:

1. Extract the password hash from the locked PDF ($pdf$... format, compatible with
   John the Ripper / hashcat).
2. Run a cracking tool against the hash using a wordlist (dictionary attack).
3. Use the recovered password to open the PDF.

## Result

Both modules recover the same password for My Locked PDF1.pdf:

password1

## Ethics & Scope Note

This exercise is for learning purposes only, performed against a file provided by the
course for this lab. The same techniques should never be used against systems, accounts,
or files you do not own or do not have explicit authorization to test.

## Repository Structure

password-cracking-module/
├── README.md                          <- this file
├── module1-jtr/
│   └── README.md                      <- JTR / Johnny walkthrough
├── module2-networkwalks-tools/
│   └── README.md                      <- Networkwalks web tools walkthrough
└── sample-files/
    └── hash1.txt.example              <- example of the extracted $pdf$ hash format


================================================================

# Project Module 1 — Password Cracking with JTR (John the Ripper)

Task: Crack the password of My Locked PDF1.pdf using JTR John and the Johnny GUI on Windows.

## Tools

- John the Ripper (JTR) — cross-platform password cracking tool; can target many hash
  types and unlock password-protected PDF/ZIP/Office files.
- Johnny — graphical front end for John the Ripper, for users who prefer a point-and-click
  interface over the command line.
- Online Hash Extractor — used here to pull the crackable hash out of the PDF
  (equivalent to running pdf2john locally).

Note: On Kali Linux, John the Ripper is pre-installed, so steps 1–2 (download/install) are not needed.

## Steps

1. Install the tools
   - Download John the Ripper (jumbo build recommended — supports more hash/file types than core John).
   - Download and install the Johnny GUI.
   - In Johnny, go to Settings -> Browse and point it at john.exe (found in the run folder
     of the JTR download).

2. Extract the hash from the PDF
   - Upload My Locked PDF1.pdf to a PDF hash extractor tool (pdf2john-compatible).
   - Copy the resulting hash. It must start with $pdf$...
   - If the copied value has stray characters (e.g. a leading b'), strip them — the saved
     hash should start cleanly with $pdf$.
   - Paste the hash into Notepad and save it as hash1.txt.

3. Load the hash into Johnny
   - Open password file -> select hash1.txt.
   - Johnny will list one entry with the PDF hash and format PDF.

4. Run the attack
   - Click Start new attack.
   - John the Ripper runs its default cracking mode (dictionary/incremental) against the hash.
   - Progress and crack time depend on password complexity and CPU speed.

5. Retrieve the password
   - Once cracked, the password appears in the Password column of Johnny.
   - Result: password1

6. Unlock the PDF
   - Open My Locked PDF1.pdf in a PDF reader (e.g. Adobe Acrobat).
   - Enter password1 at the password prompt.
   - The file opens, confirming the password is correct.

## Notes

- john.exe lives under ...\john-X.X.X-jumbo-X-winXX\run\
- Johnny needs a valid path to John's executable before any attack can start — it will
  warn if the path is wrong.
- This same hash ($pdf$...) format is also what hashcat and the Networkwalks tools in
  Module 2 consume — the extraction step is identical conceptually.




# Project Module 2 — Password Cracking with Networkwalks Tools

Task: Crack the password of My Locked PDF1.pdf using the Networkwalks Hash Calculator
and Password Cracker browser-based tools (no installation required).

## Tools

- Networkwalks Hash Calculator — https://networkwalks.com/hash-calculator/
  Generates MD5/SHA-1/SHA-256/SHA-384/SHA-512 hashes, and can extract a crackable
  ($pdf$..., hashcat/pdf2john-compatible) hash from a password-protected PDF. Runs
  entirely client-side in the browser — no file upload.
- Networkwalks Password Cracker — https://networkwalks.com/password-cracker/
  Runs a dictionary attack against a pasted $pdf$ hash, using a built-in wordlist
  (or an uploaded custom wordlist).

## Steps

1. Get the locked file
   - Download My Locked PDF1.pdf from the lab page.

2. Extract the hash
   - Open the Hash Calculator tool.
   - Switch to the PDF tab and upload My Locked PDF1.pdf.
   - The tool parses the file locally in-browser and reports that it is encrypted, returning
     a crackable hash in pdf2john/hashcat format starting with $pdf$.
   - Copy the entire hash string — missing any part of it will cause the crack to fail.

3. Run the dictionary attack
   - Open the Password Cracker tool.
   - Paste the full $pdf$... hash into the input box.
   - Choose a wordlist: the built-in list (~100 common passwords) or upload a custom
     .txt wordlist.
   - Click Start Cracking.
   - The tool tries each word in the list against the hash and reports matches/misses live.

4. Get the result
   - On a match, the tool displays: Password Cracked Successfully -> password1

5. Open the PDF
   - Open My Locked PDF1.pdf and enter password1 when prompted.
   - The file unlocks.

## Notes

- Both tools are browser-based and compute everything client-side — no file or hash is
  uploaded to a server, per the tool's own description.
- The same $pdf$ hash format produced here is compatible with John the Ripper/hashcat,
  so this hash could equally be fed into the JTR workflow from Module 1.
- Cracking speed depends entirely on wordlist size/quality — a weak, dictionary-word
  password like password1 falls almost instantly; a long random password would not be
  found by a small built-in wordlist at all.

## Key Takeaway (both modules)

password1 is a textbook weak password: a dictionary word plus a single trailing digit.
Both the heavyweight JTR/Johnny workflow and the lightweight browser-based Networkwalks
tools cracked it in seconds, which is the point of the exercise — short, predictable
passwords offer almost no real protection.