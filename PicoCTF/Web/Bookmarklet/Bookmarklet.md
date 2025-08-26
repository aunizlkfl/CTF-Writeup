# 📑 Bookmarklet — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/6fcb3dfa-18cf-40db-9f61-23b318c5914c)

---

## 📌 Challenge Info
- 🎯 **Category:** Web Exploitation  
- 🌱 **Difficulty:** Easy  
- 🏆 **Event:** picoCTF 2025  

**📝 Description:**  
We’re given a site that shows a **JavaScript bookmarklet**. The page claims “your browser has successfully received the flag.” Let’s inspect the code and recover it 👀

---

## 🛠️ Steps to Solve

### 1️⃣ Inspect the Page 🔍
The page contains a textbox with some JavaScript.

📸 Screenshot:  
![Textbox with JS](https://github.com/user-attachments/assets/282ba2e2-5405-4cdf-8e22-f7602cd60346)

The bookmarklet code:

```javascript
javascript:(function() {
    var encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÓÇ¡¥Ìí";
    var key = "picoctf";
    var decryptedFlag = "";
    for (var i = 0; i < encryptedFlag.length; i++) {
        decryptedFlag += String.fromCharCode(
            (encryptedFlag.charCodeAt(i) - key.charCodeAt(i % key.length) + 256) % 256
        );
    }
    alert(decryptedFlag);
})();
```
---

### 2️⃣ Understand the Script Logic 🧩
What the code does, step by step:

- `encryptedFlag` is a string of odd-looking characters (the ciphertext).  
- `key = "picoctf"` acts like a repeating key.  
- For each character `i` in `encryptedFlag`:  
  - Take its char code.  
  - Subtract the char code of `key[i % key.length]` (so the key repeats).  
  - Add `256` and `% 256` to keep it in byte range.  
  - Convert back to a character and append to `decryptedFlag`.  
- Finally `alert(decryptedFlag)` pops the flag.

➡️ In short: a simple **per-character shift** (Vigenère-ish on bytes) using `"picoctf"` as the repeating key.

---

### 3️⃣ Option A — Run the Bookmarklet in Browser ⚡
1. Copy everything starting from `javascript:(function(){ ... })();`  
2. Paste into the browser’s address bar **or** run inside DevTools Console.  
3. A popup will show the flag.

---

### 4️⃣ Option B — Decrypt Locally with Python 🐍
Create a file named `decrypt.py`:

```python
# decrypt.py
encryptedFlag = "àÒÆÞ¦È¬ëÙ£ÖÓÚåÛÑ¢ÕÓÓÇ¡¥Ìí"
key = "picoctf"

decryptedFlag = ""
for i in range(len(encryptedFlag)):
    decryptedFlag += chr(
        (ord(encryptedFlag[i]) - ord(key[i % len(key)]) + 256) % 256
    )

print("Decrypted flag:", decryptedFlag)
```
Run it:
```bash
python3 decrypt.py
```
Output
```text
Decrypted flag: picoCTF{p@g3_turn3r_0c0d211f}
```
#🎯 Flag
```text
picoCTF{p@g3_turn3r_0c0d211f}
```
✨ Game over — bookmarklet cracked ✨


---

### Explanation:
🔎 What Went Wrong

❌ Sensitive value (the flag) was embedded client-side.

❌ The bookmarklet revealed the full decryption logic.

❌ Anyone can inspect, copy, and reproduce the decryption offline.

📝 Lessons Learned

✅ Never trust client-side secrecy — users can always read JS.

✅ Don’t ship decryption keys/logic alongside ciphertext if secrecy matters.

✅ Use server-side checks for sensitive material; treat obfuscation as cosmetic, not security.
