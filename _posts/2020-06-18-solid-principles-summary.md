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
- SOILD design comes from paper "[Design Principles and Design Patterns](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjHkI-g5YrqAhUczjgGHaBSChAQFjACegQIARAB&url=https%3A%2F%2Ffi.ort.edu.uy%2Finnovaportal%2Ffile%2F2032%2F1%2Fdesign_principles.pdf&usg=AOvVaw1i8O0yvzDSdHlwinUGJxSy)" by [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin)
- This mnemonic SOLID is attributed to [Michael Feathers](https://twitter.com/mfeathers)
- The principles are:
    - **S**ingle Resposibility Principle *SRP*
    - **O**pen Closed Principle
    - **L**iskov's Substitutabilty Principle
    - **I**nterface Segregation Principle
    - **D**ependency Inversion Principle
- **SOLID** design principles are meant to be used holistically to get the real value out of it.

# Single Responsibility Principle (SRP)
- **Definition:** *Every module/class should only have one responsibility and therefore only one reason to change.*
- **Relevant Zen:** *There should be one-- and preferably only one --obvious way to do things.*
- **Notes**:
    - It increases [cohesion](https://en.wikipedia.org/wiki/Cohesion_\(computer_science\)) and [loose coupling](https://en.wikipedia.org/wiki/Loose_coupling) by organizing code around responsibilities.
    - Think of reponsibilities as use cases.
    - Each use case should only be handled in one place therefore creating one obvious way to do things (following the Zen) hence "one reason to change" only if use case has changed (following SRP).
    - The code is sparse, and not dense, simple not complex, flat and not nested with SRP
