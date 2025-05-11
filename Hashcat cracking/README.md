
# 🎯 Hashcat cracking in Kali Linux

See here the detailed version on Nortion. It is called [Generating Hashes in Kali Linux](https://bold-top-b0e.notion.site/Generating-Hashes-in-Kali-Linux-1e2d8ff66ad28018b06fd12cdaca6c06). 

### 📝 Description
In this project, I gained hands-on experience with hashing techniques and password cracking using different tools. I analysed the hash format of the hashes I've generated in [this repository](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Generating%20Hashes/README.md) and performed dictionary attacks to simulate real-world password recovery or cracking scenarios.

### 🎯 Objective
This lab aimed to build foundational knowledge in password security, improve technical documentation skills, and develop a structured approach to using cybersecurity tools effectively in both offensive and defensive contexts.

### Tools Used

- Notion – For deep documentation
- Kali Linux – For penetration testing and command-line operations
- Hashcat – Advanced password recovery tool for GPU-based hash cracking
- hashid – Tool to identify hash types based on format
- rockyou.txt – Popular wordlist for dictionary attacks
- GZip
- Online tools (hashes.com ...)

  
### Skills Learned

- Identified hash types by analyzing prefixes ($2b$, $6$, etc.) using hashid and online tools
- Cracked hashes using dictionary attacks with Hashcat
- Understood how salts affect hash uniqueness in bcrypt and SHA-512crypt
- Practiced troubleshooting common issues like extra characters in hash files
- Basic terminal and scripting skills, including Python one-liners and Linux command
- Gained awareness of password security and the importance of strong authentication practices
- Documented technical processes clearly and systematically

## Steps Overview

[Understand and Identify the hash](#Understand-and-Identify-the-hash)

[Time to CRACK 😱](#Time-to-CRACK-😱)

[Id and Crack md5](#Id-and-Crack-md5)

[Id and Crack sha256](#Id-and-Crack-sha256)

[Id and Crack sha512crypt](#Id-and-Crack-sha512crypt)

[Id and Crack bcrypt](#Id-and-Crack-bcrypt)


# Steps begin here

### Understand and Identify the hash

<details><summary>Here is how to understand the hash</summary>
  
- In this project, we have stored the hashes in /projects/hashes but usually in Linux the passwords are stored in /etc/shadow and are normally only readable by root.
  
- Iwill find each line in this format:

username:$prefix$options$salt$hash:last_change:min_age:max_age:warn_age:inactive_expire:expire_date:reserved

- And here is is an exemple:

alice:$6$rounds=5000$abcDEF$/OOSg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4:18698:7:90:7:30:2024-12-31::

- We will focus on the encrypted passwod (hashed passphrase):

$prefix$options$salt$hash

- 6 indicates the hash algorithm used, SHA-512
  
- rounds=5000 is a parameter passed to the algorithm


- abcDEF is the salt used
  
- /OOSg64dhfF.TigVPdzqiFang6uZA4QA1pzzegKdVm4 is the hash value

More info can be found by executing man 5 shadow.
</details>

<details><summary>Here is how to Identify the type of hash</summary>
  
To know what type of hash, Ineed to look at the $prefix$ and know it instantly, like in the following list. 

Or Ican use the hashid tool (recommended) or the hash-identifier (built-in script in kali linux).

### `$y$` — yescrypt

- Type : Scalable password hashing scheme.
- Use Case : Default/recommended in modern systems for secure password storage.
- Features : Designed to resist GPU/ASIC cracking attacks by requiring large memory usage.

### `$gy$` — gost-yescrypt

- Type : Hybrid of GOST R 34.11-2012 (Russian standard) + yescrypt.
- Use Case : Combines yescrypt’s memory-hardness with GOST’s cryptographic strength.

### `$7$` — scrypt

- Type : Password-based key derivation function.
- Use Case : Prioritizes memory hardness to thwart brute-force attacks.
- Origin : Common in cryptocurrency wallets (e.g., Litecoin, Ethereum).

### `$2a$`, `$2x$`, `$2b$`, `$2y$` — bcrypt

- Type : Blowfish-based adaptive hash.
- Variants :
    - `$2a$`: Original OpenBSD implementation.
    - `$2x$`/`$2y$`: Experimental/interim versions (rarely used).
    - `$2b$`: Standardized bcrypt (widely adopted).
- Use Case : Legacy and modern Unix-like systems (Linux, FreeBSD, Solaris).
- Strength : Slows cracking via iterative hashing and salting.

### `$6$` — sha512crypt

- Type : SHA-512-based hash.
- Use Case : Default on older Linux systems (via GNU libc).
- Weakness : Vulnerable to GPU attacks due to speed (less secure than bcrypt/yescrypt).

### `$md5$` — SunMD5

- Type : MD5-based hash.
- Use Case : Legacy Solaris systems.
- Weakness : MD5 is cryptographically broken; trivial to crack.

### `$1$` — md5crypt

- Type : MD5-based hash.
- Use Case : Older FreeBSD/OpenBSD systems.
- Weakness : Weak against modern cracking (avoid for new systems).

And much more.
</details>

### Time to CRACK 😱

Usually, when an attacker finds a set of hashed passwords, they aren't labeled with their hash type. The attacker must first identify the hash algorithm before attempting to crack them. So, I’ll start by identifying each hash as the first step.

<details><summary>Here is a word on easy crack</summary>
  
To crack hashes, Ican use online rainbow tables for weak or unsalted hashes like MD5 or SHA-1. However, rainbow tables are outdated and ineffective against salted hashes. For better results, use tools like Hashcat or John the Ripper with wordlists, rules, or mask attacks.
</details>

<details><summary>Here is the difference between JtR, Hashcat and a wordlist</summary>
  
  Hashcat and JtR are two indenpendant tools and wordlists (and rules) can by used by Hashcat and JtR. Since it can be a little confusing, here are more details:

- **John the Ripper** : A user-friendly, CPU-based tool that auto-detects hash types and excels at rules-based password guessing.
- **Hashcat** : A high-speed, GPU-accelerated tool for advanced password cracking using brute-force, dictionary, or hybrid attacks.
- **Wordlists** : Precompiled text files (like `rockyou.txt`) containing potential passwords used as input for dictionary or hybrid attacks.
</details>

<details><summary>Here is the structure of how Hashcat operates</summary>
  
hashcat -m <hash_type> -a <attack_mode> hashfile wordlist

-m <hash_type> sets the hash algorithm via a numeric identifier. For example, -m 1000 targets NTLM hashes. I can find the correct code for the hash type in Hashcat’s official documentation (man hashcat) or example guides.

-a <attack_mode> defines the attack strategy. For instance, -a 0 triggers a "straight" attack, which systematically tests each password in the wordlist sequentially.

hashfile is the file containing the hash I aim to crack.

wordlist is the dictionary file holding potential passwords to test against the hash.
</details>

## Id and Crack md5

First, identify the hash:
0421008445828ceb46f496700a5fa65e

- A 32-character hexadecimal hash is typically an MD5 hash.

- Otherwise, I can run the next command and I’ll have tips on what hash type it can be, then try them with the wordlist.

```!#/bin/bash
hashid md5_hash.txt
```

- Or, a no brainer, go to [hashes.com](https://hashes.com/en/decrypt/hash) and enter the hash.
  
- Now that I know the hash type, in the **Generic hashes** type in [hashcat website](https://hashcat.net/wiki/doku.php?id=example_hashes), look for the number in the **Hash-Mode** column.

For MD5 it is 0, so put 0 after the -m.

- Now crack the hash:

```!#/bin/bash
hashcat -m 0 -a 0 md5_hash.txt /usr/share/wordlists/rockyou.txt
```

<details><summary>Problem encountered</summary>
  
- Note that I might have the GZip version of rockyou.txt. If that is the case, add .gz a the end of the command or decompress with gunzip rockuyou.txt.gz
  
- If I got the message in the screenshot, I can open the hash with nano and delete the “-” at the end.
  
![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/token%20length%20md5.png)

</details>

- Once the command is properly run, I should see this output:
  
![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/md5%20poop.png)

- Passed is **`"poop"`**

## Id and Crack sha256


First identify the hash:
f0e2e750791171b0391b682ec35835bd6a5c3f7c8d1d0191451ec77b4d75f240

- The hash has **64 characters, t**his is a strong indicator of **SHA-256** , which always produces a 64-character hexadecimal string.
  
- Otherwise, I can run the next command and I can see SHA-256 in the output:

```!#/bin/bash
hashid sha256_hash.txt
```

or

```!#/bin/bash
hashid f0e2e750791171b0391b682ec35835bd6a5c3f7c8d1d0191451ec77b4d75f24
```
![imge](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/hashid%20sha256_hash.txt.png)
![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/hashid%20sha256%20f0.png)

- Or, I can still use the no brainer [hashes.com](https://hashes.com/en/decrypt/hash) and enter the hash.
  
- Now that I know the hash type, in the **Generic hashes** type in [hashcat website](https://hashcat.net/wiki/doku.php?id=example_hashes), look for the number in the **Hash-Mode** column.

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/1400.png)

- Since there is no salt of prefix, we will go with 1400 SHA2-256, so put 1400 after the -m.
  
- Now crack the hash:

```!#/bin/bash
hashcat -m 1400 -a 0 sha256_hash.txt /usr/share/wordlists/rockyou.txt
```


<details><summary>Problem encountered</summary>

- Note that I might have the GZip version of rockyou.txt. If that is the case, add .gz a the end of the command or decompress with gunzip rockuyou.txt.gz
  
- If I got the message “Token length exception” I can open the hash with nano and delete the “-” at the end.

</details>

- Once the command is properly run, I should see this output:

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/sha256%20spongebob.png)

- Password is **`”spongebob”`**

## Id and Crack sha512crypt

First identify the hash:

output:$6$6zxk0rvcxNNvM2.t$ZrvO.OT9d2hvQzIo92wQeRd/C7advhtqrjRLHWSdQFMeDagUqI7/9KPwO/ZCn6.QVHSP03/9co/tHq3dgYe34/

- Since [hashes.com](https://hashes.com/en/decrypt/hash) does not work with sha512crypt, as the site cannot crack salted, computationally expensive hashes like sha512crypt. I will use `hashid`.

- And also, the hash has $6$ as its prefix which is typical for sha512crypt.

- Otherwise, I can run the next command and I can confirm that it is SHA-512 Crypt:

  ```!#/bin/bash
  hashid sha512_hash.txt
  ```
![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/sha512crypthassh.txt.png)

- Now that I know the hash type, in the **Generic hashes** type in [hashcat website](https://hashcat.net/wiki/doku.php?id=example_hashes), I look for the number in the **Hash-Mode** column. For sha512crypt it is 1800, so put 1800 after the -m.

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/sha512crypt%201800.png)

- Now I crack the hash:

```!#/bin/bash
hashcat -m 1800 -a 0 sha512crypt_hash.txt /usr/share/wordlists/rockyou.txt
```

<details><summary>Problem encountered:</summary>
I might have the GZip version of rockyou.txt. If that is the case, I add `.gz` a the end of the command or decompress with `gunzip rockuyou.txt.gz`
</details>

- Once the command is properly run, I should see this output:

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/sha512crypt%20princess.png)

- Password is **`”princess”.`**


## Id and Crack bcrypt

First identify the hash:

$2b$12$GASflvqEVO67Be3JDaPHCOjPuCszRW4dOTBQIG9PGfpJlVIuhqR0K

- The hash has $2b$ as its prefix which is typical for bcrypt, knowing that  **`$2b$`**, **`$2y$`**, or **`$2a`**are bcrypt prefix. bcrypt does not work in [hashes.com](https://hashes.com/en/decrypt/hash) either, as the site cannot crack salted, computationally expensive hashes like bcrypt.
  
- Otherwise, I can run the next command and I can see bcrypt in the output:

```!#/bin/bash
hashid bcrypt_hash.txt
```
or

```!#/bin/bash
hashid $2b$12$GASflvqEVO67Be3JDaPHCOjPuCszRW4dOTBQIG9PGfpJlVIuhqR0K
```

- For me its not working as I can see. Probably due to a limitation of hashid with bcrypt.

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/hashid%20bcrypt.png)
![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/hashid%202b.png)

- But it does not matter because I know the type of hash with the from the prefix.
  
- Now that I know the hash type, in the **Generic hashes** I type in [hashcat website](https://hashcat.net/wiki/doku.php?id=example_hashes), look for the number in the **Hash-Mode** column. For bcrypt it is 3200, so I put 3200 after the -m.

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/3200.png)

- Now I crack the hash:

```#!/bin/bash
hashcat -m 3200 -a 0 bcrypt_hash.txt /usr/share/wordlists/rockyou.txt
```

<details><summary>Problem encountered</summary>

- Note that I might have the GZip version of rockyou.txt. If that is the case, add .gz a the end of the command or decompress with gunzip rockuyou.txt.gz
  
- Was not able to hashid the bcrypt_hash.txt. No need, it is obvious that it is a bcrypt since the recognizable prefix.

</details>

- Once the command is properly run, Ishould see this output:

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Hashcat%20cracking/images/princess.png)

- Password is **`iloveyou`**.

## 🔙 Back to Portfolio
[⬅️ Back to my Cybersecurity Portfolio](https://github.com/RobinBoucherSec/RobinBoucherSec)





