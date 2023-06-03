---
author: Ayush Saha
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

The Vigenère cipher is a classic encryption technique that dates back to the 16th century. It was invented by a French diplomat and cryptographer named Blaise de Vigenère, hence the name. It was known as “le chiffre indéchiffrable,” which means “the indecipherable cipher,” and remained unbroken until British polymath Charles Babbage broke it in the 19th century.

## How it works

The Vigenère cipher is a polyalphabetic substitution cipher that uses a keyword to encrypt and decrypt messages. The key idea behind the Vigenère cipher is to use multiple [Caesar ciphers](https://crypticcamel.github.io/posts/01/caesar-cipher/) based on the letters of the keyword.

Let's say we have a plaintext message $P$ consisting of $n$ letters: $P = p_1, p_2, \ldots, p_n$​. We also have a keyword $K$ consisting of $m$ letters: $K = k_1, k_2, \ldots, k_m$​, where $m \leq n$.

To encrypt the message, we repeat the keyword until it matches the length of the plaintext. Let $K' = k_1, k_2, \ldots, k_m, k_1, k_2, \ldots, k_m, \ldots\$ repeated until it matches the length of $P$.

To encrypt each letter $p_i$​ in the plaintext, we find the corresponding letter $k_j$​ in the keyword $K'$ where $j$ is the index of $p_i$​ modulo $m$. Then, we shift the letter $p_i$​ by the index of $k_j$​ in the alphabet. Let $E$ represent the encrypted message.

The mathematical representation of the encryption process is given by:
$$E_i = (p_i + k_j) \mod 26$$
where $E_i$​ is the $i$-th letter of the encrypted message and $j = (i−1) \mod m$.

To decrypt the message, we use a similar process. We repeat the keyword until it matches the length of the ciphertext. Let $K'' = k_1, k_2, \ldots, k_m, k_1, k_2, \ldots, k_m, \ldots\$ (repeated until it matches the length of $E$).

To decrypt each letter $E_i$​ in the ciphertext, we find the corresponding letter $k_j$​ in the keyword $K'$ (where $j$ is the index of $E_i$​ modulo $m$). Then, we shift the letter $E_i​$ back by the index of $k_j$​ in the alphabet. Let $D$ represent the decrypted message.

The mathematical representation of the decryption process is given by:
$$D_i = (Ei−kj) \mod 26$$
where $D_i$​ is the $i$-th letter of the decrypted message and $j = (i−1) \mod m$.

Note that in these equations, we use modular arithmetic with $26$ since there are $26$ letters in the English alphabet. Additionally, we assume a simple mapping where $A$ is represented by $0$, $B$ by $1$, and so on.

### Example

Let's use the word "KEY" as the keyword and encrypt the plaintext message "HELLO" using the Vigenère cipher.

First, we repeat the keyword "KEY" to match the length of the plaintext "HELLO". Thus, the repeated keyword becomes "KEYKE".

The corresponding numerical representation of the plaintext message "HELLO" is: 
$$H \rightarrow 7, \quad E \rightarrow 4, \quad L \rightarrow 11, \quad L \rightarrow 11, \quad O \rightarrow 14$$

Using the mathematical encryption formula $E_i = (p_i+k_j) \mod 26$, we can calculate the encrypted values as follows:
$$E_1 = (7 + 10) \mod 26 = 17 \rightarrow R\\\
E_2 = (4 + 4) \mod 26 = 8 \rightarrow I\\\
E_3 = (11 + 24) \mod 26 = 9 \rightarrow J\\\
E_4 = (11 + 10) \mod 26 = 21 \rightarrow V\\\
E_5 = (14 + 4) \mod 26 = 18 \rightarrow S$$

Thus, the encrypted message is "RIJVS".

To decrypt the ciphertext "RIJVS" back to the original plaintext, we repeat the keyword "KEY" to match the length of the ciphertext. Thus, the repeated keyword becomes "KEYKE".

Using the mathematical decryption formula $D_i=(E_i−k_j) \mod 26$, we can calculate the decrypted values as follows:
$$D_1 = (17 - 10) \mod 26 = 7 \rightarrow H\\\
D_2 = (8 - 4) \mod 26 = 4 \rightarrow E\\\
D_3 = (9 - 24) \mod 26 = 11 \rightarrow L\\\
D_4 = (21 - 10) \mod 26 = 11 \rightarrow L\\\
D_5 = (18 - 4) \mod 26 = 14 \rightarrow O$$

Thus, the decrypted message is "HELLO".

### Vigenère Table
Encryption/decryption in case of Vigenere Cipher is a tedious task. The Vigenère table, also known as the Vigenère square or the Tabula Recta, is used to simplify the process of encryption and decryption. 

{{< figure
  src="/posts/02/vigenere-table.png"
  caption="Fig 1. The Vigenère Table"
  height="400"
  width="400"
  align="center"
>}}

It consists of a grid or matrix that provides a systematic way of encrypting and decrypting messages using the Vigenère cipher. The table is formed by aligning multiple Caesar ciphers together, each with a different shift value determined by a keyword. The rows and columns of the table represent the letters of the alphabet, and each cell contains the letter resulting from combining the row and column letters using the Caesar cipher.

## The Code

The encryption-decryption script as well as the hacking script can be downloaded from [here](https://github.com/crypticCamel/vigenere-cipher).


Here is the source code of *encryption_decryption.py*:

```python
def vigenere_cipher(text, keyword, mode):
    result = ""
    keyword_length = len(keyword)
    keyword = keyword.upper()
    key_index = 0

    for i, char in enumerate(text):
        if char.isalpha():
            ascii_offset = ord('A') if char.isupper() else ord('a')
            keyword_shift = ord(keyword[key_index % keyword_length]) - ord('A')
            
            if mode == "decrypt":
                keyword_shift = -keyword_shift  # Reverse the shift for decryption

            char = chr((ord(char) - ascii_offset + keyword_shift) % 26 + ascii_offset)
            key_index+=1
        result += char

    return result

# Get mode
while True:
    print('Do you want to (e)ncrypt or (d)ecrypt?')
    response = input('> ').lower()
    if response.startswith('e'):
        action = 'encrypt'
        break
    elif response.startswith('d'):
        action = 'decrypt'
        break
    print('Please enter the letter e or d.')

print("Enter the message.")
message = input('> ')

print("Enter the keyword.")
keyword = input('> ').upper()

# Perform encryption/decryption
result = vigenere_cipher(message, keyword, action)

# Display the result
print(f"Result: {result}")
```

The vigenere_cipher function performs the encryption or decryption operation based on the provided parameters. It initializes an empty string called result to store the final result. It calculates the length of the keyword and converts it to uppercase.

Next, it iterates through each character in the text input. If the character is a letter, it determines whether it is uppercase or lowercase and assigns an ASCII offset accordingly. It calculates the shift value for the current character based on the corresponding letter in the keyword. If the mode is set to "decrypt," it reverses the shift value.

Then, it applies the shift to the current character by subtracting the ASCII offset, adding the keyword shift, and taking the modulo 26 to wrap around the alphabet. Finally, it converts the shifted value back to a character using the ASCII offset and appends it to the result string. The key_index is incremented to ensure the keyword characters are used cyclically.

After processing all characters in the text, the result string contains the encrypted or decrypted message, depending on the mode.

The code prompts the user to enter the encryption or decryption mode (encrypt or decrypt) and stores the response in the action variable. It keeps asking for input until a valid mode is provided.

Then, it asks the user to enter the message and keyword, storing them in the message and keyword variables, respectively.

The vigenere_cipher function is called with the provided inputs (message, keyword, and action), and the result is stored in the result variable.

### The Program in Action

Here is a sample output that we get upon executing the program:

```console
Do you want to (e)ncrypt or (d)ecrypt?
> e
Enter the message.
> Enemy is approaching! Send troops immediately!
Enter the keyword.
> Moonlight
Result: Qbszj qy hibfcnnpouz! Esbq ezuvie wazplohmqzm!

Do you want to (e)ncrypt or (d)ecrypt?
> d
Enter the message.
> Qbszj qy hibfcnnpouz! Esbq ezuvie wazplohmqzm!
Enter the keyword.
> Moonlight
Result: Enemy is approaching! Send troops immediately!
```
Here the key used is *Moonlight*.

## Conclusion

There are several ways to hack the Vigenère Cipher, such as Kasiski examination, Friedman test, frequency analysis, dictionary attack, etc. However, these are advanced methods and require complex application of combinatorics and statistics. Discussing even one of those would require a separate post. For now we will focus on encryption and decryption. I will surely post a Vigenère hack tutorial in future.

**Note:** Vigenère Cipher, although complex, is still too easy to break using modern methods and should not be used for serious encryption purposes.