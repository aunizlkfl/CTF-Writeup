# Nyeow Nyeow PDF
> Nyeow Nyeow did a little silly. But fear not, Nyum Nyum make the actual PDF generator now. Secret? What secret? Nyum dont know. Nyum just following /nyan-nyan
> Hint : I can't believe our intern leaked our source code online. AGAIN

## Challenge

You get a Flask app with two important routes:

- /nyan-nyan returns the flag only if the request comes from 127.0.0.1 (localhost).
- /generate fetches a user-supplied URL and converts its HTML to PDF.

The app checks the initial URL’s host to block private IPs, but it still follows redirects without re-checking the final destination. That means we can use a public open redirect to bounce the server into calling http://127.0.0.1:5000/nyan-nyan.

Key parts of `app.py` that make the bug possible:

Flag endpoint is local-only :
```python
@app.route('/nyan-nyan')
def secret():
    # Only return flag if the caller is 127.0.0.1 / ::1
    if request.remote_addr not in ('127.0.0.1', '::1'):
        return ('Forbidden', 403)
    with open(FLAG_PATH, 'r') as f:
        return make_response(f.read(), 200)
```
Only checks the host you submit (before any redirects) :
```python
def is_private_host(hostname):
    infos = socket.getaddrinfo(hostname, None)
    for info in infos:
        addr = info[4][0]
    # returns True for private/loopback ranges; otherwise False
    # (only checks the ORIGINAL hostname you submit)
    ...
```

```python
@app.route('/generate', methods=['POST'])
def generate():
    url = request.form.get('url', '')
    parsed = urlparse(url)
    host = parsed.hostname

    if host and is_private_host(host):
        return ('Invalid target', 400)

    # IMPORTANT: requests.get() follows redirects by default
    resp = requests.get(url, timeout=10)

    html = resp.text
    pdf_bytes = pdfkit.from_string(html, False)
    return send_file(io.BytesIO(pdf_bytes), mimetype='application/pdf', as_attachment=True, download_name='output.pdf')

```

- Bug: It validates only the first URL, then follows redirects without re-checking.
- So if we give it a public open-redirect that bounces to http://127.0.0.1:5000/nyan-nyan, the server will fetch the flag as localhost and embed it into the PDF.

## Solution

Use an open-redirector to point /generate at 127.0.0.1 after the first fetch:
```
https://httpbin.org/redirect-to?url=http://127.0.0.1:5000/nyan-nyan
```

<img width="1011" height="249" alt="image" src="https://github.com/user-attachments/assets/5240e2fd-f26a-47f4-bf2e-fc8ead433058" />

When /generate follows that redirect, the server requests /nyan-nyan from localhost, which returns the flag. The response HTML is then embedded into the generated PDF.

## Flag

Open flag.pdf — the flag is inside.
<img width="1113" height="107" alt="image" src="https://github.com/user-attachments/assets/61deeca0-4724-4a01-911d-14bac96f44b9" />


