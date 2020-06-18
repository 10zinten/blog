---
toc: true
layout: post
comments: true
description: This is a summary of SOLID Principles explained in blog mentioned below.
categories: [software-engineering, python]
title: Summary of SOLID Principles
---
Here is the original [blog](https://dev.to/ezzy1337/a-pythonic-guide-to-solid-design-principles-4c8i)

# What is SOLID Design
- SOILD design comes from paper [Design Principles and Design Patterns](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjHkI-g5YrqAhUczjgGHaBSChAQFjACegQIARAB&url=https%3A%2F%2Ffi.ort.edu.uy%2Finnovaportal%2Ffile%2F2032%2F1%2Fdesign_principles.pdf&usg=AOvVaw1i8O0yvzDSdHlwinUGJxSy) by [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin)
- This mnemonic SOLID is attributed to [Michael Feathers](https://twitter.com/mfeathers)
- The principles are:
    - **S**ingle Resposibility Principle *SRP*
    - **O**pen Closed Principle
    - **L**iskov's Substitutabilty Principle
    - **I**nterface Segregation Principle
    - **D**ependency Inversion Principle
- **SOLID** design principles are meant to be used holistically to get the real value out of it.

## Single Responsibility Principle (SRP)
- **Definition:** *Every module/class should only have one responsibility and therefore only one reason to change.*
- **Relevant Zen:** *There should be one-- and preferably only one --obvious way to do things.*
- **Notes**:
    - It increases [cohesion](https://en.wikipedia.org/wiki/Cohesion_\(computer_science\)) and [loose coupling](https://en.wikipedia.org/wiki/Loose_coupling) by organizing code around responsibilities.
    - Think of reponsibilities as use cases.
    - Each use case should only be handled in one place therefore creating one obvious way to do things (following the Zen) hence "one reason to change" only if use case has changed (following SRP).
    - The code is sparse, and not dense, simple not complex, flat and not nested with SRP
    - From the example, the original class `FPTClient` has two resposibilities (handles two use cases), `FTP` and `SFTP` server, therefore two reason to change the class. Splitting the `FPTClient` class into 2 classes to handle echa server separately fixes issue.

## Open Closed Principle (OCP)
- **Definition:** *Software Entities (classes, functions, modules) should be open for extension but closed to change.*
- **Relevant Zen:** *Simple is better than complex. Complex is better than complicated.*
- **Notes**:
    - Think of **Change** and **Extension** in term of *function signatures*.
    - Forcing calling code to be updated is the **Change**, includes chaging function name, swapping the order of parameter or adding a non-default parameter.
    - Not forcing calling code to be updated while providing new fuctionality, includes renaming a parameter, adding a new parameter with a default value, or adding `args`, or `kwarg` (relates to Effective python, Item 23: *Provide Optional Behavior with Keyword Arguments*)
    - In terms of classes, changing methods signatures is the **Change** and adding new method is the **Extension**.
    - From the example, adding `upload_bulk` and `download_bulk` to the `FTPClient` extends the class, satisfying the **OCP**. This also satisfies **SRP** (only one reason to change the `FTPClient`, i.e., support for bulk download and upload). Although we can create `BulkFTPCLient` inheriting from `FTPClient` but for that we have to change the singnature for `download` and `upload` method as it would return list of bytes instead of just bytes therefore voilating the **OCP** as well as **LSP**.
    
## Liskov's Substituitability Principle (LSP)- 
- **Definition:** *If S is a subtype of T, then objects of type T may be replaced with objects of Type S.*
- **Relevant Zen:** *Special cases aren’t special enough to break the rules.*
- **Notes**:
    - This principle says that "Any child class can replace it's parent class withour breaking functionality".
    - From the example, `SFTPClient` object can replace the `FTPClient` object, meaning the calling code won't be aware of calling `upload` and `download` methods of `SFPTClient` or `FTPClient`. Take an another case of specialized  case of FTP file transfer is FTPS (different from SFTP). 