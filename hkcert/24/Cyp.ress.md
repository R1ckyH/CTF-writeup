# Cyp.ress (200 points, 9 solves)

By [`R1ckyH`](https://github.com/R1ckyH)

~~ChatGPT~~(Claude) is my best friend
是咁的，我啲醫藥費GPT幫我出左了！

### Challenge Description

```
You will get sser.pyc when you reverse the title. Now reverse it back for me.
```

We use `pycdas` in [`pycdc`](https://github.com/zrax/pycdc) to view the `bytecode`

After decode to bytecode, we can send it to ~~ChatGPT~~ Claude

[Claude-3-Haiku](https://poe.com/Claude-3-Haiku) is used in here in free

### Using pycdc
```bytecode
sser.cpython-312.pyc (Python 3.12)
[Code]
File Name: sser.py
Object Name: <module>
Qualified Name: <module>
Arg Count: 0
Pos Only Arg Count: 0
KW Only Arg Count: 0
Stack Size: 6
...
...
...
...
478 STORE_NAME 25: c1
480 PUSH_NULL
482 LOAD_NAME 26: print
484 LOAD_CONST 11: '🙆🙅'
486 LOAD_NAME 16: c0
488 LOAD_NAME 25: c1
490 COMPARE_OP 55 (!=)
494 BINARY_SUBSCR
498 CALL 1
506 POP_TOP
508 RETURN_CONST 1: None
please help me change the python bytecode to python code
```

## Result of Claude

```python
import os
import requests
from Crypto.Cipher import AES
import hashlib

def get_nonce():
    nonce = os.urandom(16)
    while True:
        h = hashlib.sha256(b'pow/' + nonce).digest()
        if h[:3] == b'\x00\x00\x00':
            return nonce
        nonce = os.urandom(16)

print("What is the flag?> ", end="")
flag = input().encode()
nonce = get_nonce()

r = requests.post("https://c12-cypress.hkcert24.pwnable.hk/", json={"nonce": nonce.hex()})
c0 = bytes.fromhex(r.text)

key = hashlib.sha256(b'key/' + nonce).digest()[:16]
iv = hashlib.sha256(b'iv/' + nonce).digest()[:16]
cipher = AES.new(key, AES.MODE_CFB, iv)
c1 = cipher.encrypt(flag)

print("🙆🙅" if c0 != c1 else "")
```

In the code we can find that request a input and encrypt the input to verify the input is equal to the flag encrypted by AES

But if you look carefully, the encrypted flag is store in `c0`

so we just need to modify some code

```python
#print("What is the flag?> ", end="")
#flag = input().encode()
nonce = get_nonce()

r = requests.post("https://c12-cypress.hkcert24.pwnable.hk/", json={"nonce": nonce.hex()}, verify=False)
c0 = bytes.fromhex(r.text)

key = hashlib.sha256(b'key/' + nonce).digest()[:16]
iv = hashlib.sha256(b'iv/' + nonce).digest()[:16]
cipher = AES.new(key, AES.MODE_CFB, iv)
print(cipher.decrypt(c0))
#c1 = cipher.encrypt(flag)
```

Then the flag can be decrypted easily!