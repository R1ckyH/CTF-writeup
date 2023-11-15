# Decompetiton: hard.pyc (300 points, 9 solves)
# 逆競賽: Py both

By [`R1ckyH`](https://github.com/R1ckyH) and [`Sunny`](https://github.com/LoSunny)

是咁的，請B6a還我眼睛，賠醫藥費！

When I open the
[zip file](https://file.hkcert23.pwnable.hk/decomp-pyhard_d41ee23047e7a3d61ad3b9391744718c.zip)
, I find four files, `flag.txt`, `compiler.py`, and `internal_flag.py`, `pyhard.cpython-312.pyc`

After viewing the files, we can find that `flag.txt` and `internal_flag.py` is useless. ~~fuck~~

We reach out that to win this game, we need to reverse the file `pyhard.cpython-312.pyc` to `python` source code, detail to space ~~what the fuck man?~~

compiler.py:  `The similarity is too low! You need to recover at least 95% (0.95) of the original source code first!`

In order to pass 95%, we use [`pycdc`](https://github.com/zrax/pycdc) to reverse `pyhard.cpython-312.pyc`

We got this:
```python
# Source Generated with Decompyle++
# File: pyhard.cpython-312.pyc (Python 3.12)

Unsupported opcode: CALL_FUNCTION_EX
import webbrowser
import pickle
data = b'\x80\x04\x95\xed\x00\x00\x00\x00\x00\x00\x00(K\x00K\x00K\x00K\x00K\x03K\x00C(\x97\x00\x02\x00e\x00d\x00\xab\x01\x00\x00\x00\x00\x00\x00Z\x01e\x01d\x01k(\x00\x00r\x03d\x02Z\x02y\x04d\x03Z\x02y\x04\x94(\x8c\rwhat is key? \x94\x8c$95b4e2a4-40c1-405d-bf63-86c76f3cebd9\x94\x8c\x08c0mpl3xx\x94\x8c\x08\x00\x00\x00\x00\x00\x00\x00\x00\x94Nt\x94\x8c\x05input\x94\x8c\x01a\x94\x8c\rINITIAL_STATE\x94\x87\x94)\x8c\x08<string>\x94\x8c\x08<module>\x94h\x0bK\x01C \xf0\x03\x01\x01\x01\xe1\x04\t\x88/\xd3\x04\x1a\x80\x01\xd8\x1e\x1f\xd0#I\xd2\x1eI\x90\n\x81\r\xc8v\x81\r\x94C\x00\x94))t\x94.'
cipher = b'\xcc\x9f\xe3\xd6\xe2\x9e\xdf\xe3\xd6\x9a\xa7\xd6\xe8\x94\xb5\xef\xcb\x99\xd7\xdb\xb7\x98\xe7\x9c\x99X\x95\xb4\xe6_\xa2\xf0\xe2r\xb7\x80\xb3\x81\x8d\xcdz\x84'

Error decompyling pyhard.cpython-312.pyc: vector::_M_range_check: __n (which is 3) >= this->size() (which is 2)
get = lambda : 
```

Well ~~fuck b6a python 3.12 reverse~~ we can find part of the `code` and save some time (really?)

We use `pycdas` in [`pycdc`](https://github.com/zrax/pycdc) to view the `bytecode`

```python
pyhard.cpython-312.pyc (Python 3.12)
[Code]
    File Name: pyhard.py
    Object Name: <module>
    Qualified Name: <module>
    Arg Count: 0
    Pos Only Arg Count: 0
    KW Only Arg Count: 0
    Stack Size: 5
    Flags: 0x00000000
    [Names]
        'webbrowser'
        'pickle'
        'data'
        'cipher'
        'get'
        'type'
        '__code__'
        'codeobj'
        'loads'
...
...
        302     LOAD_NAME                     0: webbrowser
        304     LOAD_ATTR                     36 <INVALID>
        324     LOAD_CONST                    12: 'https://www.youtube.com/watch?v=4HBIxZ5Bs7c'
        326     CALL                          1
        334     POP_TOP
        336     PUSH_NULL
        338     LOAD_NAME                     19: Exception
        340     LOAD_CONST                    13: 'Invalid command'
        342     CALL                          1
        350     RAISE_VARARGS                 1
```

After reverse these python `bytecode` hand by hand, we got the full `code` (95%) ~~Fuck B6a~~:

```python
import webbrowser
import pickle
import dis

data = b'\x80\x04\x95\xed\x00\x00\x00\x00\x00\x00\x00(K\x00K\x00K\x00K\x00K\x03K\x00C(\x97\x00\x02\x00e\x00d\x00\xab\x01\x00\x00\x00\x00\x00\x00Z\x01e\x01d\x01k(\x00\x00r\x03d\x02Z\x02y\x04d\x03Z\x02y\x04\x94(\x8c\rwhat is key? \x94\x8c$95b4e2a4-40c1-405d-bf63-86c76f3cebd9\x94\x8c\x08c0mpl3xx\x94\x8c\x08\x00\x00\x00\x00\x00\x00\x00\x00\x94Nt\x94\x8c\x05input\x94\x8c\x01a\x94\x8c\rINITIAL_STATE\x94\x87\x94)\x8c\x08<string>\x94\x8c\x08<module>\x94h\x0bK\x01C \xf0\x03\x01\x01\x01\xe1\x04\t\x88/\xd3\x04\x1a\x80\x01\xd8\x1e\x1f\xd0#I\xd2\x1eI\x90\n\x81\r\xc8v\x81\r\x94C\x00\x94))t\x94.'
cipher = b'\xcc\x9f\xe3\xd6\xe2\x9e\xdf\xe3\xd6\x9a\xa7\xd6\xe8\x94\xb5\xef\xcb\x99\xd7\xdb\xb7\x98\xe7\x9c\x99X\x95\xb4\xe6_\xa2\xf0\xe2r\xb7\x80\xb3\x81\x8d\xcdz\x84'
get = lambda: cipher.hex()

codeobj = type(get.__code__)
key = codeobj(*pickle.loads(data))
eval(key)

def checker_gen(ii, kk):
    for i,x in enumerate(ii):
        c = kk[i % len(kk)]
        x ^= i
        x += c
        x %= 256
        yield x


def attempt(flag):
    gen = checker_gen(flag.encode(), INITIAL_STATE.encode())
    for c in cipher:
        if c != next(gen):
            print('Incorrect!')
            exit(1)
    print('Correct!')
a = input().split(' ')

match a:
    case ['get', *_]:
        print(get())
    case ['attempt', flag]:
        attempt(flag)
    case _:
        webbrowser.open('https://www.youtube.com/watch?v=4HBIxZ5Bs7c')
raise Exception('Invalid command') 
```

Well, we pass `> 95%` rules

`What is the internal flag hidden inside the pyc file?`

wtf? we need `internal flag`? what is Internal flag?

There is nothing about `internal flag` in the code!

Well, let's try to run the `code`!

```what is the key```

well, let use show the `bytecode` of `data` with python module `dis`

```python
key = codeobj(*pickle.loads(data))
print(dis.code_info(key))
print(dis.dis(key))
```

```python
Name:              <module>
Filename:          <string>
Argument count:    0
Positional-only arguments: 0
Kw-only arguments: 0
Number of locals:  0
Stack size:        3
Flags:             0x0
Constants:
   0: 'what is key? '
   1: '95b4e2a4-40c1-405d-bf63-86c76f3cebd9'
   2: 'c0mpl3xx'
   3: '\x00\x00\x00\x00\x00\x00\x00\x00'
   4: None
Names:
   0: input
   1: a
   2: INITIAL_STATE
  0           0 RESUME                   0

  2           2 PUSH_NULL
              4 LOAD_NAME                0 (input)
              6 LOAD_CONST               0 ('what is key? ')
              8 CALL                     1
             16 STORE_NAME               1 (a)

  3          18 LOAD_NAME                1 (a)
             20 LOAD_CONST               1 ('95b4e2a4-40c1-405d-bf63-86c76f3cebd9')
             22 COMPARE_OP              40 (==)
             26 POP_JUMP_IF_FALSE        3 (to 34)
             28 LOAD_CONST               2 ('c0mpl3xx')
             30 STORE_NAME               2 (INITIAL_STATE)
             32 RETURN_CONST             4 (None)
        >>   34 LOAD_CONST               3 ('\x00\x00\x00\x00\x00\x00\x00\x00')
             36 STORE_NAME               2 (INITIAL_STATE)
             38 RETURN_CONST             4 (None)
```

By analyzing the code, we could see it first request the user to input a `Key`, which will be stored in a

Then it compares if it matches `95b4e2a4-40c1-405d-bf63-B6c76f3cebd9`

If it matches then it will set `INITIAL_STATE` to `c0mpl3xx`, else it will set to hex `00000000`.

After retrieving the code, there is a `checker_gen` function, which will return a generator

At the function attempt, we can see it will compare the output of `checker_gen` with each character in the `flag`.

By understanding that concept, we can create a `python script` to loop through all combinations of the `flag`.

```python
from Crypto.Util.number import long_to_bytes

def checker_gen(ii, kk):
    for i,x in enumerate(ii):
        #print(i,x)
        c = kk[i % len(kk)]
        x ^= i
        x += c
        x %= 256
        yield x

cipher = b'\xcc\x9f\xe3\xd6\xe2\x9e\xdf\xe3\xd6\x9a\xa7\xd6\xe8\x94\xb5\xef\xcb\x99\xd7\xdb\xb7\x98\xe7\x9c\x99X\x95\xb4\xe6_\xa2\xf0\xe2r\xb7\x80\xb3\x81\x8d\xcdz\x84'
flag = "internal{"
cha = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!#$%&'()*+,-./:;<=>?@[\]^_`{|}~"

def get_new(flag):
    for i in cha:
        r = True
        test = flag + i
        gen = checker_gen((flag + i).encode(), "c0mpl3xx".encode())
        for c in cipher[:len(flag)+1]:
            a = next(gen)
            if c != a:
                r = False
                break
        if r:
            return test
    
for i in range(33):
    flag = get_new(flag)
    print(flag)
```

result:

```
internal{c
internal{c0
internal{c0m
internal{c0mp
internal{c0mpl
internal{c0mpl3
internal{c0mpl3x
internal{c0mpl3xx
internal{c0mpl3xxx
internal{c0mpl3xxxx
internal{c0mpl3xxxxx
internal{c0mpl3xxxxx_
internal{c0mpl3xxxxx_p
internal{c0mpl3xxxxx_py
internal{c0mpl3xxxxx_py3
internal{c0mpl3xxxxx_py3.
internal{c0mpl3xxxxx_py3.1
internal{c0mpl3xxxxx_py3.12
internal{c0mpl3xxxxx_py3.12_
internal{c0mpl3xxxxx_py3.12_f
internal{c0mpl3xxxxx_py3.12_f1
internal{c0mpl3xxxxx_py3.12_f14
internal{c0mpl3xxxxx_py3.12_f14g
internal{c0mpl3xxxxx_py3.12_f14g_
internal{c0mpl3xxxxx_py3.12_f14g_c
internal{c0mpl3xxxxx_py3.12_f14g_ch
internal{c0mpl3xxxxx_py3.12_f14g_ch3
internal{c0mpl3xxxxx_py3.12_f14g_ch3c
internal{c0mpl3xxxxx_py3.12_f14g_ch3ck
internal{c0mpl3xxxxx_py3.12_f14g_ch3ck3
internal{c0mpl3xxxxx_py3.12_f14g_ch3ck3r
internal{c0mpl3xxxxx_py3.12_f14g_ch3ck3r?
internal{c0mpl3xxxxx_py3.12_f14g_ch3ck3r?}
```

Internal flag is here: `internal{c0mpl3xxxxx_py3.12_f14g_ch3ck3r?}`


Flag here!
```
Congrats! You successfully recover most of the original source code with similarity 95.58011049723757%!
Now go ahead to reverse the hidden flag inside that pyc! The format should be internal{some_internal_flag}
What is the internal flag hidden inside the pyc file?internal{c0mpl3xxxxx_py3.12_f14g_ch3ck3r?}
You master the skill of reversing python!
The flag to be submit is:  hkcert23{d1d_u_3ven_us3_th0s3_pyth0n_f3ature5?4t_l3as5_it_h0p3ful1y_m3ssup_y0ur_dec0mpilers}
```

btw 我隻眼好痛 ~~b6a幾時比醫藥費~~