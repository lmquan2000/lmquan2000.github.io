---
layout: post
title: Write-up for HCMUS-CTF Warm-Up Stage
subtitle: CRYPTOGRAPHY
---
### SanityCheck
```
MQZGQ3K2PFBDMYTONB4USR3YNFQUGQTJLEZUU2CJI5UGUSKHPBUWCR2VM5RW26DZLJTW6S2WKZBGCU2FLF2FKRLEKRSTA4DZLAZDK3DDNQ4VAZKXGV3WKR2OGJMVQ2DZLJLDS4LDNZWHOWLOOB4VQMTENFMDGVTXMVWWQ3KYGNBG4YZRHB4U2RCJPBTFCPJ5  
```
Guess that the message is encoded in Base32
```python
base64.b32decode(s32).decode()
```
```
d2hmZyB6bnhyIGxiaCBiY3JhIGhjIGxiaGUgcmxyZgoKVVBaSEYtUEdTe0pyX25lcl9PeW5weGN2YXhyZV9qcnlwYnpyX2diX3VwemhmX3Bnc18yMDIxfQ==
```
You see == at the end => It's Base64 encode
```python
base64.b64decode(s64).decode()
```
```
whfg znxr lbh bcra hc lbhe rlrf

UPZHF-PGS{Jr_ner_Oynpxcvaxre_jrypbzr_gb_upzhf_pgs_2021}
```
UPZHF-PGS{Jr_ner_Oynpxcvaxre_jrypbzr_gb_upzhf_pgs_2021} is similar to the flag format and UPZHF-PGS ~ HCMUS-CTF => Caesar Cipher - shift by key = 13

{: .box-note}
**==> Flag:** HCMUS-CTF{We_are_Blackpinker_welcome_to_hcmus_ctf_2021}

### SingleByte
```
r4SJmJOanoOFhMqDmcqLyp2Lk8qFjMqZiZiLh4iGg4SNyo6LnovKmYXKnoKLnsqFhIaTyoufnoKFmIOQj47KmouYnoOPmcqJi4TKn4SOj5iZnouEjsqego/Kg4SMhZiHi56DhYTEyqOEyp6PiYKEg4mLhsqej5iHmcbKg57Kg5nKnoKPypqYhYmPmZnKhYzKiYWEnI+YnoOEjcqCn4eLhMeYj4uOi4iGj8qahouDhJ6Pkp7KnoXKg4SJhYeamI+Cj4SZg4iGj8qej5KexsqLhpmFyoGEhZ2EyouZyomDmoKPmJ6Pkp6iqae/ucepvqyRnY+1gYSFnbWegouetZOFn7WJi4S1joW1mYOHmoaPtbKluLXf3tnb2dvf3ouIiYyP396LjNiPiYuIlw==
```
== at the end => Base64 encode
```python
s = base64.b64decode(s64).decode()
```
The title is single byte encode. Do u find it familiar ?? => XOR encode  
Also, remember format of the flag: HCMUS-CTF{...}. The key may be
```python
k = s[0] ^ ord('H')
```
or 
```python
k = s[1] ^ ord('C')
```
=> Try all possible key and finally i succeed.

```python
k = s[-1] ^ ord('}')
print(''.join(chr(c ^ k) for c in s))
```

```
Encryption is a way of scrambling data so that only authorized parties can understand the information. In technical terms, it is the process of converting human-readable plaintext to incomprehensible text, also known as ciphertextHCMUS-CTF{we_know_that_you_can_do_simple_XOR_54313154abcfe54af2ecab}
```


{: .box-note}
**==> Flag:** HCMUS-CTF{we_know_that_you_can_do_simple_XOR_54313154abcfe54af2ecab}

### TheChosenOne
Given an AES-ECB encryption oracle. The problem is a basic AES-ECB blockcipher Chosen Plaintext Attack.  
First, you should determine the size of the block. Look into the given script **server.py**

```python
def padding(plaintext):

    plaintext_length = len(plaintext)
    padding_length = 0
    
    if plaintext_length % 32 != 0:
        padding_length = (plaintext_length // 32 + 1) * 32
    else:
        padding_length = 0
    return padding_length
```
Guess that the blocksize is 32. However, you can verify by sending from 1 to n characters to find the block size. 
Second, you should find the offset if chosen plaintext does not start as the first byte of a block. But in this challenges, it's easier than expected. The server code shows that our plaintext appear as first part of text for encryption.

```python
def main():
    flag = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # TODO 
    key = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" # TODO
    
    padding_character = "D"
    
    assert (len(flag) == 32) and (len(key) == 32)
    cipher = AES.new(key, AES.MODE_ECB)

    banner = """
```

Another useful information is the length of flag = 32.  
**Attack!**
Idea of the attacking method is sending blocksize - 1 byte for the encryption oracle and the last byte of block is the first of the flag. Then you just bruteforce to find that character. Repeating doing this for the rest of flag.

```python
index = 0
res = []
while True:
    inp_str = 'A' * (31 - index)
    sendline(inp_str)

    cipher_text = recv_cipher()
    try_inp_str = inp_str

    for j in res:
        try_inp_str += j
    for i in range(32, 126):
        tmp_inp_str = try_inp_str + chr(i)
        sendline(tmp_inp_str)

        try_cipher_text = recv_cipher()
        if try_cipher_text[:64] == cipher_text[:64]:
            res.append(chr(i))
            break
    index += 1
```

{: .box-note}
**==> Flag:** HCMUS-CTF{You_Can_4ttack_A3S!?!}  
