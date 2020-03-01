# Multipass

A multi-user password manager which aims to be more usable than the standard unix password manager pass.


## Why

Because of company policy no external password manager is allowed to be used and the IT was looking for a 
usable password manager. There current recommendation is to use Keepass which has the drawback that you only 
can share passwords in a limited way. Of cause there are other open source & commercial, self-hosted, local & 
web-based solutions, but they all have drawbacks one way or another (paid/only local/single-user/administer server/etc.).
What we have is our own git server where everybody has access to and can create their own repository - so
why not use it to create our own multi-user-password-manager. Our users are not only developers but also
Product Owners, Scrum Master, Management, Human Resources, Accounting, etc. so it has to be user friendly
and needs to work on Win/Mac/Linux.


## Guidelines

- All public keys of the users are in the repository (no central KeyServer available)
- All members of a group can add new members (a member knowing the secret can give it away anyways)
- All users can create new groups
- All groups & identities need to be obfuscated (to hide which systems we have)


## Specification

Three diffent types of files are used.

1. pgp public keys
2. pgp encrypted files which contain only one genrated password
3. with password encrypted json files which represent either a folder or an identity 


### Folder structure

```
store
|
+-- keys
|   |
|   +-- 440415BAE0EAF44D94E55C9939A1D2BE5DD50F76.asc                        (Alex PubKey)
|   \-- 34AF8F807658249736B97639E42F49CC3ADE160A.asc                        (Moni PubKey)
|
+-- vault
    |
    +-- _
    |   |
    |   +-- meta-parent                                                     (empty file for convention)
    |   +-- meta                                                            (with master password encrypted metadata)
    |   +-- 440415BA.gpg                                                    (master password enc with Alex PubKey - name first 8 byte)
    |   \-- 34AF8F80.gpg                                                    (master password enc with Moni PubKey - name first 8 byte)
    |
    +-- B04538FA                                                            (with master password enc identity file - name random)
    |
    +-- DA39                                                                (unique random number)
    |   |
    |   +-- _                                                               
    |   |   |
    |   |   +-- meta-parent                                                 (with master password encrypted metadata)
    |   |   +-- meta                                                        (with section DA39 password encrypted metadata)
    |   |   \-- 440415BA.gpg                                                (section password enc with Alex PubKey)
    |   |
    |   +-- A25D47KL                                                        (with section DA39 password enc identity file - name random)
    |   \-- F3BC081A                                                        (with section DA39 password enc identity file - name random)
    |
    +-- 0FA3
        |
        +-- _
        |   |
        |   +-- meta-parent                                                 (empty file to hide folder name and other metadata)
        |   +-- meta                                                        (with section 0FA3 password encrypted metadata)
        |   \-- 34AF8F80.gpg                                                (section password enc with Moni PubKey)
        |
        \-- FH3A00LK                                                        (with section 0FA3 password enc identity file - name random)
```


### Meta & Meta-Parent

meta and meta-parent can contain the same information, but it is not mandatory

```
{
    "name" : "foldername",
    "description" : "purpose of the folder",
    "modified" : "9223E6E0"
}
```


### Identyfile

The identity file contains 

```
{
    "name" : "name of identity",
    "description" : "description of identity",
    "modified" : "9223E6E0",
    "username" : "",
    "password" : "",
    "notes" : "",
    "fields" : [
        { "key" : "", "value" : "", "type" : "text", "password" : "yes" },
        { "key" : "", "value" : "", "type" : "bool", "password" : "no" }
    ]
}
```
