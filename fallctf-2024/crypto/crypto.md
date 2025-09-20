---
title: Cryptography
---
# Crypto
> "*never roll your own crypto*"

## Crypto concepts and terms
Wikipedia has *relatively* good resources and definitions on both encoding and cryptographic schemes, so any time you come across an unfamiliar encryption, I highly recommend checking them out.
- **Encoding**: to convert something from one system of communication to another
> [!IMPORTANT]  
> encoding is not intended to be secure against unauthorized access but rather to ensure data compatibility across different systems
- **Encryption**: a process that transforms data (plaintext) into an unreadable format (ciphertext) using a key. 
- **Cipher**: an algorithm for performing encryption or decryption.
- **Ciphertext**: The encrypted message (usually looks like gobbledegook)
- **Key**: part of the input into a cryptographic function that modifies the function’s operations while creating ciphertext in such a way that you have to have the key to get the plaintext from a decryption function
- **Symmetric Cipher**: The same key is used to encrypt and decrypt the message. For example, ROT13.
- **Asymmetric Cipher**: Two distinct yet related keys (public and private) are used to encrypt and decrypt the message. For example, RSA.

### Classical Ciphers
In the old *old* days of crypto (2000+ years ago! 😮) we had the **Caesarean Cipher**. 

The Caesarian Shift cipher, or Caesar cipher is a substitution method that involves rotating an alphabet by key n and substituting the rotated letters for the plaintext letters

For instance, if n=4, then we have our original alphabet and shifted alphabet below:

`ABCDEFGHIJKLMNOPQRSTUVWXYZ`
`EFGHIJKLMNOPQRSTUVWXYZABCD`

So `A` gets mapped to `E`, `B -> F`, and so on. A simple message like `sigpwny` becomes `wmktarc`

So if the shift is 4, how do we get back the plaintext?

### Mod arithmetic primer
Modular arithmetic is arithmetic with remainders. We fix some modulus `n` throughout. Every time we add and multiply two numbers `x` and `y`, we will divide by `n` and keep the remainder. This is written as `x + y (mod 26)`

This info allows us to write a quick caesar encoding program:
```py
message = 'sigpwny'
shift = 4
ciphertext = ''
for c in message:
  x = ord(c) - 97 #Get the integer code for each ASCII character in the message
  x = (x + shift) % 26 #shift to the right by 4 and mod 26
  ciphertext += chr(x + 97) #convert back to character and add it to the ciphertext
print(ciphertext) #prints 'wmktarc'
```
And similarly, decryption is

```py
ciphertext = "wmktarc"
shift = 4
message = ''
for c in ciphertext:
  x = ord(c) - 97 
  x = (x - shift) % 26 #essentially "shift back"
  message += chr(x + 97)
print(message) #prints 'sigpwny'
```

Nowadays, you can see why this is very insecure. There are only 26 possible shifts, 0 through 25. So given some ciphertext that we know came from an English word, we can just bruteforce all possible shifts and just keep the one that looks like English. For example, you know that the flags must contain the word “sigpwny” at the start! This is why classical ciphers are no longer used. Because computer are really, really fast at bruteforcing small combinations, classical ciphers are essentially useless.

### XOR and One-Time-Pads
One of the most basic modern cryptographic tools is the Exclusive-Or operation, also called XOR. In math it gets denoted
by `x ⊕ y`, while in code we use `x ^ y`.
If `x` and `y` are 2 bits (0 or 1), `x ^ y` is defined to be one if and only if one of `x` or `y` is 1, but not both. Another way to think about it is that the XOR returns `1` if the bits are different:

| x | y | x^y |
|---|---|-----|
| 0 | 0 | 0   |
| 0 | 1 | 1   |
| 1 | 0 | 1   |
| 1 | 1 | 0   |


To apply this to whole messages m with some key `k`, we convert the message to a bunch of bits in some method and just do this bit by bit. So for instance, `0101 ^ 1110 = 1010`

The relevant properties of the XOR operation is that:
1. `(x ^ y) ^ z = x ^ (y ^ z)`
2. `x ^ x = 0`
3. `x ^ 0 = x`
4. `x ^ y = y ^ x`
5. `x ^ y = z ==> x = y ^ z`


## Modern Ciphers

Modular arithmetic is much more useful than what was previously advertised when talking about classical ciphers. A major theme of cryptography is the idea of a trapdoor: something that is easy to enter one way but hard to escape from the other way. Prime numbers give us this trapdoor. It is incredibly easy for us (or rather, our computers) to multiply two numbers, even if those numbers are 512 bits long or even 1024 bits long. However, factoring numbers into primes is computationally difficult. This trapdoor will be the basis of the **RSA cryptosystem, named after Rivest, Shamir, and Adleman**.

Interestingly enough, RSA is an *assymetric* encryption instead of a symmetric one. This means the sender of a message does not have to know the secret key to be able to successfully encrypt a message. The math almost magically seems to work out! The goal of challenges like this will be to try to identify some, or all, of the secret information to be able decrypt the flag.

The first step is that two large (and i mean *large*) primes `p` and `q` are chosen. Then, a value `n = pq` is computed. Everyone in the world is allowed to know this `n`, but `p, q` are kept secret. Furthermore, the world is given access to an exponent, `e`. For many reasons (most of which are unimportant for now), usually we select `e = 2**(16) + 1 = 65537`. Another secret value `φ = (p-1)(q-1)` is completed, and `d` such that `ed == 1 mod φ`. In python, you can easily calculate this with 
`d = pow(e, -1, φ)`. Afterwards, the ciphertext `c` can be computed with our public information:
$$c = m^e \mod n$$
And decryption is done with `d`, so 
$$m = c^d \mod n$$
 So in short, the secrets are `d, p, q` and the publically released values are `n, e`. 

 Using [PyCryptodome](https://www.pycryptodome.org/), it is possible to convert text data into large integers, which are used for RSA. Since `d` is kept secret, and `p, q, n` are sufficiently large enough, it would in theory be too hard to factor `p, q`. 
 
 But if the values were safely chosen, this wouldn't be a CTF now would it?



 ## General Advice
 - Every challenge author has their own way of converting between messages and numbers and bytes and other data formats. Read the source code and make sure you know how these are being done and what objects you are working with. Whatever they are doing to convert the flag into workable data, doing the reverse of that should be part of your decryption.
- **Get out scratch paper**. A lot of crypto relies on math. Math is not a spectator sport. You will need to get your hands dirty, and perhaps use a python interpreter to test your math.
- **Google**: A huge number of ctf challenges including the hardest ones will make use of an obscure idea or encryption scheme that *has been shown to be vulnerable before*. Google is your best friend for this.

## Tools of the Trade
[SageMath](https://www.sagemath.org): An open source math software system that has many tools to help solve mathematically complex problems. These include methods of breaking encryption schemes.

[CyberChef](https://gchq.github.io/CyberChef/): For Text manipulation, processing, ciphers, encoding, and even basic cracking

[FeatherDuster](https://github.com/nccgroup/featherduster): For Cipher identification, this is your "Crypto Recon" tool.

[dCode](https://www.dcode.fr/tools-list#cryptography): Has a lot of readily available encryption/decryption solutions

