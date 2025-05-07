# Hashcat-Intro

## Objective

Learn and practice the fundamentals of hash generation and password cracking using tools like Hashcat and John the Ripper, with a focus on improving clarity in explanation, workflow discipline, and understanding real-world cybersecurity implications.

### Skills Learned

- Generated hashes using multiple algorithms: MD5 , SHA-256 , bcrypt , and SHA-512crypt
- Identified hash types by analyzing prefixes ($2b$, $6$, etc.) using hashid and online tools
- Cracked hashes using dictionary attacks with Hashcat and John the Ripper
- Understood how salts affect hash uniqueness in bcrypt and SHA-512crypt
- Practiced troubleshooting common issues like extra characters in hash files and compressed wordlists
- Improved terminal and scripting skills using Python one-liners and Linux commands
- Gained awareness of password security and the importance of strong authentication practices
- Documented technical processes clearly and systematically

### Tools Used
[Bullet Points - Remove this afterwards]

- Kali Linux – For penetration testing and command-line operations
- Hashcat – Advanced password recovery tool for GPU-based hash cracking
- John the Ripper (JtR) – Fast, CPU-based password cracker
- hashid – Tool to identify hash types based on format
- rockyou.txt – Popular wordlist for dictionary attacks
- Python 3 + bcrypt – For generating salted bcrypt hashes
- mkpasswd – Utility for generating SHA-512crypt hashes
- md5sum / sha256sum – Utilities for generating fast hashes
- Online tools – Such as hashes.com for hash identification and decryption attempts

## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

1. Generating Hashes

Created hashes using multiple algorithms:

MD5 : echo -n "fuckyou" | md5sum > file

SHA-256 : echo -n "spongebob" | sha256sum > file

bcrypt : Python script using bcrypt.hashpw()

SHA-512crypt : mkpasswd --method=sha-512 princess

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

5. Revealing the Magic Sentence
6. 
Successfully cracked all four hashes:

fuckyou
spongebob
iloveyou
princess


Example below.

*Ref 1: Network Diagram*
