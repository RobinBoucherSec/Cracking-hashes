# 🎯 Hashcat-Intro

First, note that I've created a comprehensive and detailed Notion's guide of Hashcat that I suggest to you if you're interested in understading the fundamentals of hashing. It is called [Intro to Hashcat - Making & Cracking Hashes](https://bold-top-b0e.notion.site/Intro-to-Hashcat-making-cracking-hashes-1e2d8ff66ad2808c910fd25b868c9840?pvs=4). 

## 📝 Description
In this project, I gained hands-on experience with hashing techniques and password cracking using tools such as Hashcat , John the Ripper , and hashid . I generated hashes using multiple algorithms (MD5, SHA-256, bcrypt, SHA-512crypt), analyzed hash formats, and performed dictionary attacks to simulate real-world password recovery scenarios.

## 🎯 Objective
This lab aimed to build foundational knowledge in password security, improve technical documentation skills, and develop a structured approach to using cybersecurity tools effectively in both offensive and defensive contexts.

### Tools Used

- Notion – For deep documentation
- Kali Linux – For penetration testing and command-line operations
- Hashcat – Advanced password recovery tool for GPU-based hash cracking
- John the Ripper (JtR) – Fast, CPU-based password cracker
- hashid – Tool to identify hash types based on format
- rockyou.txt – Popular wordlist for dictionary attacks
- Python 3 + bcrypt – For generating salted bcrypt hashes
- mkpasswd – Utility for generating SHA-512crypt hashes
- md5sum / sha256sum – Utilities for generating fast hashes
- Online tools – Such as hashes.com for hash identification and decryption attempts

### Skills Learned
- Generated hashes using multiple algorithms: MD5 , SHA-256 , bcrypt , and SHA-512crypt
- Identified hash types by analyzing prefixes ($2b$, $6$, etc.) using hashid and online tools
- Cracked hashes using dictionary attacks with Hashcat and John the Ripper
- Understood how salts affect hash uniqueness in bcrypt and SHA-512crypt
- Practiced troubleshooting common issues like extra characters in hash files and compressed wordlists
- Improved terminal and scripting skills using Python one-liners and Linux commands
- Gained awareness of password security and the importance of strong authentication practices
- Documented technical processes clearly and systematically

## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc

1. Generating Hashes

First we will create the file location for all the hashes we will create:

```bash
mkdir -p ~/projects/hashes/
```

Created hashes using multiple algorithms:

```bash
MD5 : echo -n "poop" | md5sum > ~/projects/hashes/md5_hash.txt
```
```bash
SHA-256 : echo -n "spongebob" | sha256sum > ~/projects/hashes/sha256_hash.txt
```
```bash
sudo apt install python3 python3-pip SEPARÉ OU PAS ? TESTER DANS VM
pip3 install bcrypt
```
```bash
SHA-512crypt : mkpasswd --method=sha-512 princess
```


3. Understanding Hash Types

Learned to recognize hash formats by their prefixes:
$2b$ → bcrypt

$6$ → SHA-512crypt

$1$ → MD5crypt

$y$ → yescrypt

Used hashid and online tools to help identify unknown hashes.

5. Cracking Hashes

Performed dictionary attacks using rockyou.txt:

Identified each hash type

Used correct Hashcat hash modes (-m)

MD5 → -m 0

SHA-256 → -m 1400

bcrypt → -m 3200

SHA-512crypt → -m 1800

Executed commands like:

bash


1
hashcat -m 3200 -a 0 bcrypt_hash.txt /usr/share/wordlists/rockyou.txt

4. Troubleshooting

Fixed common issues:

Removed extra characters from hash files

Decompressed .gz versions of rockyou.txt

Manually identified hashes when automatic detection failed

## 🔙 Back to Portfolio
[⬅️ Back to my Cybersecurity Portfolio](https://github.com/RobinBoucherSec/RobinBoucherSec)

