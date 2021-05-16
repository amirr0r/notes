# [Cryptography](https://fr.wikipedia.org/wiki/Cryptographie)

## Terminology

- **Plaintext**: data with no encryption.

- **Ciphertext**: encrypted data.

- **Cryptology**: science of secrecy.

- **Cryptography**: study of techniques for secure communication, used to preserve **integrity** and **confidentiality** of sensitive information.

> _"A cryptosystem should be secure even if everything about the system, except the key, is public knowledge"_ [Kerckhoffs's principle](https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle)

- **Cryptanalysis**: set of methods tp attack cryptography by finding weaknesses in the underlying maths. Retrieving clear information without knowing the decryption key. 

- **Encoding**: (NOT a form of encryption) just a form of data representation such as **base64**. This is reversible.

- **Steganography**: hiding the existence of the message.

> Example of Histiee: shave the head of a slave, tattoo the message on his head, wait for the hair to grow back.

## Hashing

A **hash** is the output of a hash function. 

In principle, it's irreversible. 

A hash function takes an input _(strings, files, etc)_ and outputs a hash (fixed length string/hexadecimal number).

![md5 hash](img/md5.png)

> Bunch of different algorithms are used for hashing: `md5`, `sha1`, `sha256`, etc.

In opposite to encryption, there is no key, and it's meant to be impossible (or very very difficult) to go from the hash to the input.

**Any small change in the input data (even a single bit) should cause a large change in the output.**

> Tools like [hashID](https://pypi.org/project/hashID/) can be used to recognize which hashing function has been used.

> Different hash formats can be found here: <https://hashcat.net/wiki/doku.php?id=example_hashes>

### Uses of hash functions

There are 2 main reasons of using hash functions:

1. Verifying data **integrity**
2. Verifying passwords without storing them in plaintext

#### Integrity checking

Since for the same input you will get the same output/hash, hash functions can be used to check if a file has been modified.

> For instance, to ensure we've downloaded the file correctly.

It can also be used to find duplicate files.

> TODO: write about **Secure Boot**, **Signatures** and **HMAC**...

#### Password verification

Passwords are stored in databases as **hashes** in order to prevent the website admin to know your passwords and to protect them in case there is a database leak.

> The wordlist "`rockyou.txt`" comes from a databreach where passwords were stored in plaintext.

> Unix passwords are stored in `/etc/shadow` following this format: `$hashingalgorithmID$rounds$salt$hash`. Only root user can read this file.

> Windows passwords are hashed using NTLM and password hashes are stored in the **SAM** (Security Accounts Manager). Windows tries to prevent normal users from dumping them, but tools like `mimikatz` can help to do this. 

Password verification will be performed using **hash function**: if the output of the hash function (using the password you sent to log) is the same as the hash stored in the database, then it will consider that the password is correct.

> Here it comes the subject of **collisions**...

### Collisions

Since your password could be any string (so basically all the possibilities are an infinite set), and hashes have a fixed size (set with limited elements), there could be **collisions** &rarr; **two inputs having the same output/hash**.

> https://www.mscs.dal.ca/~selinger/md5collision/

> https://shattered.io/

### Rainbow tables

A **rainbow table** is basically  a collection of **precomputed hashes**.

A rainbow table is a pre-generated list of hash inputs to outputs, to quickly be able to look up an input (in this case, a password) from its hash.

It can be used by attackers to "crack" passwords.

> https://crackstation.net/

> https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/

To prevent this we can use a "**salt**", a random value (unique to each user) that we concatenate with user's password before using the hash function.

> Hash functions like bcrypt and sha512crypt handle this automatically.

> Salts don’t need to be kept private.

> Tools like `hashcat` or `john` can help to "crack" hashes.

___

## Useful links

- [cryptohack.org](https://cryptohack.org/challenges/)
- [dCode](https://www.dcode.fr/)
- [**John Hammond** - Cryptography (CTF WU)](https://www.youtube.com/watch?v=p__QZIxjHMk&list=PL1H1sBF1VAKU05UWhDDwl38CV4CIk7RLJ)
