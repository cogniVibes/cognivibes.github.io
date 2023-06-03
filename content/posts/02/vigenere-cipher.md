---
title: "Understanding the Vigenère Cipher"
cover:
  image: "/posts/02/vigenere.png"
date: 2023-06-02
summary: "Learn to encrypt or decrypt a Vigenère Cipher"
tags:
    - python
    - programming
    - cryptography
math: true
draft: true
---

## About Vigenère Cipher

The Vigenère cipher is a classic encryption technique that dates back to the 16th century. It was invented by a French diplomat and cryptographer named Blaise de Vigenère, hence the name. It was known as “le chiffre indéchiffrable,” which means “the indecipherable cipher,” and remained unbroken until British polymath Charles Babbage broke it in the 19th century.

## How it works

The Vigenère cipher is a polyalphabetic substitution cipher that uses a keyword to encrypt and decrypt messages. The key idea behind the Vigenère cipher is to use multiple Caesar ciphers based on the letters of the keyword.

Let's say we have a plaintext message $P$ consisting of $n$ letters: $P = p_1, p_2, \ldots, p_n$​. We also have a keyword $K$ consisting of $m$ letters: $K = k_1, k_2, \ldots, k_m$​, where $m \leq n$.

To encrypt the message, we repeat the keyword until it matches the length of the plaintext. Let $K' = k_1, k_2, \ldots, k_m, k_1, k_2, \ldots, k_m, \ldots\$ repeated until it matches the length of $P$.

To encrypt each letter $p_i$​ in the plaintext, we find the corresponding letter $k_j$​ in the keyword $K'$ where $j$ is the index of $p_i$​ modulo $m$. Then, we shift the letter $p_i$​ by the index of $k_j$​ in the alphabet. Let $E$ represent the encrypted message.

The mathematical representation of the encryption process is given by:
$$
E_i = (p_i + k_j) \mod 26
$$

where $E_i$​ is the $i$-th letter of the encrypted message and $j = (i−1) \mod m$.

To decrypt the message, we use a similar process. We repeat the keyword until it matches the length of the ciphertext. Let $K'' = k_1, k_2, \ldots, k_m, k_1, k_2, \ldots, k_m, \ldots\$ (repeated until it matches the length of $E$).

To decrypt each letter $E_i$​ in the ciphertext, we find the corresponding letter $k_j$​ in the keyword $K'$ (where $j$ is the index of $E_i$​ modulo $m$). Then, we shift the letter $E_i​$ back by the index of $k_j$​ in the alphabet. Let $D$ represent the decrypted message.

The mathematical representation of the decryption process is given by:
$$
D_i = (Ei−kj) \mod 26
$$

where $D_i$​ is the $i$-th letter of the decrypted message and $j = (i−1) \mod m$.

Note that in these equations, we use modular arithmetic with $26$ since there are $26$ letters in the English alphabet. Additionally, we assume a simple mapping where $A$ is represented by $0$, $B$ by $1$, and so on.
