---
title: "Hacking the Caesar Cipher"
cover:
  image: "/posts/01/caesar.jpg"
date: 2023-04-08
summary: "Learn to encrypt or decrypt a Caesar Cipher or hack it"
tags:
    - python
    - programming
    - tutorial
---

**Note:** Before we dive in, it's important to note that this guide is intended for beginners who want to improve their programming skills through practical projects. It's assumed that you have a basic understanding of Python, including various data types, working with variables, if-else conditionals, `for` and `while` loops, manipulating lists and dictionaries, and familiarity with built-in Python functions.

## Introduction

Cryptography is the practice of secure communication in the presence of third parties. Throughout history, people have used various techniques to protect their messages, ranging from simple substitution ciphers to more advanced encryption methods. Caesar Cipher is a classical encryption method that will be discussed in this post. It is not used much nowadays as it is very easy to crack. However, it fulfils the purpose of teaching and and casual encryption where security is not the main concern. 

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

### Encryption table

To make it easier to perform encryption and decryption, we can create a table that shows the correspondence between the plaintext letters and the ciphertext letters, as well as the numerical values of the letters. Suppose the key is 3 then the encrption table will look like this.

| Letter | A | B | C | D | E | F | *--snip--* | U | V | W | X | Y | Z |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **Encrypted Letter** | D | E | F | G | H | I | *--snip*-- | X | Y | Z | A | B | C |


## The Code

I chose the python programming language because it is straightforward and simple to understand. The encryption-decryption script as well as the hacking script can be downloaded from [here](https://github.com/crypticCamel/Caesar-Cipher) if you do not want to write the code yourself or copy-paste the code. However, if you are a beginner to coding, I suggest you type the code yourself as that will give you a better understanding.

### Encryption and Decryption

Here is the source code for *encryption_decription.py*

```python
## This code will encrypt or decrypt a Caesar Cipher
def caesar_encrypt(key):
    """
    Encrypts the user's input message using the Caesar Cipher algorithm
    and prints the encrypted text.
    """
    print("Enter message to encrypt")
    message = input("> ")
    encrypted_text = ''

    for character in message:
        if character.isalpha():
            ## If the character is an alphabetic character, determine
            ## the offset based on whether it is uppercase or lowercase.
            if character.isupper():
                offset = 65
            else:
                offset = 97

            ## Shift the character by the key in the opposite direction
            ## and wrap around if necessary.
            shift = (ord(character) - offset + key) % 26 + offset
            encrypted_text += chr(shift)
        else:
            encrypted_text += character

    print("Encrypted Text = " + encrypted_text)

def caesar_decrypt(key):
    """
    Decrypts the user's input message using the Caesar Cipher algorithm
    and prints the decrypted text.
    """
    print("Enter message to decrypt")
    message = input("> ")
    decrypted_text = ''
    
    for character in message:
        if character.isalpha():
            ## If the character is an alphabetic character, determine
            ## the offset based on whether it is uppercase or lowercase.
            if character.isupper():
                offset = 65
            else:
                offset = 97

            ## Shift the character by the key and wrap around if necessary.
            shift = (ord(character) - offset - key) % 26 + offset
            decrypted_text += chr(shift)
        else:
            decrypted_text += character

    print("Decrypted Text = " + decrypted_text)

def main():
	while True:
	    print("Would you like to (e)ncrypt or (d)ecrypt")
	    response = input("> ").lower()
	    if response.startswith('e'):
	        mode = 'encrypt'
	        break
	    elif response.startswith('d'):
	        mode = 'decrypt'
	        break
	    print("Please enter 'e' or 'd'")
	
	while True:
	    print("Please enter the key (0 to 25) to use.")
	    response = input("> ")
	    if not response.isdecimal():
	        continue
	    if 0 <= int(response) <= 25:
	        key = int(response)
	        break
	
	if mode == 'encrypt':
	    caesar_encrypt(key)
	elif mode == 'decrypt':
	    caesar_decrypt(key)

if __name__ == '__main__':
	main()
```

Let's understand how the script works.

#### Encryption

The `caesar_encrypt()` function looks like this:
```python
def caesar_encrypt(key):
    """
    Encrypts the user's input message using the Caesar Cipher algorithm
    and prints the encrypted text.
    """
    print("Enter message to encrypt")
    message = input("> ")
    encrypted_text = ''

    for character in message:
        if character.isalpha():
            ## If the character is an alphabetic character, determine
            ## the offset based on whether it is uppercase or lowercase.
            if character.isupper():
                offset = 65
            else:
                offset = 97

            ## Shift the character by the key in the opposite direction
            ## and wrap around if necessary.
            shift = (ord(character) - offset + key) % 26 + offset
            encrypted_text += chr(shift)
        else:
            encrypted_text += character

    print("Encrypted Text = " + encrypted_text)
```

First we define the function `caesar_encrypt()` which takes the `key` as an argument. We take the message as user input and initialize an empty string `encrypted_text`. We run a `for` loop through the message string and extract the characters of the string. Inside the loop we will work with every `character`.

First we check if the `character` is an alphabet with `if character.isalpha()`.
The `isalpha()` function is a built-in python function which checks if a character is an alphabet or not and returns a boolean value accordingly.

We then check if the alphabet is in uppercase or lowercase. If it is in uppercase, we set an offset of 65 otherwise we set an offset of 97. This step is necessary if want to preserve the case of the message. We could have skipped this step by converting the message to lowercase or uppercase entirely but then we wouldn't be able to preserve the case.
The numbers 65 and 97 are the starting ASCII value of uppercase and lowercase alphabets respectively.

The we move on to `shift = (ord(character) - offset + key) % 26 + offset`.
This line of code performs the core operation of the Caesar Cipher encryption algorithm, which is to shift the positions of characters in a message by a fixed number of positions to create an encrypted message. 

Let's understand what is going on here.

First, `ord(character)` returns the Unicode code point of the current `character`. Subtracting the ASCII value of the offset (either 65 for uppercase letters or 97 for lowercase letters) from the Unicode code point gives the position of the current `character` in the range of 0 to 25.

Then, the key value is added to this position to shift the `character` by `key` positions.

If the resulting shifted position is greater than or equal to 26, the modulo operator `%` wraps the position back around to the start of the alphabet (i.e., position 0), which is equivalent to subtracting 26 from the shifted position until it is less than 26.

Finally, the `offset` value is added back to the shifted position to get the new Unicode code point of the encrypted character which is converted back to its corresponding character using `chr(shift)` and appended (concatenated) to the `encrypted_text` string.

All of this only happens if the `character` is an alphabet. However, the `if character.isalpha()` statement might end up evaluating to `false` because the character can be a space or a punctuation mark or any other symbol. In that case we simply append the `character` as it is. We then move out of the `for` loop and print the `encrypted_text`.

We are done with encryption. Let's move on to decryption.

#### Decryption

The `caesar_decrypt` function looks like this:
```python
def caesar_decrypt(key):
    """
    Decrypts the user's input message using the Caesar Cipher algorithm
    and prints the decrypted text.
    """
    print("Enter message to decrypt")
    message = input("> ")
    decrypted_text = ''
    
    for character in message:
        if character.isalpha():
            ## If the character is an alphabetic character, determine
            ## the offset based on whether it is uppercase or lowercase.
            if character.isupper():
                offset = 65
            else:
                offset = 97

            ## Shift the character by the key and wrap around if necessary.
            shift = (ord(character) - offset - key) % 26 + offset
            decrypted_text += chr(shift)
        else:
            decrypted_text += character

    print("Decrypted Text = " + decrypted_text)
```

Upon closer inspection we see that the `caesar_decrypt()` function is more or less same as the `caesar_encrypt()` function. All operations performed by the `caesar_decrypt()` function are same as that of the `caesar_encrypt()` function. The only notable difference is that while calculating the `shift` we subtract the `key` instead of adding it. This is because decryption is the opposite of encryption.

For example if our character is 'C' and the key is 5 then encrypting it would give us 'H'.

```math
C + 5 -> H
```

On decrypting 'H' with the key 5, we get back 'C'.

```math
H - 5 -> C
```

**Note:** One thing we must note is that in this case we are not actually hacking the cipher. In fact we are simply decrypting it with a known key. We will come to hacking the cipher a bit later.

#### The `main()` function

The `main()` function is where the program will start executing. The `main()` function looks like this:
```python
def main():
	while True:
	    print("Would you like to (e)ncrypt or (d)ecrypt")
	    response = input("> ").lower()
	    if response.startswith('e'):
	        mode = 'encrypt'
	        break
	    elif response.startswith('d'):
	        mode = 'decrypt'
	        break
	    print("Please enter 'e' or 'd'")
	
	while True:
	    print("Please enter the key (0 to 25) to use.")
	    response = input("> ")
	    if not response.isdecimal():
	        continue
	    if 0 <= int(response) <= 25:
	        key = int(response)
	        break
	
	if mode == 'encrypt':
	    caesar_encrypt(key)
	elif mode == 'decrypt':
	    caesar_decrypt(key)
```

We run a `while` loop which will continue to ask the user whether they wish to encrypt or decrypt a message until they give a valid `response`. We ask the user to enter either 'e' or 'd' based on which operation they wish to perform and set a `mode` based on the `response`.

We run another `while` loop which will keep asking for the `key` until the user enters enters a valid key. Within the loop we check if the `key` entered by the user is decimal or not and if it is within 0 and 25 or not. The `key` is valid only if it is a decimal value between 0 and 25 inclusive.

Lastly, we call either the `caesar_encrypt()` function or the `caesar_decrypt()` function based on the mode with `key` as the argument.

#### The Program in Action

Here is an example of what the output would be upon executing *encryption_decryption.py*:

```
Would you like to (e)ncrypt or (d)ecrypt
> e
Please enter the key (0 to 25) to use.
> 11
Enter message to encrypt
> Enemy is approaching! Send troops immidiately!
Encrypted Text = Pypxj td laaczlnstyr! Dpyo eczzad txxtotlepwj!

Would you like to (e)ncrypt or (d)ecrypt
> d
Please enter the key (0 to 25) to use.
> 11
Enter message to decrypt
> Pypxj td laaczlnstyr! Dpyo eczzad txxtotlepwj!
Decrypted Text = Enemy is approaching! Send troops immidiately!
```

That's it for *encryption_decryption.py*. Let's move on to hacking the Caesar Cipher.

### Hacking the Caesar Cipher

In Caesar Cipher it is pretty easy to hack encrypted messages even if we don't know the key. There are only 26 possible keys for Caesar Cipher so a computer can try all possible decryptions and display the results to the user. In cryptography this method is called the *brute-force attack*. 

Here is the hack script named *decipher.py*:
```python
## This code will decipher a Caesar Cipher
def main():
    print("Enter the Caesar Cipher text")
    message = input("> ")

    for key in range(26):
        deciphered = ''
        for character in message:
            if character.isalpha():
                ## If the character is an alphabetic character, determine
                ## the offset based on whether it is uppercase or lowercase.
                if character.isupper():
                    offset = 65
                else:
                    offset = 97

                ## Shift the character by the key and wrap around if necessary.
                shift = (ord(character) - offset - key) % 26 + offset
                deciphered += chr(shift)
            else:
                deciphered += character
                
        print("Key: {}: {}".format(key, deciphered))

if __name__ == '__main__':
    main()
```

Note that code is nearly identical to the `caesar_decrypt()` function of *encryption_decryption.py*. The hacking program implements the same
decryption code, except that it does so in a `for` loop, which runs the code
for every possible key.

Unfortunately, the hacking program isn’t sophisticated enough to
identify when it has found the correct key. It relies on a human to read
the output and identify which decryption produced the original English message.

Here is a sample output of the above code:

```
Enter the Caesar Cipher text
> Pypxj td laaczlnstyr! Dpyo eczzad txxtotlepwj!
Key: 0: Pypxj td laaczlnstyr! Dpyo eczzad txxtotlepwj!
Key: 1: Oxowi sc kzzbykmrsxq! Coxn dbyyzc swwsnskdovi!
Key: 2: Nwnvh rb jyyaxjlqrwp! Bnwm caxxyb rvvrmrjcnuh!
Key: 3: Mvmug qa ixxzwikpqvo! Amvl bzwwxa quuqlqibmtg!
Key: 4: Lultf pz hwwyvhjopun! Zluk ayvvwz pttpkphalsf!
Key: 5: Ktkse oy gvvxuginotm! Yktj zxuuvy ossojogzkre!
Key: 6: Jsjrd nx fuuwtfhmnsl! Xjsi ywttux nrrninfyjqd!
Key: 7: Iriqc mw ettvseglmrk! Wirh xvsstw mqqmhmexipc!
Key: 8: Hqhpb lv dssurdfklqj! Vhqg wurrsv lpplgldwhob!
Key: 9: Gpgoa ku crrtqcejkpi! Ugpf vtqqru kookfkcvgna!
Key: 10: Fofnz jt bqqspbdijoh! Tfoe usppqt jnnjejbufmz!
Key: 11: Enemy is approaching! Send troops immidiately!
Key: 12: Dmdlx hr zooqnzbghmf! Rdmc sqnnor hllhchzsdkx!
Key: 13: Clckw gq ynnpmyafgle! Qclb rpmmnq gkkgbgyrcjw!
Key: 14: Bkbjv fp xmmolxzefkd! Pbka qollmp fjjfafxqbiv!
Key: 15: Ajaiu eo wllnkwydejc! Oajz pnkklo eiiezewpahu!
Key: 16: Zizht dn vkkmjvxcdib! Nziy omjjkn dhhdydvozgt!
Key: 17: Yhygs cm ujjliuwbcha! Myhx nliijm cggcxcunyfs!
Key: 18: Xgxfr bl tiikhtvabgz! Lxgw mkhhil bffbwbtmxer!
Key: 19: Wfweq ak shhjgsuzafy! Kwfv ljgghk aeeavaslwdq!
Key: 20: Vevdp zj rggifrtyzex! Jveu kiffgj zddzuzrkvcp!
Key: 21: Uduco yi qffheqsxydw! Iudt jheefi yccytyqjubo!
Key: 22: Tctbn xh peegdprwxcv! Htcs igddeh xbbxsxpitan!
Key: 23: Sbsam wg oddfcoqvwbu! Gsbr hfccdg waawrwohszm!
Key: 24: Rarzl vf nccebnpuvat! Fraq gebbcf vzzvqvngryl!
Key: 25: Qzqyk ue mbbdamotuzs! Eqzp fdaabe uyyupumfqxk!
```

Note how the script tries every `key` and displays all possible encryption breaks even though only one of them (key 11 in this case) is the correct decryption.

You can either skip the rest of the tutorial or choose to improve the current hack script. The rest of the tutorial will teach how to detect English sentences which we will need when we work with other types of ciphers.

### Limitation of the hack script

Although, the *decipher.py* script works as intended and reading through all possible decryptions to find the correct one will take only few seconds it would be better if it gave us the exact correct sentence. All we need to do is find which decryption is an actual English statement. However, the solution to this problem is much more complex than it seems. In fact the solution is a lot of hassle for a situation where the user has to read through only a few lines before they find the correct sentence. Still we will try to fix this problem because:
* Finding solutions to problems is the best way to learn.
* A programmer's job is to make the life of users as easy as possible.
* We will face this problem when we work with other types of ciphers so it is best to find the solution here itself and then we can follow the same approach everywhere else.

## Detecting English sentences

To detect if a sentence is English or not we need to have a word list. Instead of hunting for word lists on the internet, you  can download and use [the *dictionary.txt* file from my GitHub repository](/home/ayush-saha/Projects/Cryptic/static/cryptic-camel.jpg).

The English detection script is named *detect_english.py*. The *dictionary.txt* file must be placed in the same directory as the script for the code to work. Or you can modify the path in the code itself.

Here is the code of *detect_english.py*:

```python
def load_dictionary():
    """
    Loads a dictionary file and returns a dictionary of words in uppercase.
    """
    english_words = {}
    with open('dictionary.txt') as f:
        for word in f.read().split('\n'):
            english_words[word.upper()] = None
    return english_words

dictionary_words = load_dictionary()

def get_english_count(message):
    """
    Calculates the percentage of English words in a message.
    """
    message = message.upper()
    message = remove_non_letters(message)
    possible_words = message.split()
    
    if not possible_words:
        return 0.0 ## No words at all, so return 0.0
    
    matches = 0
    for word in possible_words:
        if word in dictionary_words:
            matches += 1
    return float(matches) / len(possible_words)

def remove_non_letters(message):
    """
    Removes non-letter symbols from a message and returns only letters in it.
    Spaces, tabs and new lines are left as they are.
    """
    letters_only = []
    for symbol in message:
        if symbol.isalpha() or symbol in ' \t\n':
            letters_only.append(symbol)
    return ''.join(letters_only)

def is_english(message, word_percentage=20, letter_percentage=85):
    """
    Returns True if a message is likely to be English, based on word and letter percentages.
    By default, 20% of the words must exist in the dictionary file, and
    85% of all the characters in the message must be letters or spaces
    (not punctuation or numbers).
    """
    words_match = get_english_count(message) * 100 >= word_percentage
    num_letters = len(remove_non_letters(message))
    message_letters_percentage = float(num_letters) / len(message) * 100
    letters_match = message_letters_percentage >= letter_percentage
    return words_match and letters_match
```

There are many things going on here. Let's understand one function at a time.

### Loading the word list

The *dictionary.txt* file has contains English words - one word per line all in uppercase. Here is a part of it:

```
AARHUS
AARON
ABABA
ABACK
ABAFT
ABANDON
ABANDONED
ABANDONING
--snip--
```

To load the *dictionary.txt* file, we use a helper function `load_dictionary()`:

```python
def load_dictionary():
    """
    Loads a dictionary file and returns a dictionary of words in uppercase.
    """
    english_words = {}
    with open('dictionary.txt') as f:
        for word in f.read().split('\n'):
            english_words[word.upper()] = None
    return english_words
```

The `load_dictionary()` function loads the word list  `dictionary.txt` and returns a dictionary of words in uppercase. This function first initializes an empty dictionary called `english_words`. 

**Note:** The name of the file *dictionary.txt* has nothing to do with storing the word list in a dictionary.

Then it reads the `dictionary.txt` file using a `with` statement to ensure the file is properly closed after it's been read. It then loops through each word in the file, converts the word to uppercase using the `upper()` method, and assigns it to the dictionary `english_words` with a value of `None`. We store the words as keys in `english_words` and assign them the value `None` because they don't need to have any value. Remember we are using the dictionary like a list in this case.Finally, it returns the `english_words` dictionary

**Note:** We used a dictionary instead of a list because dictionaries in python are implemented using a data structure called a hash table, which allows for fast access to data. In a list however we would have to loop through every word to find a match.

We call the `load_dictionary()` function and store the returned `english_words` in `dictionary_words`. Now we can use the dictionary `dictionary_words` which contains all the words from *dictionary.txt*.

### Removing non-letters

The `remove_non_letters()` function is needed to remove symbols from the message and returns the message consisting of letters only (along with spaces, tabs and new lines if any).

```python
def remove_non_letters(message):
    """
    Removes non-letter symbols from a message and returns only letters in it.
    Spaces, tabs and new lines are left as they are.
    """
    letters_only = []
    for symbol in message:
        if symbol.isalpha() or symbol in ' \t\n':
            letters_only.append(symbol)
    return ''.join(letters_only)
```

The `remove_non_letters(message)` function removes all non-letter symbols from a message and returns only letters in it. Spaces, tabs, and new lines are left as they are. This function initializes an empty list called `letters_only` and loops through each symbol in the input message. For each symbol, it checks if it is an alphabetic character or a space, tab or newline character, using the `isalpha()` method or a membership check for these characters. If the symbol is one of these characters, it is appended to the `letters_only` list. Finally, the function returns the list of letters as a single string using the `join()` method.

### Calculating percentage of English words

The function `get_english_count()` takes a message as input and returns the percentage of English words in the message. 

```python
def get_english_count(message):
    """
    Calculates the percentage of English words in a message.
    """
    message = message.upper()
    message = remove_non_letters(message)
    possible_words = message.split()
    
    if not possible_words:
        return 0.0 ## No words at all, so return 0.0
    
    matches = 0
    for word in possible_words:
        if word in dictionary_words:
            matches += 1
    return float(matches) / len(possible_words)
```

This helper function first converts the message to uppercase using the `upper()` method and removes all non-letter symbols using the `remove_non_letters()` function. The function then splits the resulting message into a list of possible words using the `split()` method with no argument, which splits by whitespace. If there are no possible words, the function returns 0.0. Otherwise, the function counts the number of words in the message that are in the dictionary of English words loaded by the `load_dictionary()` function. It returns the percentage of matching English words by dividing the number of matches by the total number of possible words and multiplying by 100.

### Checking if the sentence is English or not

The function `is_english()` takes a message as input and returns a boolean indicating whether it is likely to be English or not. 

```python
def is_english(message, word_percentage=20, letter_percentage=85):
    """
    Returns True if a message is likely to be English, based on word and letter percentages.
    By default, 20% of the words must exist in the dictionary file, and
    85% of all the characters in the message must be letters or spaces
    (not punctuation or numbers).
    """
    words_match = get_english_count(message) * 100 >= word_percentage
    num_letters = len(remove_non_letters(message))
    message_letters_percentage = float(num_letters) / len(message) * 100
    letters_match = message_letters_percentage >= letter_percentage
    return words_match and letters_match
```

The function has two optional arguments, `word_percentage` and `letter_percentage`, which are used to set the thresholds for the percentage of English words and letters in the message. By default, the thresholds are set to 20% and 85%, respectively.

The function first calculates the percentage of English words in the message using the `get_english_count` function and checks if it is greater than or equal to the `word_percentage` threshold. It then calculates the percentage of letters in the message using the `remove_non_letters()` function and checks if it is greater than or equal to the `letter_percentage` threshold. Finally, the function returns a boolean indicating whether both conditions are met.

That's it. We are done with *detect_english.py*. We can import this module to any program and use its `is_english()` function do check whether a given message is likely to be English or not. It is not perfect and may produce false positives or false negatives, but it will work for us when we try to hack ciphers.

## Fixing the limitation of the hack script

This is the updated *decipher.py*:

```python
## This code will decipher a Caesar Cipher
import detect_english

def main():
    flag = 0 ## Initialize flag
    count = 0 ## Initialize encryption break count
    print("Enter the Caesar Cipher text")
    message = input("> ")

    for key in range(26):
        deciphered = ''
        for character in message:
            if character.isalpha():
                ## If the character is an alphabetic character, determine
                ## the offset based on whether it is uppercase or lowercase.
                if character.isupper():
                    offset = 65
                else:
                    offset = 97

                ## Shift the character by the key and wrap around if necessary.
                shift = (ord(character) - offset - key) % 26 + offset
                deciphered += chr(shift)
            else:
                deciphered += character
                
        if detect_english.is_english(deciphered):  
            count += 1
            print("Possible encryption break: ")
            print("Key: {}: {}".format(key, deciphered))
            print("Enter D for done or just press Enter to continue breaking:")
            response = input("> ")

            if response.upper().startswith('D'):
                flag = 0
                break
            else:
                flag = 1

    if count == 0:
        print("No possible encryption break found.")

    if flag == 1:
        print("No other possible encryption break found.")

if __name__ == '__main__':
    main()
```

**Note:** For the updated *decipher.py* script to work, you mush have the *detect_english.py* module and the *dictionary.txt* file all in the same directory.

The changes made here are minimal. We imported the `detct_english` module in the beginning. Two variables `flag` and `count` are initialized to zero. `flag` is used to keep track of whether the user has requested to stop the program and `count` is used to keep track of how many possible encryption breaks have been found. 

The `detect_english.is_english()` function is used to check if the `deciphered` message is in English. If the `deciphered` message is in English, the `count` variable is incremented by 1 and the possible encryption break is printed along with the key value used to break it.

The user is prompted to enter 'D' to stop the program or press enter to continue breaking other possible keys. If the user enters 'D', the `flag` variable is set to 0 and the program is stopped. If the user does not enter 'D', the `flag` variable is set to 1 and the program continues to break other possible keys. 

If no possible encryption break is found, the program prints "No possible encryption break found." If the `flag` variable is 1, the program prints "No other possible encryption break found."

Here is a sample output of the modified *decipher.py*:

```
Enter the Caesar Cipher text
> Pypxj td laaczlnstyr! Dpyo eczzad txxtotlepwj!
Possible encryption break: 
Key: 11: Enemy is approaching! Send troops immidiately!
Enter D for done or just press Enter to continue breaking:
> 
No other possible encryption break found.
```

We can now successfully hack a Caesar Cipher without knowing the key.

## Conclusion

Previously, I mentioned that Caesar Cipher is not suitable for secure encryption purposes. This tutorial is intended for beginners who are interested in exploring the field of cryptography and want to enhance their programming skills. While there are numerous encryption methods available, Caesar Cipher is one of the most straightforward to understand. However, I will be posting more content in the future that discusses other types of ciphers in greater depth.