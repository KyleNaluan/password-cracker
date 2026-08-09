# Password Cracker

## Overview

This is an educational Java project that demonstrates offline password-cracking techniques against SHA-1 password hashes.
It was built as a security/coursework learning exercise, not as a production security tool.
Do not use it against systems or accounts you do not own or have explicit authorization to test.

## Why it exists

Password cracking is a classic way to learn about hashing, dictionary attacks, brute-force search space design, and basic concurrency in Java.
This project implements several common cracking strategies side by side so the trade-offs between search space size and speed are visible in practice.

## How to run

Requirements:
- Java (JDK 8 or later)
- Apache Commons Codec (`commons-codec`), for SHA-1 hashing via `DigestUtils`

Steps:
1. Download the `commons-codec` JAR (e.g. from Maven Central) and place it on your classpath.
2. Compile the source:
   ```
   javac -cp commons-codec-<version>.jar -d bin PasswordCracker/src/passwordCracker/PasswordCracker.java
   ```
3. Run it from the `PasswordCracker` directory (so it can find `passwords.txt` and `dictionary.txt`):
   ```
   java -cp bin:commons-codec-<version>.jar passwordCracker.PasswordCracker
   ```
4. Cracked results are printed to the console and appended to `cracked_passwords.txt`.

Input files:
- `passwords.txt` - one `userId <SHA-1 hash>` pair per line, the hashes to crack.
- `dictionary.txt` - a plain word list, one word per line, used for dictionary-based guesses.

## What it demonstrates technically

- **Hashing**: matching candidate plaintexts against SHA-1 hashes using Apache Commons Codec.
- **Dictionary attacks**: single-word, two-word, and three-word concatenation guesses drawn from a ~5,500-word list.
- **Brute-force attacks**: exhaustive generation of all-numeric passwords up to 9 digits.
- **Hybrid/mask attacks**: word-plus-digits and digits-plus-word combinations (e.g. `password123`, `123password`), including two-word-plus-digits variants.
- **Concurrency**: each cracking strategy runs as its own task on a fixed `ExecutorService` thread pool, sharing a `ConcurrentHashMap` of target hashes for thread-safe lookups.
- **File I/O**: streaming reads of the password list and dictionary, and incremental writes of cracked results to disk as they're found.
