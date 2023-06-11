---
author: Ayush Saha
title: "Hacking the Caesar Cipher"
cover:
  image: "/programming/01/caesar.jpg"
date: 2023-05-12
summary: "Learn to encrypt or decrypt a Caesar Cipher or hack it"
tags:
    - python
    - programming
    - cryptography
---

## Introduction

Cryptography is the practice of secure communication in the presence of third parties. Throughout history, people have used various techniques to protect their messages, ranging from simple substitution ciphers to more advanced encryption methods. Caesar Cipher is a classical encryption method that will be discussed in this post. It is not used much nowadays as it is very easy to crack. However, it fulfils the purpose of teaching and casual encryption where security is not the main concern. 

## About Caesar Cipher

Caesar Cipher is one of the oldest and simplest encryption algorithms. It is named after Julius Caesar, who used it to communicate secretly with his generals. The idea behind Caesar Cipher is to replace each letter in the plaintext with a letter that is a fixed number of positions down the alphabet. For example, if the key is 3, then each letter in the plaintext will be replaced by the letter that is three positions down the alphabet. Thus, 'A' becomes 'D', 'B' becomes 'E', 'C' becomes 'F', and so on.

## How it works

Caesar Cipher works by shifting each letter in the plaintext by a fixed number of positions down the alphabet. The number of positions shifted is known as the key or the shift value. The formula for encrypting a letter using Caesar Cipher is:

```math
E(x) = (x + k) mod 26
```

where `x` is the numerical value of the letter (A=0, B=1, C=2, ..., Z=25), `k` is the key or the shift value, and `mod 26` means to take the remainder when divided by 26.

To decrypt a letter, the formula is:

```math
D(x) = (x - k) mod 26
```

where `x` is the numerical value of the encrypted letter, `k` is the key or the shift value, and `mod 26` means to take the remainder when divided by 26.

### Example

Suppose the plaintext is "HELLO" and the key is 3. Then each letter in the plaintext is shifted by 3 positions down the alphabet, according to the encryption formula:

```
H -> K
E -> H
L -> O 
L -> O
O -> R
```

So the ciphertext is "KHOOR". To decrypt the ciphertext, we simply shift each letter back by 3 positions, according to the decryption formula:

```
K -> H
H -> E
O -> L
O -> L
R -> O
```

So the plaintext is "HELLO" again.

### Encryption table and the Caesar wheel

To make it easier to perform encryption and decryption, we can create a table that shows the correspondence between the plaintext letters and the ciphertext letters, as well as the numerical values of the letters. Suppose the key is 23 then the encryption table will look like this.

{{< figure
  src="/posts/01/encryption-table.jpg"
  caption="Fig 1. Encryption Table"
  height="60"
  width="525"
  align="center"
>}}

Another tool to make it easier to perform encryption or decryption in Caesar cipher is the Caesar wheel. The Caesar cipher wheel is a tool used for encryption and decryption in cryptography. It consists of a circular disk with the letters of the alphabet written around its circumference in a specific order. The disk can be rotated to any position, allowing the user to shift the letters a certain number of places to the right or left. To encrypt a message, each letter of the plaintext is replaced with the letter that is a certain number of positions to the right on the wheel. To decrypt the message, the process is reversed by shifting the letters to the left.

{{< figure
  src="/posts/01/caesar-wheel.jpg"
  caption="Fig 2. The Caesar Wheel"
  height="300"
  width="300"
  align="center"
>}}


## The Code

I chose the python programming language because it is straightforward and simple to understand. The encryption-decryption script as well as the hacking script can be downloaded from [here](https://github.com/crypticCamel/caesar-cipher) if you do not want to write the code yourself or copy-paste the code. However, if you are a beginner to coding, I suggest you type the code yourself as that will give you a better understanding.

### Encryption and Decryption

Here is the source code for *encryption_decription.py*

```python
# This code will encrypt or decrypt a Caesar Cipher
# Core encryption/decryption function
def caesar_cipher(text, shift, mode):
    result = ""

    if mode == "decrypt":
        shift = -shift  # Reverse the shift for decryption

    for char in text:
        if char.isalpha():
            ascii_offset = ord('A') if char.isupper() else ord('a')
            char = chr((ord(char) - ascii_offset + shift) % 26 + ascii_offset)
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

# Get shift value
while True:
    maxKey = 26
    print('Please enter the key (0 to 25) to use.')
    response = input('> ').upper()
    if not response.isdecimal():
        continue

    if 0 <= int(response) < 26:
        key = int(response)
        break

# Perform encryption/decryption
result = caesar_cipher(message, key, action)

# Display the result
print(f"Result: {result}")
```

Let's understand how the script works.

The `caesar_cipher()` function takes in three parameters: `text` is the message to encrypt or decrypt, `shift` is the number of positions to shift each letter, and `mode` specifies whether to encrypt or decrypt.

The `if char.isalpha():` condition checks if the character is a letter. It ensures that only alphabetic characters are processed, ignoring any other characters such as spaces or punctuation.

If the character is a letter, the code determines the ASCII offset based on whether the letter is uppercase or lowercase. If the character is uppercase, the ASCII offset is set to the value of 'A' (65 in ASCII), and if the character is lowercase, the offset is set to the value of 'a' (97 in ASCII).

Next, the code performs the shift operation on the character. It subtracts the ASCII offset from the current character's ASCII value, then adds the shift value, and finally applies the modulo operator % 26 to ensure that the result stays within the range of the alphabet. This step effectively shifts the character by the specified number of positions.

After the character has been shifted, it is converted back to a character using the chr() function, and the resulting character is concatenated to the result variable.

This process is repeated for each character in the input text, resulting in the encryption or decryption of the entire message.

The program prompts the user to select whether to encrypt or decrypt, and then prompts for the message to encrypt/decrypt and the shift value. The `caesar_cipher()` function is called with the appropriate parameters, and the result is displayed.

#### The Program in Action

Here is an example of what the output would be upon executing *encryption_decryption.py*:

```console
Do you want to (e)ncrypt or (d)ecrypt?
> e
Enter the message.
> Enemy is approaching! Send troops immediately!
Please enter the key (0 to 25) to use.
> 11
Result: Pypxj td laaczlnstyr! Dpyo eczzad txxpotlepwj!

Do you want to (e)ncrypt or (d)ecrypt?
> d
Enter the message.
> Pypxj td laaczlnstyr! Dpyo eczzad txxpotlepwj!
Please enter the key (0 to 25) to use.
> 11
Result: Enemy is approaching! Send troops immediately!
```

That's it for *encryption_decryption.py*. Let's move on to hacking the Caesar Cipher.

### Hacking the Caesar Cipher

To hack the Caesar cipher we use a technique called the brute force technique also known as exhaustive search.

In cryptography, brute force attack is a method of trying all possible keys or passwords to decrypt encrypted data. It involves systematically attempting every combination until the correct one is found. This technique can be time-consuming and computationally expensive, especially with longer and more complex keys. To protect against brute force attacks, cryptographic systems use stronger and longer keys, making it practically infeasible to try all combinations within a reasonable timeframe.

However, brute force can be effective against the Caesar cipher because the Caesar cipher has a small key space and a limited number of possible keys. The Caesar cipher is a simple substitution cipher that shifts each letter of the plaintext by a fixed number of positions in the alphabet. Since there are only 26 possible shifts in the English alphabet, a brute force attack can easily try all 26 possibilities to decrypt the ciphertext. By systematically trying each shift, the correct plaintext can be discovered.

Unfortunately, brute force technique isn’t sophisticated enough to identify when it has found the correct key. It relies on a human to read the output and identify which decryption produced the original English message.

Here is the hack script named *decipher.py*:
```python
# This code will decipher a Caesar Cipher

# Get cipher text
print("Enter the Caesar Cipher text")
message = input("> ")

for shift in range(26):
    result = ''
    for char in message:
        if char.isalpha():
            ascii_offset = ord('A') if char.isupper() else ord('a')
            char = chr((ord(char) - ascii_offset - shift) % 26 + ascii_offset)
        result += char

    print(f"Key: {shift} | Decrypted message: {result}")
```

Note that code is nearly identical to the `caesar_cipher()` function of *encryption_decryption.py*. The hacking program implements the same decryption logic, except that it does so in a `for` loop, which runs the code for every possible key.

Here is a sample output of the above code:

```console
Enter the Caesar Cipher text
> Pypxj td laaczlnstyr! Dpyo eczzad txxpotlepwj!
Key: 0 | Decrypted message: Pypxj td laaczlnstyr! Dpyo eczzad txxpotlepwj!
Key: 1 | Decrypted message: Oxowi sc kzzbykmrsxq! Coxn dbyyzc swwonskdovi!
Key: 2 | Decrypted message: Nwnvh rb jyyaxjlqrwp! Bnwm caxxyb rvvnmrjcnuh!
Key: 3 | Decrypted message: Mvmug qa ixxzwikpqvo! Amvl bzwwxa quumlqibmtg!
Key: 4 | Decrypted message: Lultf pz hwwyvhjopun! Zluk ayvvwz pttlkphalsf!
Key: 5 | Decrypted message: Ktkse oy gvvxuginotm! Yktj zxuuvy osskjogzkre!
Key: 6 | Decrypted message: Jsjrd nx fuuwtfhmnsl! Xjsi ywttux nrrjinfyjqd!
Key: 7 | Decrypted message: Iriqc mw ettvseglmrk! Wirh xvsstw mqqihmexipc!
Key: 8 | Decrypted message: Hqhpb lv dssurdfklqj! Vhqg wurrsv lpphgldwhob!
Key: 9 | Decrypted message: Gpgoa ku crrtqcejkpi! Ugpf vtqqru koogfkcvgna!
Key: 10 | Decrypted message: Fofnz jt bqqspbdijoh! Tfoe usppqt jnnfejbufmz!
Key: 11 | Decrypted message: Enemy is approaching! Send troops immediately!
Key: 12 | Decrypted message: Dmdlx hr zooqnzbghmf! Rdmc sqnnor hlldchzsdkx!
Key: 13 | Decrypted message: Clckw gq ynnpmyafgle! Qclb rpmmnq gkkcbgyrcjw!
Key: 14 | Decrypted message: Bkbjv fp xmmolxzefkd! Pbka qollmp fjjbafxqbiv!
Key: 15 | Decrypted message: Ajaiu eo wllnkwydejc! Oajz pnkklo eiiazewpahu!
Key: 16 | Decrypted message: Zizht dn vkkmjvxcdib! Nziy omjjkn dhhzydvozgt!
Key: 17 | Decrypted message: Yhygs cm ujjliuwbcha! Myhx nliijm cggyxcunyfs!
Key: 18 | Decrypted message: Xgxfr bl tiikhtvabgz! Lxgw mkhhil bffxwbtmxer!
Key: 19 | Decrypted message: Wfweq ak shhjgsuzafy! Kwfv ljgghk aeewvaslwdq!
Key: 20 | Decrypted message: Vevdp zj rggifrtyzex! Jveu kiffgj zddvuzrkvcp!
Key: 21 | Decrypted message: Uduco yi qffheqsxydw! Iudt jheefi yccutyqjubo!
Key: 22 | Decrypted message: Tctbn xh peegdprwxcv! Htcs igddeh xbbtsxpitan!
Key: 23 | Decrypted message: Sbsam wg oddfcoqvwbu! Gsbr hfccdg waasrwohszm!
Key: 24 | Decrypted message: Rarzl vf nccebnpuvat! Fraq gebbcf vzzrqvngryl!
Key: 25 | Decrypted message: Qzqyk ue mbbdamotuzs! Eqzp fdaabe uyyqpumfqxk!
```

## Conclusion

Previously, I mentioned that Caesar Cipher is not suitable for serious encryption purposes. This post is intended for beginners who are interested in exploring the field of cryptography and want to enhance their programming skills. While there are numerous encryption methods available, Caesar Cipher is one of the most straightforward to understand. However, I will be posting more content in the future that discusses other types of ciphers in greater depth.
