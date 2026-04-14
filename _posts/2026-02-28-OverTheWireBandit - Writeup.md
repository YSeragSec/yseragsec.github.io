---
title: "OverTheWire Bandit — Level 0 to Level 10 Writeup"
date: 2026-02-28 00:00:00 +0200
categories: [Security Challenge Writeups, CAT]
tags: [linux, overthewire, bandit, writeup, find, grep, strings, base64, sort, uniq, ctf]
description: Writeup for OverTheWire Bandit levels 0 through 10. Covers basic Linux file reading, hidden files, find command, strings, sort/uniq, and base64 decoding.
last_modified_at: 2026-02-28 00:00:00 +0200
image:
  path: assets/img/posts/CATEntryLevel/Linux/OverTheWire2.png
---

<p style="font-size: 1.15em;"><a href="https://overthewire.org/wargames/bandit/">Bandit</a> is a wargame by OverTheWire designed for absolute beginners to learn Linux basics through practical challenges. Each level gives you a clue — you have to find the password for the next level.</p>

**Connection:** `ssh bandit0@bandit.labs.overthewire.org -p 2220`

---

## Level 0 → Level 1

**Goal:** The password is stored in a file called `readme` in the home directory.

```bash
cat readme
```

**Explanation:** `cat` reads and prints the contents of a file to the terminal.

![Bandit 00](/assets/img/posts/CATEntryLevel/Overthewire/img0.png)
![Bandit 00](/assets/img/posts/CATEntryLevel/Overthewire/img1.png)

 
---

## Level 1 → Level 2

**Goal:** The password is stored in a file called `-`.

```bash
cat ./-
```

**Explanation:** The dash (`-`) is interpreted by the shell as "read from stdin" instead of a file. Prefixing with `./` tells the shell it's a file path, not a flag.

![Bandit 01](/assets/img/posts/CATEntryLevel/Overthewire/img2.png)
![Bandit 01](/assets/img/posts/CATEntryLevel/Overthewire/img3.png)

---

## Level 2 → Level 3

**Goal:** The password is in a file called `spaces in this filename`.

```bash
cat "spaces in this filename"
```

Or using tab completion — type the first letter and press Tab.

![Bandit 02](/assets/img/posts/CATEntryLevel/Overthewire/img4.png)
![Bandit 02](/assets/img/posts/CATEntryLevel/Overthewire/img5.png)


---

## Level 3 → Level 4

**Goal:** The password is in a hidden file in the `inhere` directory.

```bash
cd inhere
ls -a
cat .hidden
```

**Explanation:** In Linux, files starting with `.` are hidden and won't show with a plain `ls`. The `-a` flag reveals them.

![Bandit 03](/assets/img/posts/CATEntryLevel/Overthewire/img6.png)
![Bandit 03](/assets/img/posts/CATEntryLevel/Overthewire/img7.png)


---

## Level 4 → Level 5

**Goal:** The password is in the only human-readable file in the `inhere` directory.

```bash
cd inhere
file ./-file*
cat ./-file07
```

**Explanation:** The `file` command identifies the type of each file. Use it with a wildcard to check all at once — the one labeled "ASCII text" is the human-readable one.

![Bandit 04](/assets/img/posts/CATEntryLevel/Overthewire/img8.png)
![Bandit 04](/assets/img/posts/CATEntryLevel/Overthewire/img9.png)

---

## Level 5 → Level 6

**Goal:** File is human-readable, 1033 bytes, and not executable.

```bash
find . -type f -readable ! -executable -size 1033c
```

**Explanation:** The `find` command with `-readable`, `! -executable`, and `-size 1033c` (c = bytes) narrows it down precisely.

![Bandit 05](/assets/img/posts/CATEntryLevel/Overthewire/img10.png)
![Bandit 05](/assets/img/posts/CATEntryLevel/Overthewire/img11.png)

---

## Level 6 → Level 7

**Goal:** File is owned by user `bandit7`, group `bandit6`, and is 33 bytes.

```bash
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

**Explanation:** Searching from `/` (root) covers the whole server. `2>/dev/null` redirects permission-denied errors to null so they don't clutter the output.

![Bandit 06](/assets/img/posts/CATEntryLevel/Overthewire/img12.png)
![Bandit 06](/assets/img/posts/CATEntryLevel/Overthewire/img13.png)

---

## Level 7 → Level 8

**Goal:** Password is in `data.txt` next to the word `millionth`.

```bash
grep "millionth" data.txt
```

**Explanation:** `grep` searches inside the file and returns only the matching line. Fast and simple.

![Bandit 07](/assets/img/posts/CATEntryLevel/Overthewire/img14.png)
![Bandit 07](/assets/img/posts/CATEntryLevel/Overthewire/img15.png)

---

## Level 8 → Level 9

**Goal:** Password is the only line that occurs exactly once in `data.txt`.

```bash
sort data.txt | uniq -u
```

**Explanation:** `sort` arranges lines alphabetically so duplicates are adjacent. `uniq -u` then prints only lines that appear exactly once.

![Bandit 08](/assets/img/posts/CATEntryLevel/Overthewire/img16.png)
![Bandit 08](/assets/img/posts/CATEntryLevel/Overthewire/img17.png)

---

## Level 9 → Level 10

**Goal:** Password is in `data.txt` among the human-readable strings, preceded by several `=` characters.

```bash
strings data.txt | grep "==="
```

**Explanation:** `strings` extracts all human-readable text from a binary file. Piping to `grep "==="` filters for lines starting with multiple `=` signs as described.

![Bandit 09](/assets/img/posts/CATEntryLevel/Overthewire/img18.png)
![Bandit 09](/assets/img/posts/CATEntryLevel/Overthewire/img19.png)

---

## Level 10 → Level 11

**Goal:** Password is in `data.txt` which contains base64-encoded data.

```bash
cat data.txt | base64 -d
```

**Explanation:** `base64 -d` decodes base64-encoded text back to its original form.

![Bandit 10](/assets/img/posts/CATEntryLevel/Overthewire/img20.png)
![Bandit 10](/assets/img/posts/CATEntryLevel/Overthewire/img21.png)

---

***If you have any questions, feel free to reach out on [LinkedIn](https://www.linkedin.com/in/yousefserag/) or [Discord](https://discord.com/users/751180791492378774)***
