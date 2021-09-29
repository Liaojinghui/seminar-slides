---
title: "Runtime Random Number for Blockchain Smart Contract"
author: "Jinghui Liao, Zhanbo Wang"
institute: "Wayne State University, SusTech"
urlcolor: blue
colortheme: "beaver"
date: "Sep 28, 2021"
theme: "Heverlee"
aspectratio: 43
lang: en-US
marp: true
---

<!-- # Runtime Random Number for Blockchain Smart Contract

### Jinghui Liao
### Zhanbo Wang

### WSU & SusTech

### Sep 28, 2021
--- -->
# What we want to do?

- Anti-MEV attack
- Random number for smart contract at runtime.

---

# Why we want to do it?

- It is critical to provide random number for smart contract
    - Randomly select someone
    - Randomly distribute asset
    - Randomly determine the rarity

- It is hard to generate reliable random number at runtime
    - No one should know the random value in advance
    - No one should have control on the random value
    - There should be only one certain value
    - The random number should be verifiable

---

# Is there any existing solution?

- VRF (Chainlink)

- VDF (Eth2.0)

- BLS (Some other projects)

---

# What are the technique challenges?

- MEV attack
- BLS setup

---

# What is our solution?

- Anti-MEV 
    * Split the right of deciding transaction list from the miner
    * Genrate the transaction list during the consensus
    * Use BLS to generate the random number

---

# Where are we now? 

- We are the very first project to provide random number for smart contract at runtime
- We have designed the anti-mev attack solution
- We have verified the BLS algorithm
- We are writing the introduction part
---

# Implementation & Evaluation?
- Our design is based on the BFT consensus
- Will implement it on NEO
- Evaluation will based on (one and multiple random numbers)
    - The number of transactions required
    - The transaction fee required
    - The time it takes to acquire a random number