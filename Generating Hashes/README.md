## Generating Hashes in Kali Linux

Here, I will simply prepare the passwords in different hash format.

## Description

In this project, I explored how to generate different types of hashes (MD5, SHA-256, SHA-512crypt and bcrypt) using terminal commands in Kali Linux. This is a foundational skill in understanding password storage and security mechanisms.

## Objective

My objective is to learn how to manually create hashes using various algorithms and understand how salting improves security in modern hashing techniques like bcrypt and SHA-512crypt.

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

I will create four types of hashes one by one and store them in a file.

- First I will create the file location for all the hashes:

``` bash
mkdir -p ~/projects/hashes/
```
<details>
  <summary>Commend explanation here</summary>
  
- mkdir is creating a new directory in ~/projects/hashes/
  
- -p is to ensures that all necessary parent directories (**`projects`** and **`hashes`**) are created if they don't already exist.
</details>

### md5

Note that I can use an online tools like [crackstation.net](https://crackstation.net/)  [hashes.com](https://hashes.com/en/decrypt/hash) or [tools4noobs.com](https://www.tools4noobs.com/) any other. This goes for all types of hashes.

- But we I  will use the terminal to create the hash value for “poop” in md5. Here is how to create a hash and the file with the hash in it:

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

- I can open the md5_hash.txt with cat command to see the hash value.

- Here I see what should happen in the terminal in kali linux:

  ![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Generating%20Hashes/images%20hashes%20generating/md5.png)

### sha256

sha256 hashes can be created with online tool as well but we will use the terminal to create the hash value for “spongebob” for sha256. Here is how to create a hash and the file with the hash in it:

```#!/bin/bash
echo -n "spongebob" | sha256sum > ~/projects/hashes/sha256_hash.txt
```

<details>
    <summary>Command explanation</summary>
    
- The `echo` command is used to display text or variables on the terminal or write them to files.

- The `-n` option suppresses the trailing newline, allowing subsequent output to appear on the same line.

- “spongebob” is the password that we are hashing.

- shs256sum is the type of hash

- ~/projects/hashes is the full path to directory

- sha256_hash.txt is the file where the hash is gonna be stored.

</details>


- I can open the sha256_hash.txt with cat command to see the hash value:

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Generating%20Hashes/images%20hashes%20generating/sha256.png)



  ### SHA512crypt

Here I cannot use online tools for generating the hash. SHA512crypt uses the SHA-512 hash with random salt and configurable rounds. It's secure but faster than bcrypt, making it less resistant to brute-force attacks. 

-  I will use the terminal to create the hash value for “princess” for sha512crypt. Here is how to create a hash and the file with the hash in it:

-  First verify that you have mkpasswd. It is part of the whois package, usually pre-installed on kali linux. If not, update/install it:

```!#/bin/bash
sudo apt update
sudo apt install whois
```

- Here is how to create a sha512crypt hash and the file with the hash in it:

```!#/bin/bash
mkpasswd --method=sha-512 princess > ~/projects/hashes/sha512crypt_hash.txt
```

<details><summary>Command explanation here</summary>

#### `mkpasswd`

- A utility for generating crypt-style hashed passwords.
  
- Part of the `whois` package in Kali Linux.

- Used here to create a secure hash using the SHA-512crypt algorithm.

This is not for changing real user passwords — it's often used for testing, scripting, or learning how password hashing works.

---

#### `-method=sha-512`

- Tells `mkpasswd` to use the SHA-512crypt algorithm.

- This is the same hashing method used by modern Linux systems (like in `/etc/shadow`).

Hashes generated this way start with: $6$…

- `$6$` is the identifier for SHA-512crypt.

---

#### `princess`

- The plaintext password you're going to hash.

- In this case, the word `"princess"` is being hashed.

- It will never be stored directly — only its hash will be saved.

---

#### `>`

- This is a shell redirection operator.
- It takes the output from the command before it and writes it into a file instead of printing it on screen.

---

#### `~/projects/hashes/sha512crypt_hash.txt`

- This is the file path where the hash result will be saved.

- Breakdown:

    - `~`: Your home directory (e.g.,`/home/kali`)

    - `/projects/hashes/`: A custom folder structure you might have made
   
    - `sha512crypt_hash.txt`: The file name

So if your username is `kali`, this resolves to: /home/kali/projects/hashes/sha512crypt_hash.txt

If the file doesn’t exist yet, it will be created automatically.

- Note that each time you run it, you’ll get a different hash because sha512crypt uses a random salt.

- You can open the sha512crypt_hash.txt with cat command to see the hash value.
  
</details>

- Note that each time I run it, I’ll get a different hash because sha512crypt uses a random salt.
  
- I can open the sha512crypt_hash.txt with cat command to see the hash value:

![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Generating%20Hashes/images%20hashes%20generating/sha512crypt.png)



### bcrypt

same thing here, you cannot use online tools for generating the hash. bcrypt uses a slower Blowfish cipher with random salt and a configurable work factor. It’s specifically designed for secure password storage and is more resistant to brute-force attacks. 

- I will use the terminal to create the hash value for “iloveyou” for bcrypt. Here it is gonna be different. I can install node.js and bcrypt OR I can install python3 and bcrypt.

- I will use python3. I Install it if I dont already have it:

```!#/bin/bash
  sudo apt install python3 python3-pip
pip3 install bcrypt
```

- Here is how to create a bcrypt hash and the file with the hash in it:

```!#/bin/bash
python3 -c 'import bcrypt; print(bcrypt.hashpw(b"iloveyou", bcrypt.gensalt()).decode())' > ~/projects/hashes/bcrypt_hash.txt
```

<details><summary>Command explanation here:</summary>
  
- python3

  - Tell the system to use the Python 3 interpreter to run the code.

- -c

  - Tells Python to run the code that follows in quotes , instead of loading a file. This lets you execute Python code directly from the command line .

- 'import bcrypt; print(bcrypt.hashpw(b"iloveyou", bcrypt.gensalt()).decode())'

- This is the actual Python code being executed. It's wrapped in single quotes so the shell knows to treat it as one argument for Python

</details>


<details><summary>Here for the details of the python code</summary>


#### `import bcrypt`

  - Loads the bcrypt module , which must already be installed (`pip install bcrypt`). Provides functions for secure password hashing.

#### `b"iloveyou"`

  - The `b` prefix means this is a byte string , required by `bcrypt.hashpw()`. bcrypt only works with bytes, not regular strings.

#### `bcrypt.gensalt()`

  - Generates a new random salt every time it's called. Salt ensures that even if the same password is hashed multiple times, the output will always be different.

#### `bcrypt.hashpw(...)`

  - This is the core function that actually hashes the password using bcrypt.
  
- Takes two arguments:
  
    - Password (as bytes)
 
    - Salt (generated above)

#### `.decode()`

  - Converts the byte string hash into a regular UTF-8 string.
  
- Needed because `hashpw()` returns a byte string like:
  
    b'$2b$12$abc123...’

- But we want to store it as:
    
    $2b$12$abc123…
    
- F. `print(...)` Outputs the final hash string so it can be redirected to a file.

#### > `~/projects/hashes/bcrypt_hash.txt` 

- Is not part of python, it’s handled by the Linux shell. > is the output redirection operator, it  takes whatever is printed by the Python script and writes it to the file location described.

</details>

- Note that each time I run it, I’ll get a different hash because bcrypt uses a random salt.
  
- I can open the bcrypt_hash.txt with cat command to see the hash value.

  ![image](https://github.com/RobinBoucherSec/Cracking-hashes/blob/main/Generating%20Hashes/images%20hashes%20generating/bcrypt.png)



  
