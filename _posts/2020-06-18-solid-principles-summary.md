---
toc: true
layout: post
comments: true
description: This is a summary of SOLID Principles explained in blog mentioned below.
categories: [software-engineering, python]
title: Summary of SOLID Principles
---

# What is SOLID Design
- SOILD design comes from paper [Design Principles and Design Patterns](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjHkI-g5YrqAhUczjgGHaBSChAQFjACegQIARAB&url=https%3A%2F%2Ffi.ort.edu.uy%2Finnovaportal%2Ffile%2F2032%2F1%2Fdesign_principles.pdf&usg=AOvVaw1i8O0yvzDSdHlwinUGJxSy) by [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin)
- This mnemonic SOLID is attributed to [Michael Feathers](https://twitter.com/mfeathers)
- **SOLID** principle are intended to foster simpler, more robust and updatable code from software developers.
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
    - From the example, the original class `FPTClient` has two resposibilities (handles two use cases), `FTP` and `SFTP` server, therefore two reason to change the class. Splitting the `FPTClient` class into 2 classes to handle each server separately fixes the issue.

## Open Closed Principle (OCP)
- **Definition:** *Software Entities (classes, functions, modules) should be open for extension but closed to change.*
- **Relevant Zen:** *Simple is better than complex. Complex is better than complicated.*
- **Notes**:
    - Think of **Change** and **Extension** in term of *function signatures*.
    - Forcing calling code to be updated is the **Change**, includes chaging function name, swapping the order of parameter or adding a non-default parameter.
    - Not forcing calling code to be updated while providing new fuctionality, includes renaming a parameter, adding a new parameter with a default value, or adding `args`, or `kwarg` (relates to [Effective python](https://effectivepython.com/), Item 23: *Provide Optional Behavior with Keyword Arguments*)
    - In terms of classes, changing methods signatures is the **Change** and adding new method is the **Extension**.
    - From the example, adding `upload_bulk` and `download_bulk` to the `FTPClient` extends the class, satisfying the **OCP**. This also satisfies **SRP** (only one reason to change the `FTPClient`, i.e., support for bulk download and upload). Although we can create `BulkFTPCLient` inheriting from `FTPClient` but for that we have to change the singnature for `download` and `upload` method as it would return list of bytes instead of just bytes therefore voilating the **OCP** as well as **LSP**.
    
## Liskov's Substituitability Principle (LSP)
- **Definition:** *If S is a subtype of T, then objects of type T may be replaced with objects of Type S.*
- **Relevant Zen:** *Special cases aren’t special enough to break the rules.*
- **Notes**:
    - This principle says that "Any child class can replace it's parent class without breaking the functionality".
    - From the example, `SFTPClient` object can replace the `FTPClient` object, meaning the calling code won't be aware of calling `upload` and `download` methods of `SFPTClient` or `FTPClient`. Take an another case of specialized  case of FTP file transfer is FTPS (different from SFTP), creating a new class `FTPSClient` that extends `FTPClient` is the way to go.

## Interface Segregation Principle (ISP)
- **Definition:** *A client should not depend on methods it does not use.*
- **Relevant Zen:** *Readability Counts && complicated is better than complex.*
- **Notes**:
    - It's about making reasonable choices for how other developers will interface with your code.
    - A good interface will have good abstraction making the code more readable (follows the Zen).
    - from the example, Since `S3` not a special case of `FTP`. We create a `FileTransferCLient` abstract class (it's closest thing to interface in python) with method `download` and `upload`, since that `S3` and `FTP` have in common, the file transfer protocol.  Similarly we can have `BulkFileTransferClient` abstract class. So, any functions can be generic by operating on `FileTransferClient` instead of coulping with specific client like `FPTClient` or `S3Client`.
    - Example code:
        ```python
        class FileTransferClient(ABC):
          def upload(self, file:bytes):
            pass

          def download(self, target:str) -> bytes:
            pass
        ```

## Dependency Inversion Principle (DIP)
- **Definition:** *High-level modules should not depend on low-level modules. They should depend on abstractions and abstractions should not depend on details, rather details should depend on abstractions.*
- **Relevant Zen:** *Explicit is Better than Implicit*
- **Notes**:
    - **DIP** ties all principles together. **SOLID** principles was all about getting to a place where we are no longer dependent on a detail.
    - Dependency should be on abstractions not concretization.
    - Our high-level modules no longer need to depend on a low-level module like `FTPCleint`, `SFTPClient`, or `S3Clent`, instead, they depend on abstraction `FileTransferClient`.
    - Our abstraction `FileTransferClient` is not dependent on protocol specific details and instead, those details depend on how they will be used through the abstraction (i.e. files can be uploaded or downloaded)
    - We can now write code around our business rules without tying them to a specific implementation.
    - example code:
        ```python
        def exchange(client:FileTransferClient, to_upload:bytes, to_download:str) -> bytes:
            client.upload(to_upload)
            return client.download(to_download)

        if __name__ == '__main__':
            ftp = FTPClient('ftp.host.com')
            sftp = FTPSClient('sftp.host.com', 22)
            ftps = SFTPClient('ftps.host.com', 990, 'ftps_user', 'P@ssw0rd1!')
            s3 = S3Client('ftp.host.com')
            scp = SCPClient('ftp.host.com')

            for client in [ftp, sftp, ftps, s3, scp]:
                exchange(client, b'Hello', 'greeting.txt')
        ```
        - High-level function `exchange` depends on abstraction `FileTransferClient` and all the clients implementation depends on it too.
# Refrences
- [Python SOLID Presentation](https://www.slideshare.net/DrTrucho/python-solid)
- [A Pythonic Guide to SOLID Design Principles ](https://dev.to/ezzy1337/a-pythonic-guide-to-solid-design-principles-4c8i)
- [S.O.L.I.D Principles explained in Python with examples.](https://medium.com/@dorela/s-o-l-i-d-principles-explained-in-python-with-examples-3332520b90ff)
- [SOLID Python: SOLID principles applied to a dynamic programming language](https://www.researchgate.net/publication/323935872_SOLID_Python_SOLID_principles_applied_to_a_dynamic_programming_language)
