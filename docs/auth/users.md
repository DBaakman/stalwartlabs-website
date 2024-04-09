---
sidebar_position: 3
---

# Users and Groups

As stated previously, Stalwart Mail Server offers the possibility to use either an internal directory or connect to an external directory service.
When using an internal directory, all [account management](/docs/management/cli/directory/overview) tasks, such as creating new user accounts, updating passwords, and setting quotas, are performed directly within Stalwart Mail Server.
However, if an external directory is utilized, all user account management must be performed within that external system. Stalwart Mail Server will rely on this external directory for authentication and user information but will not have the ability to directly modify user details.

## Users

Each time a user logs in, the following information associated with the account is retrieved from the directory server:

- `name`: The username of the account.
- `type`: The account type, which can be either `individual` for user accounts or `admin` for administrator accounts.
- `description`: A description or full name for the user.
- `secret`: The password for the user account. 
- `email`: A list of email addresses associated with the user.
- `member-of`: A list of group names that the user is a member of.
- `quota`: Disk quota for the user, in bytes.

## Passwords

Passwords can be stored in the directory hashed or in plain text (not recommended). The following password hashing schemes are supported:

- **Argon2:** A password-hashing function that was selected as the winner of the Password Hashing Competition in 2015. Identified by the prefix `"$argon2"` in the hashed secret.
- **PBKDF2 (Password-Based Key Derivation Function 2):** A key derivation function that is part of RSA Laboratories' Public-Key Cryptography Standards (PKCS) series. Identified by the prefix `"$pbkdf2"` in the hashed secret.
- **Scrypt:** A password-based key derivation function created by Colin Percival, originally for the Tarsnap online backup service. Identified by the prefix `"$scrypt"` in the hashed secret.
- **bcrypt (Blowfish Crypt):** A password hashing method based on the Blowfish cipher, it uses a salt to protect against rainbow table attacks and a cost factor to increase the amount of work to check a password, thereby slowing down any brute-force attempts. Identified by the prefix `"$2"` in the hashed secret.
- **SHA-512 Crypt and SHA-256 Crypt:** Cryptographic hash functions, designed by the NSA, which produce a hash of 512 bits and 256 bits, respectively. Identified by the prefixes `"$6$"` and `"$5$"` in the hashed secret.
- **SHA-1 Crypt:** An older cryptographic hash function which produces a 160-bit hash value. Identified by the prefix `"$sha1"` in the hashed secret.
- **MD5-based hash:** An older and weaker hash function that produces a 128-bit hash value. Identified by the prefix `"$1"` in the hashed secret.
- **BSDi Crypt (Enhanced DES-based hash):** A password hashing method that is a slight modification of the original Unix crypt function. Identified by the prefix `"_"` in the hashed secret.
- **Base64-encoded SHA-1, Salted SHA-1, SHA-256, Salted SHA-256, SHA-512, Salted SHA-512, MD5:** These hashing algorithms are identified by a prefix enclosed in curly braces, such as `"{SHA}"` or `"{SSHA}"`. The salted variants use a random value (salt) to guard against pre-computed lookup table attacks (rainbow tables).
- **Unix Crypt:** Traditional Unix password hashing method. Identified by the prefix `"{CRYPT}"` or `"{crypt}"`.
- **Plain Text:** In some cases, passwords may be stored as plain text, although this is generally not recommended due to security concerns. Identified by the prefixes `"{PLAIN}"`, `"{plain}"`, `"{CLEAR}"`, or `"{clear}"`.

If the hashed secret does not match any of these known prefixes, it is treated as a plain text password and directly compared to the provided secret.

## Groups

Groups are used to manage access to resources and to simplify the process of granting permissions to multiple users at once. For example, you can create a group called `sales` and add all sales representatives to it. You can then grant access to a shared mailbox to the `sales` group instead of having to grant access to each individual user.

Please refer to the [directory configuration](/docs/auth/directory/overview) section for more information on how groups are represented in your directory server.

## Quotas

Disk quotas are used to limit the amount of disk space a user or group can use. This tool is particularly useful in shared systems where it helps prevent a single user from consuming all the storage resources. Quotas work by monitoring and restricting the amount of disk space used.  When a user reaches their quota, they can no longer receive new emails until they delete some of their existing messages or until their quota is increased.

In Stalwart Mail Server, quota settings are stored in the configured [directory](/docs/auth/directory/overview). This means the disk quotas can be centrally managed and adjusted as necessary by the system administrator. Disk quota limits can be set individually for each user or can be applied uniformly across a group of users, depending on the system's needs.

## Email aliases

An email alias is an alternative email address that is associated with a user's primary email address. In Stalwart Mail Server, email aliases are managed through the configured directory. This means that if you want to create, modify, or remove an alias, these operations should be performed directly in the directory. 

## Mailing lists

A mailing list is a collection of email addresses used to distribute content to multiple recipients simultaneously. It is an efficient way to send newsletters, updates, or other forms of mass communication to a large group of people. Just like aliases, mailing lists are managed through the configured directory. This means that if you want to create, modify, or remove a mailing list, these operations should be performed directly in the directory. 
