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
    ```
* **Password Found:** `B0s2khmbT9u0geKuOoVGW3JZKhndE3BG`
