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
base64.b64decode(s64).decode()
```
The title is single byte encode. Do u find it familiar ?? => XOR encode  
Also, remember that format of flag: HCMUS-CTF{...}. The key may be
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
