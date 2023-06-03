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
---

## About Vigenère Cipher

The Vigenère cipher is a classic encryption technique that dates back to the 16th century. It was invented by a French diplomat and cryptographer named Blaise de Vigenère, hence the name. It was known as “le chiffre indéchiffrable,” which means “the indecipherable cipher,” and remained unbroken until British polymath Charles Babbage broke it in the 19th century.The idea behind Caesar Cipher is to replace each letter in the plaintext with a letter that is a fixed number of positions down the alphabet. For example, if the key is 3, then each letter in the plaintext will be replaced by the letter that is three positions down the alphabet. Thus, 'A' becomes 'D', 'B' becomes 'E', 'C' becomes 'F', and so on.

## How it works

The Vigenère cipher is a polyalphabetic substitution cipher that uses a keyword to encrypt and decrypt messages. The key idea behind the Vigenère cipher is to use multiple Caesar ciphers based on the letters of the keyword.

Let's say we have a plaintext message \\(P\\) consisting of nn letters: \\(P = p_1, p_2, \ldots, p_n\\)​. We also have a keyword \\(K\\) consisting of \\(\mathcal{m}\\) letters: \\(K = k_1, k_2, \ldots, k_m\\)​, where \(m \leq n\).

To encrypt the message, we repeat the keyword until it matches the length of the plaintext. Let \\(K' = k_1, k_2, \ldots, k_m, k_1, k_2, \ldots, k_m, \ldots\\) repeated until it matches the length of \\(P\\).

To encrypt each letter \\(p_i\\)​ in the plaintext, we find the corresponding letter \\(k_j\\)​ in the keyword \\(K′\\) where \\(j\\) is the index of \\(p_i\\)​ modulo \\(m\\). Then, we shift the letter \\(p_i\\)​ by the index of \\(k_j)​ in the alphabet. Let \\(E\\) represent the encrypted message.

The mathematical representation of the encryption process is given by:
$$
\(E_i = (p_i + k_j) \mod 26\)
$$

