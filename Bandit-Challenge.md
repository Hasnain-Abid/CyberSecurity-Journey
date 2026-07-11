# My Learning Report: Navigating Linux and Finding Files
Platform:OverTheWire (Bandit Game)  
Objective:Learn how to use basic Linux commands to move around a system, find hidden files, and read information without using a mouse.

**What I Learned (Core Concepts)**
In cybersecurity, most servers do not have a visual desktop. You have to type commands to do everything. This project taught me how to talk to a Linux computer using text.

# Important Skills Learned:
**Moving Around:** How to change folders and check where I am standing.
**Fixing Broken Names:** How to open files that have strange names (like spaces or dashes) that usually confuse the computer.
**Finding Hidden Items:** How to see files that the computer tries to hide.
**Checking File Types:** How to ask the computer if a file contains readable text or unreadable computer code.

---

# Step-by-Step Walkthrough

# Level 0 -> Level 1: Logging In and Looking Around
**Goal:** Log into the remote computer and find the first password.
**What I did:** I used the `ssh` command to connect to the game server. Then I checked my location, looked at the files, and read the "readme" file.
**Commands I used:**
    ssh bandit0@bandit.labs.overthewire.org -p 2220
    pwd
    ls
    cat readme
 **Password Found:** `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`


# Level 1 -> Level 2: Reading a Dash File
**Goal:** Read a file whose name is just a single dash `-`.
**What I did:** Usually, a dash means a command option, which confuses Linux. To fix this, I added `./` in front of the dash (`./-`). This tells the computer: "Look inside this current folder for a file named dash."
**Commands I used:**
    ls
    cat ./-
**Password Found:** `PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`

---

# Level 2 -> Level 3: Reading Files with Spaces
**Goal:** Open a file that has spaces in its name (`spaces in this filename`).
**What I did:** Linux gets confused by spaces because it thinks they are separate commands. I used a backslash `\` before each space to tell the computer that the whole line is just one single filename.
**Commands I used:**
    ls
    cat ./--spaces\ in\ this\ filename--
**Password Found:** `7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME`

---

### Level 3 -> Level 4: Finding Hidden Files
**Goal:** Find a file hidden inside a folder called `inhere`.
**What I did:** I moved into the folder using `cd`. When I typed `ls`, nothing showed up. I used `ls -la` to show hidden files, and found a file starting with dots.
**Commands I used:**
    cd inhere/
    ls -a
    cat ...Hiding-From-You
**Password Found:** `xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq`

# Level 4 -> Level 5: Finding the Only Readable File
**Goal:** Find the one readable text file inside a folder full of unreadable computer files.
**What I did:** There were 10 files. Instead of opening all of them, I used the `file` command on everything (`./*`). It showed me that file number 7 was the only plain text file.
**Commands I used:**
    cd inhere/
    file ./*
    cat ./-file07
**Password Found:** `6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG`

### Level 5 -> Level 6: Using Advanced Filtering
* **Goal:** Find a specific file hidden deep inside many folders that is exactly 1033 bytes big and not executable.
* **What I did:** I used the `find` command with flags to search for files (`-type f`), specific sizes (`-size 1033c`), and left out executable choices (`! -executable`).
* **Commands I used:**
    ```bash
    find . -type f -size 1033c ! -executable
    cat ./maybehere07/.file2
    ```
* **Password Found:** `pXa26xhMWaC2SvDotA4r9EgZkulOeSBW`

---

### Level 6 -> Level 7: Searching the Entire System
* **Goal:** Find a file belonging to user `bandit7` and group `bandit6` that is exactly 33 bytes big.
* **What I did:** I searched from the absolute root directory (`/`). I added `2>/dev/null` at the end of my command to hide all the "Permission Denied" errors, leaving only the successful file match on my screen.
* **Commands I used:**
    ```bash
    find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
    cat /var/lib/dpkg/info/bandit7.password
    ```
* **Password Found:** `Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3`

---

### Level 7 -> Level 8: Finding a Word in a Sea of Text
* **Goal:** Extract the password inside a huge text file called `data.txt` right next to the word "millionth".
* **What I did:** Instead of scrolling through thousands of lines, I used `grep` to scan the file and pull out only the line containing "millionth".
* **Commands I used:**
    ```bash
    grep "millionth" data.txt
    ```
* **Password Found:** `VR1ljMayciFxbnUokuQmJFw6QC9VKtub`

---

### Level 8 -> Level 9: Sorting and Dropping Duplicates
* **Goal:** Find the only line of text in `data.txt` that occurs exactly once. All other lines are duplicates.
* **What I did:** The `uniq` command only works if identical lines are touching. So, I first used `sort` to line up all duplicate text rows together, and then piped (`|`) that text into `uniq -u` to delete all repeating data.
* **Commands I used:**
    ```bash
    sort data.txt | uniq -u
    ```
* **Password Found:** `EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl`

---

### Level 9 -> Level 10: Extracting Readable Strings
* **Goal:** Read a password hidden inside a corrupted, unreadable binary file.
* **What I did:** Running `cat` on a binary file floods the screen with broken symbols. I used the `strings` tool to extract only the human-readable text parts, then used `grep` to look for the password lines starting with multiple `=` marks.
* **Commands I used:**
    ```bash
    strings data.txt | grep "===="

    ### Level 10 -> Level 11: Decoding Base64
* **Goal:** Extract clear text from a file encoded in Base64 layout.
* **What I did:** Ran the base64 reverse program to unpack the file content back into normal characters.
* **Commands I used:**
    ```bash
    base64 -d data.txt
    ```
* **Password Found:** `pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro`

---

### Level 11 -> Level 12: Translating ROT13 Scrambled Data
* **Goal:** Reconstruct readable text that had its characters shifted 13 places.
* **What I did:** Used the character translation filter `tr` to map lowercase and uppercase strings backward by 13 positions.
* **Commands I used:**
    ```bash
    cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-n"
    ```
* **Password Found:** `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN`

---

### Level 12 -> Level 13: Processing Nested Compressed Files
* **Goal:** Uncover a secret hidden inside a binary file packed inside multiple layers of different compression tools.
* **What I did:** Created a temporary workspace in `/tmp`. Converted the hex file back to a raw binary file using `xxd -r`. Kept testing file types via `file` and peeled back layers sequentially using `gunzip`, `bunzip2`, and `tar -xf`.
* **Commands I used:**
    ```bash
    xxd -r data.txt data
    file data
    mv data data.gz && gunzip data.gz
    mv data data.bz2 && bunzip2 data.bz2
    tar -xf data
    ```
* **Password Found:** `qQYQiHOBPR8zR61qxYqX45quvihF2uzk`

---

### Level 13 -> Level 14: Private Key Authentication
* **Goal:** Authenticate to the next level without using a typed password string.
* **What I did:** Discovered a private SSH key file inside the directory. Securely moved it onto my home machine using `scp`, restricted its read access rules via `chmod 600`, and initialized an identity-linked connection loop.
* **Commands I used:**
    ```bash
    scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ~/
    chmod 600 sshkey.private
    ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
    ```
* **Password Found:** `aaWecNkG4FhxJQxz07uiwzVP6bJiYS65` (stored in /etc/bandit_pass/bandit14)

---

### Level 14 -> Level 15: Submitting Tokens via Network Connections
* **Goal:** Send the current password across a specific internal connection link to verify entry.
* **What I did:** Passed the password token across a plain text pipeline into a local connection port running on channel 30000 using Netcat (`nc`).
* **Commands I used:**
    ```bash
    echo "aaWecNkG4FhxJQxz07uiwzVP6bJiYS65" | nc localhost 30000
    ```
* **Password Found:** `pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7`

---

### Level 15 -> Level 16: Interacting with Encrypted SSL Services
* **Goal:** Transmit a password over an encrypted connection port.
* **What I did:** Netcat fails when connecting to encrypted lines. I used OpenSSL's socket module to handle the secure data exchange and submitted the verification token over port 30001.
* **Commands I used:**
    ```bash
    echo "pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7" | openssl s_client -connect localhost:30001 -quiet
    ```
* **Password Found:** `kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`

---

### Level 16 -> Level 17: Port Scanning & RSA Key Collection
* **Goal:** Scan a range of hidden internal channels to find an encrypted authentication door.
* **What I did:** Scanned the network using `nmap` across lines 31000-32000. Found port 31790 running an SSL line, sent my token to it, and printed out a raw OpenSSH Private Key. Saved it into a new local file called `sshkey16` and modified permissions.
* **Commands I used:**
    ```bash
    nmap -p 31000-32000 localhost
    echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | openssl s_client -connect localhost:31790 -quiet
    nano sshkey16
    chmod 600 sshkey16
    ```
* **Password Found:** An OpenSSH Private key to access Level 17.

---

### Level 17 -> Level 18: Locating Operational File Changes
* **Goal:** Spot the single modified entry inside two massive configuration texts.
* **What I did:** Ran the file comparison command `diff` between `passwords.new` and `passwords.old` to instantly grab the line that was modified.
* **Commands I used:**
    ```bash
    diff passwords.new passwords.old
    ```
* **Password Found:** `OQxXZjELndr90zuhOTDYBEomI0SZITXI`

---

### Level 18 -> Level 19: Bypassing Shell Disconnection Scripts
* **Goal:** Connect to a server account that immediately logs you out when your session starts.
* **What I did:** The account was running a custom script that closes the door right away. I appended the `"cat readme"` command at the very end of my terminal login request, which forced the server to process my command before the script kicked me out.
* **Commands I used:**
    ```bash
    ssh -i sshkey16 bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
    ```
* **Password Found:** `KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI`

---

### Level 19 -> Level 20: Exploiting SUID Permissions
* **Goal:** Read a locked password configuration document without owning system-level keys.
* **What I did:** Found an executable file (`bandit20-do`) configured with a SUID bit. This file runs with the authority of user `bandit20` even when run by user `bandit19`. I passed `cat` through this file to read the locked password folder.
* **Commands I used:**
    ```bash
    ./bandit20-do cat /etc/bandit_pass/bandit20
    ```
* **Password Found:** `4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA`
    ```
* **Password Found:** `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`
