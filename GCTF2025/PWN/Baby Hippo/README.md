# Baby Hippo

<img width="797" height="387" alt="image" src="https://github.com/user-attachments/assets/356ee47b-3bb3-4106-b477-2e6052b88c4d" />

## Challenge

We are given a binary called baby_hippo and a remote instance to connect. The description only says “Say Hi to my Hippo!” and provides a downloadable attachment.

## Analysis
1. Running the binary shows it asks for a username and a password.

2. Inside the code, two heap chunks are allocated:

- Chunk A: stores the username.
- Chunk B: initialized with the value 0xCAFEBABE.

3. The program flow:
- Reads username into A using gets().
- Reads password into A+0x14 (still within chunk A).
- Checks if username == "tiger".
- Then checks if the integer at the start of B is equal to 0x1337BEEF.
- If both conditions are satisfied, it calls the hidden function to print the flag.
**Bug:** `gets()` is unsafe and allows overflowing chunk A into chunk B, so we can overwrite B’s value.

## Solution
- Send `tiger` as the username.  
- For the password, send enough padding to overflow from `A+0x14` into the start of B and overwrite `0xCAFEBABE` with `0x1337BEEF` (little-endian).  
- When the overwrite succeeds, the program passes both checks and reveals the flag.

```python
python3 - <<'PY' | nc 188.166.183.187 33573
import sys, struct
sys.stdout.write("tiger\n")
sys.stdout.flush()
pad = b"A"*60  # adjust; the script above sweeps to find the right value
sys.stdout.buffer.write(pad + struct.pack("<I", 0x1337BEEF) + b"\n")
sys.stdout.flush()
PY
```
This brute-forces the exact offset where the overwrite lands correctly.

<img width="581" height="334" alt="image" src="https://github.com/user-attachments/assets/72c299bc-e288-4855-b59f-3f16bf7d9d9c" />


## Flag 
```flag
Flag: GCTF25{d6ee19f85b9b_B3B3k_I5_Du(K}
```

