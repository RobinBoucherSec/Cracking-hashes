## Generating Hashes in Kali Linux

Here, we will simply prepare the passwords in different hash format.

## Description

In this project, I explored how to generate different types of hashes (MD5, SHA-256, bcrypt, SHA-512crypt) using terminal commands in Kali Linux. This is a foundational skill in understanding password storage and security mechanisms.

## Objective

To learn how to manually create hashes using various algorithms and understand how salting improves security in modern hashing techniques like bcrypt and SHA-512crypt.

## Tools used

- Kali Linux – For terminal operations and hash generation
- MD5sum – To generate MD5 hashes
- SHA-256sum – To generate SHA-256 hashes
- Python 3 + bcrypt library – For generating salted bcrypt hashes
- mkpasswd (from the whois package) – For generating SHA-512crypt hashes
- rockyou.txt wordlist – For future cracking attempts using tools like Hashcat or John the Ripper
- Online tools – Such as crackstation.net , hashes.com , or tools4noobs.com for hash verification and identification


## Steps

### Initial setup

We will create four types of hashes one by one and store them in a file.

- First we will create the file location for all the hashes:

``` bash
mkdir -p ~/projects/hashes/
```
<details>
  <summary>Commend explanation here</summary>
  
- mkdir is creating a new directory in ~/projects/hashes/
  
- -p is to ensures that all necessary parent directories (**`projects`** and **`hashes`**) are created if they don't already exist.
</details>

### md5

Note that you can use an online tools like [crackstation.net](https://crackstation.net/)  [hashes.com](https://hashes.com/en/decrypt/hash) or [tools4noobs.com](https://www.tools4noobs.com/) any other. This goes for all types of hashes.

- But we will use the terminal to create the hash value for “poop” in md5. Here is how to create a hash and the file with the hash in it:

```#!/bin/bash
echo -n "poop" | md5sum > ~/projects/hashes/md5_hash.txt
```

<details>
  <summary>Command explanation</summary>
  
- The `echo` command is used to display text or variables on the terminal or write them to files.
  
- The `-n` option suppresses the trailing newline, allowing subsequent output to appear on the same line.
  
- “poop” is the password that we are hashing.
  
- md5sum is the type of hash
  
- ~/projects/hashes is the full path to directory
  
- md5_hash.txt is the file where the hash is gonna be stored.
  
</details>

- You can open the md5_hash.txt with cat command to see the hash value.

- Here you see what should happen in the terminal in kali linux:

  ![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Generating%20Hashes/md5.png)

  ### sha256

  Here we will use the terminal to create the hash value for “spongebob” for sha256. Here is how to create a hash and the file with the hash in it:

  ```#!/bin/bash
  echo -n "spongebob" | sha256sum > ~/projects/hashes/sha256_hash.txt
  ```

  <details>
    <summary>Command explanation</summary>
    
- The **`echo`** command is used to display text or variables on the terminal or write them to files.

- The **`-n`** option suppresses the trailing newline, allowing subsequent output to appear on the same line.

- “spongebob” is the password that we are hashing.

- shs256sum is the type of hash

- ~/projects/hashes is the full path to directory

- sha256_hash.txt is the file where the hash is gonna be stored.

</details>

- You can open the sha256_hash.txt with cat command to see the hash value:
[!image]()
