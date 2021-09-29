---
title: "Runtime Random Number for Blockchain Smart Contract"
author: "Jinghui Liao"
institute: "Wayne State University, SusTech"
urlcolor: blue
colortheme: "beaver"
date: "Sep 28, 2021"
theme: "Copenhagen"
aspectratio: 43
lang: en-US
marp: true
---

# Problems before everything

Should a middleware touch the user account?

# Secrecy Escrow

## What is it?

- A Safe for the Blockchain
- Like a bank, where user can keep their secrect document (will for example)

![](images/Safe.jpeg)

---

## Why we need it?

- Everything on Blockchain is public.

    - Transaction
    - Amount of assets
    - Contract call parameters

- We need to keep something in secrect (for a certain period of time).
        
    - Answer of a question
    - Last words
    - Moves of a game player
    - Time capsule
    - Secret documents

---

## Why Automata?

- It is hard to keep data secrect in a public system
- We need a decryption key management mechanism
- The decryption key should not be controlled by or known to anyone
- Automata has TEE to provide isolated environment

---

# Account Escrow

## What is it?

- User empower Automata to control their account by giving or creating private key in Automata
- Making Automata more like a centrolized service provider
- Or deposit token to Automata owned account

---

## How we gonna use it?

- Allowing automata to insert special values into the Transaction
    - Random number
    - Decrypted secrecy 
- Allowing Automata to trigger smart contract when special even occurs
    - Delayed contract state update
    - Recursive contract state update
    - Contract state update on special event (date, block height).

---

## Why Automata?

- Automata is a middleware, it monitor the Blockchain state all the time
- TEE ensures that Automata will not abuse the user token