# My Learning Report: Navigating Linux, Networking, and Git Security
**Platform:** OverTheWire (Bandit Game, Levels 0 to 33 - COMPLETE)  
**Objective:** Learn how to use basic to intermediate Linux commands to move around a system, find hidden data, interact with network lines, and check Git files.

---

## What I Learned (Core Concepts)

In cybersecurity, most servers do not have a visual desktop. You have to type commands to do everything. This project taught me how to find information quickly across a massive system.

### Important Skills Learned:
* **Advanced File Searching:** How to search the entire computer for a file based on its exact size, owner, or folder permissions.
* **Error Hiding:** How to use `2>/dev/null` to hide annoying "Permission Denied" text errors so they do not clutter the screen.
* **Text Filtering (`grep`):** How to extract a single specific word out of a file containing thousands of sentences.
* **Finding Unique Data:** How to sort text files and delete matching duplicate lines to isolate a single unique secret.
* **Inspecting Binary Files:** Using `strings` to see human-readable words hidden inside raw machine code files.
* **Data Coding & Ciphers:** Unpacking encoded Base64 strings and solving character-shifted text layouts like ROT13.
* **Archive Extraction Loops:** Peeling back nested layers of compression types like `gzip`, `bzip2`, and `tar`.
* **Network Communications:** Checking server ports using `nmap`, sending unencrypted lines using Netcat (`nc`), and using OpenSSL for encrypted communication.
* **Task Automation:** Auditing scheduled system background processes (Cron jobs) and handling automation using bash scripts.
* **Git Code Databases:** Recovering deleted history tracking data via `git log`, checking alternative development branches, and inspecting code labels (Git tags).
* **Shell Escaping:** Outsmarting locked-down user shell profiles to drop back into standard default command lines.

---

## Step-by-Step Walkthrough

### Level 0 -> Level 5: Basic File Handling & Navigation Oddities
* **What I did:** Learned standard command navigation. Handled strange file names with relative pathing (`./-`), escaped spaces with backslashes (`\ `), exposed unindexed configurations (`ls -a`), and verified file identities via signature byte lookups (`file`).
* **Passwords Tracker:**
  * Level 1: `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR`
  * Level 2: `PK8fYLZg2hnHSz83plBL1iEPKdD3QToB`
  * Level 3: `7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME`
  * Level 4: `xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq`
  * Level 5: `6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG`

---

### Level 5 -> Level 10: System-Wide Searching and Text Filtering
* **What I did:** Used the `find` engine to track item footprints via exact metrics. Hid terminal errors (`2>/dev/null`), parsed out text words among chaotic lines (`grep`), and dropped repeating duplicates (`sort | uniq -u`).
* **Passwords Tracker:**
  * Level 6: `pXa26xhMWaC2SvDotA4r9EgZkulOeSBW`
  * Level 7: `Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3`
  * Level 8: `VR1ljMayciFxbnUokuQmJFw6QC9VKtub`
  * Level 9: `EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl`
  * Level 10: `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`

---

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
    cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-m"
    ```
* **Password Found:** `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN`

---

### Level 12 -> Level 13: Processing Nested Compressed Files
* **Goal:** Uncover a secret hidden inside a binary file packed inside multiple layers of different compression tools.
* **What I did:** Created a temporary workspace in `/tmp/work`. Converted the hex file back to a raw binary file using `xxd -r`. Kept testing file types via `file` and peeled back layers sequentially using `gunzip`, `bunzip2`, and `tar -xf`.
* **Commands I used:**
    ```bash
    xxd -r data.txt > data
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
    scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private .
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
* **What I did:** Netcat fails when connecting to encrypted lines. I used OpenSSL's socket module to handle the secure data exchange and submitted the verification token over port 30001 with `-quiet` to clean up text output.
* **Commands I used:**
    ```bash
    echo "pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7" | openssl s_client -connect localhost:30001 -quiet
    ```
* **Password Found:** `kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V`

---

### Level 16 -> Level 17: Port Scanning & RSA Key Collection
* **Goal:** Scan a range of hidden internal channels to find an encrypted authentication door.
* **What I did:** Scanned the network using `nmap` across lines 31000-32000. Used the version detection flag `-sV` to discover that port 31790 was running an SSL line. Sent my token to it, printed out a raw OpenSSH Private Key, and saved it into a local key file.
* **Commands I used:**
    ```bash
    nmap -p 31000-32000 localhost
    nmap -p 31046,31518,31691,31790,31960 -sV localhost
    echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | openssl s_client -connect localhost:31790 -quiet
    chmod 600 sshkey16
    ```
* **Password Found:** An OpenSSH Private key used to connect directly into Level 17.

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
    ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
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

---

### Level 20 -> Level 21: Parallel Processing & Network Listeners
* **Goal:** Submit a password token to a strict validation software using a network line.
* **What I did:** The validation software required another port to give it the password first. I started a local network listener using Netcat (`nc -l -p`), used the ampersand symbol (`&`) to push it into the background so my screen didn't freeze, and then executed the connector tool.
* **Commands I used:**
    ```bash
    echo "4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA" | nc -l -p 4444 &
    ./suconnect 4444
    ```
* **Password Found:** `bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY`

---

### Level 21 -> Level 22: Investigating Automated Cron Jobs
* **Goal:** Discover how the server runs automatic routines in the background to print out files.
* **What I did:** Checked the system scheduler configuration directory (`/etc/cron.d/`). Found an active task sheet pointing to a hidden script file, read the code inside it, and located where it was outputting the next credential token.
* **Commands I used:**
    ```bash
    cd /etc/cron.d/
    cat cronjob_bandit22
    cat /usr/bin/cronjob_bandit22.sh
    cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
    ```
* **Password Found:** `RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz`

---

### Level 22 -> Level 23: Reversing Variable MD5 Hashes
* **Goal:** Find a password text hidden inside a folder name generated randomly by a background script.
* **What I did:** Audited an automated script file that converts usernames into hidden MD5 hash strings. Manually recreated that exact same hash command line using the target name `bandit23` to reveal the exact filename inside `/tmp`.
* **Commands I used:**
    ```bash
    cat /usr/bin/cronjob_bandit23.sh
    echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
    cat /tmp/8ca319486bfbbc3663ea0fbe81326349
    ```
* **Password Found:** `gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw`

---

### Level 23 -> Level 24: Exploiting Automated Script Execution Folders
* **Goal:** Write a custom script that the server will automatically run with elevated privileges.
* **What I did:** Discovered a routine task that automatically executes and deletes any script placed inside a special folder (`/var/spool/bandit24/foo`). Created a new workspace folder in `/tmp`, wrote a basic script to copy the target password out, gave it full entry privileges, and copied it into the task directory.
* **Commands I used:**
    ```bash
    cat /usr/bin/cronjob_bandit24.sh
    mkdir -p /tmp/nain_space
    echo '#!/bin/bash' > /tmp/nain_space/get_pass.sh
    echo 'cat /etc/bandit_pass/bandit24 > /tmp/nain_space/pass.txt' >> /tmp/nain_space/get_pass.sh
    chmod +x /tmp/nain_space/get_pass.sh
    cp /tmp/nain_space/get_pass.sh /var/spool/bandit24/foo/
    cat /tmp/nain_space/pass.txt
    ```
* **Password Found:** `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv`

---

### Level 24 -> Level 25: Automated Network Brute-Forcing
* **Goal:** Crack a network server lock requiring a password and a matching 4-digit PIN code.
* **What I did:** Guessing 10,000 variations manually is impossible. I wrote a fast command-line `for` loop that instantly printed all options combined with the level password into a single test file (`brute.txt`). Piped the whole list into port 30002 and dropped wrong errors using `grep -v`.
* **Commands I used:**
    ```bash
    for pin in {0000..9999}; do echo "hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv $pin"; done > /tmp/nain_space/brute.txt
    cat /tmp/nain_space/brute.txt | nc localhost 30002 | grep -v "Wrong"
    ```
* **Password Found:** `SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P`

---

### Level 25 -> Level 26: Intercepting Connection Keys
* **Goal:** Download a secret access key file hosted on the target level.
* **What I did:** Discovered a private configuration folder containing a login key named `bandit26.sshkey`. Used the secure copy protocol (`scp`) to extract it from the server and put it on my home machine, adjusted its usage privileges, and logged in.
* **Commands I used:**
    ```bash
    scp -P 2220 bandit25@bandit.labs.overthewire.org:~/bandit26.sshkey .
    chmod 600 bandit26.sshkey
    ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
    ```
* **Password Found:** Secure connection file `bandit26.sshkey` used to bypass standard password entry.

---

### Level 26 -> Level 27: Escaping Restrictive Shell Environments
* **Goal:** Break out of a text shell that crashes your session instantly.
* **What I did:** The bandit26 account used a restrictive script profile. I shrank my local terminal window size before logging in. This forced the text reader to freeze the interface into a `more` file reader. Hit `v` to open full text editing mode, typed `:set shell=/bin/bash`, and initialized a clean environment using `:shell`.
* **Password Found:** Passed through system layout manipulation via a text reader.

---

### Level 27 -> Level 28: Investigating Git Repository Logs
* **Goal:** Recover a credential password deleted from a production file directory.
* **What I did:** Cloned the local development repository into a temporary space. Inspected the tracking logs using `git log` and printed out historical file changes using `git show` to view the deleted password entry.
* **Commands I used:**
    ```bash
    mkdir -p /tmp/nain_git && cd /tmp/nain_git
    git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
    git log
    git show 83d77407b76c9f86ac4e691a47618641c9d527ba
    ```
* **Password Found:** `Em7eGtqaMySwNFjCpwzzHhLhospOcdt0`

---

### Level 28 -> Level 29: Auditing Git Revision History Drops
* **Goal:** Uncover data masked by fake placeholder symbols (`xxxxxxxxxx`).
* **What I did:** Cloned the code database. Checked all previous modification logs and looked at the exact code difference to find the text string before it was replaced with placeholders.
* **Commands I used:**
    ```bash
    git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
    git log -p README.md
    ```
* **Password Found:** `y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ`

---

### Level 29 -> Level 30: Hunting Across Alternative Git Branches
* **Goal:** Extract a password hidden inside a different branch of the development project.
* **What I did:** Main master branch contained an info leak notice. I searched for all hidden development branches using `git branch -a`, found a branch named `dev`, switched my workspace over to it, and read the file.
* **Commands I used:**
    ```bash
    git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
    git branch -a
    git checkout dev
    cat README.md
    ```
* **Password Found:** `jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX`

---

### Level 30 -> Level 31: Extracting Hidden Git Tags
* **Goal:** Find production keys hidden somewhere inside an empty project directory.
* **What I did:** The branch appeared to have no files. I checked the internal deployment tags using `git tag`. Discovered a tag named `secret` and extracted its details using the show command.
* **Commands I used:**
    ```bash
    git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
    git tag
    git show secret
    ```
* **Password Found:** `82NkymblpGBYmIXG6ZQ8YldBYstHpfUf`

---

### Level 31 -> Level 32: Submitting Validated Remote Repository Commits
* **Goal:** Push a newly created validation text file back up to the server repository.
* **What I did:** Cloned the level 31 project. Created a new text file named `key.txt` filled with the exact string requested, staged the file addition into Git, committed it locally, and pushed it to the remote server to trigger the validation code.
* **Commands I used:**
    ```bash
    git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
    echo 'May I come in?' > key.txt
    git add -f key.txt
    git commit -m "Adding the requested key file"
    git push origin master
    ```
* **Password Found:** `pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT`

---

### Level 32 -> Level 33: Bypassing Uppercase Command Shell Filters
* **Goal:** Escape a custom system shell that converts all your characters to capitals.
* **What I did:** Any command I typed was shifted into uppercase letters, breaking standard commands. I invoked a built-in positional variable statement (`$0`) which refers to the original execution engine. This bypassed the filter and dropped me into a standard bash environment.
* **Commands I used:**
    ```bash
    $0
    cat /etc/bandit_pass/bandit33
    ```
* **Password Found:** `u4P2CyPOwPGLe94RdD9Uo2FxFwvnFswM` (Final Victory Flag!)

---

## Complete Credential Matrix (Levels 0 to 33)

| From Level | To Level | Password Token / Flag |
| :--- | :--- | :--- |
| **Bandit 0** | Bandit 1 | `6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR` |
| **Bandit 1** | Bandit 2 | `PK8fYLZg2hnHSz83plBL1iEPKdD3QToB` |
| **Bandit 2** | Bandit 3 | `7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME` |
| **Bandit 3** | Bandit 4 | `xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq` |
| **Bandit 4** | Bandit 5 | `6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG` |
| **Bandit 5** | Bandit 6 | `pXa26xhMWaC2SvDotA4r9EgZkulOeSBW` |
| **Bandit 6** | Bandit 7 | `Bmnnvf82KzQlfxgAI2d1zYbr1u9pr3E3` |
| **Bandit 7** | Bandit 8 | `VR1ljMayciFxbnUokuQmJFw6QC9VKtub` |
| **Bandit 8** | Bandit 9 | `EjmOSvuAu7sGAHqHVcBDPirRe9T03kxl` |
| **Bandit 9** | Bandit 10| `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG` |
| **Bandit 10**| Bandit 11| `pYfOY6HwUsDj5rL9UvyhU7MCmv8vN5Ro` |
| **Bandit 11**| Bandit 12| `GROozWPO8QyN0mGrjUkID0WCYkZiQxrN` |
| **Bandit 12**| Bandit 13| `qQYQiHOBPR8zR61qxYqX45quvihF2uzk` |
| **Bandit 13**| Bandit 14| `aaWecNkG4FhxJQxz07uiwzVP6bJiYS65` |
| **Bandit 14**| Bandit 15| `pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7` |
| **Bandit 15**| Bandit 16| `kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V` |
| **Bandit 16**| Bandit 17| (OpenSSH Private RSA Connection Key) |
| **Bandit 17**| Bandit 18| `OQxXZjELndr90zuhOTDYBEomI0SZITXI` |
| **Bandit 18**| Bandit 19| `KpsOfPkcP7i1FlIExk2QEjyt6dw8dxZI` |
| **Bandit 19**| Bandit 20| `4pIjcunZ0fK2vmp3IwfG8Vf7VhxD6pOA` |
| **Bandit 20**| Bandit 21| `bW9kBv5WC3P4yoDyf12LSdGuNz5ka6hY` |
| **Bandit 21**| Bandit 22| `RYVux2rHEm9tiXHmLFzuR7Vhx6AZQMEz` |
| **Bandit 22**| Bandit 23| `gKXDTAXnIz3OBxiPjRZ2uqutUlPZrBsw` |
| **Bandit 23**| Bandit 24| `hVQMk3lJNsmQ7VF3ubyrNNBom7BOgVXv` |
| **Bandit 24**| Bandit 25| `SoHfqMOEqIX2IYKVciZxvgpR9a2Djx4P` |
| **Bandit 25**| Bandit 26| (OpenSSH Private Key for bandit26) |
| **Bandit 26**| Bandit 27| `STJLJBRRphMxKB392CT4iOr5CbzPU9ER` |
| **Bandit 27**| Bandit 28| `y8Yd2ssKcpHpud7UvOSOxwamRMzIGIeQ` |
| **Bandit 28**| Bandit 29| `x2g9snNhZ18761A6b1wzwvG7VPhxD6pO` |
| **Bandit 29**| Bandit 30| `jq9Dfg2rXsfYsWMgFuKlXhphjdH7USgX` |
| **Bandit 30**| Bandit 31| `82NkymblpGBYmIXG6ZQ8YldBYstHpfUf` |
| **Bandit 31**| Bandit 32| `pWuj5jBQ6IgV0NXwiH6g1pXRF8S1YvbT` |
| **Bandit 32**| Bandit 33| `u4P2CyPOwPGLe94RdD9Uo2FxFwvnFswM` |
