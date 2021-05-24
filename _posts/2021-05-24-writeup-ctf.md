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
base64.b64decode(base64.b32decode(s))
```
```
b'whfg znxr lbh bcra hc lbhe rlrf\n\nUPZHF-PGS{Jr_ner_Oynpxcvaxre_jrypbzr_gb_upzhf_pgs_2021}'
```
UPZHF-PGS{Jr_ner_Oynpxcvaxre_jrypbzr_gb_upzhf_pgs_2021} is similar to the flag format and UPZHF-PGS ~ HCMUS-CTF => Caesar Cipher - shift by key = 13

{: .box-note}
**==> Flag:** HCMUS-CTF{We_are_Blackpinker_welcome_to_hcmus_ctf_2021}