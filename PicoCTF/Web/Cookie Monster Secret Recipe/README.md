# 🍪 Cookie Monster Secret Recipe — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/58de94d8-6f7b-4a23-bb41-1daa80d78b19)

---

## 📌 Challenge Info
- **Category:** Web Exploitation
- **Difficulty:** Easy
- **Event:** picoCTF 2025

**Description:**  
Cookie Monster has hidden his top-secret recipe inside his website 🍪. Our mission? Outsmart Cookie Monster and uncover the hidden cookie flag!

---

## 🛠️ Steps to Solve

### 1️⃣ Intercept the Login Request
At the login page, input any credentials (for example: `admin:admin`).  
Before hitting enter:
- Open **Burp Suite**
- Enable **Proxy Intercept**
- Make sure **FoxyProxy** in enabled in your browser.

📸 Screenshot:

![Login Intercept](https://github.com/user-attachments/assets/cf08afb1-c98f-4e3e-a19d-ecb5ca178cf2)

---

### 2️⃣ Send Request to Repeater
Once Burp Suite captures the request:
- Send it to **Repeater** (`CTRL + R`).
- Resend the request.
- Check the **Set-Cookie** header → you will find something interesting:

```
Set-Cookie: secret_recipe=cGljb0NURntjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzX0M0MzBBRTIwfQ%3D%3D
```

✨ Highlight the encoded text!

📸 Screenshot:

![Cookie Capture](https://github.com/user-attachments/assets/d0ac00ad-04c1-41f9-98a4-4e5f1d8b4163)

---

### 3️⃣ Decode the Cookie
Open **Inspector** in Burp Suite. The value is encoded twice:
1. First: **URL Encoding → Base64**
2. After decoding, you’ll get the flag 🎉

📸 Screenshot:

![Decoded Flag](https://github.com/user-attachments/assets/83877965-8e08-47e5-8abc-84d4cf6fd25d)

---

## 🔎 Explanation of the Vulnerability
The challenge relies on **insecure cookie storage**. Instead of protecting the recipe, the server simply encodes it (URL + Base64). Encoding only hides the data, but anyone with access to the cookie can decode it easily. This is a common mistake in web applications where developers confuse encoding with encryption.

---

## 📝 Lesson Learned
- **Encoding ≠ Security.** Encoded data can always be decoded back.
- Sensitive data should never be stored client-side in plain or encoded form.
- Use proper encryption and server-side validation to protect secrets.

---

## 🎯 Flag
```
picoCTF{c00k1e_m0nster_l0ves_c00kies_C430AE20}
```

🍪 Mission accomplished — Cookie Monster’s secret recipe is ours! 😋
