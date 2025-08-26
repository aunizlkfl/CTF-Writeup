# 🍪 Cookies — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/c1e00c12-1014-41c9-a336-f52e8d437c15)

---

## 📌 Challenge Info
- 🎯 **Category:** Web Exploitation  
- 🌱 **Difficulty:** Easy  
- 🏆 **Event:** picoCTF 2025  

**📝 Description:**  
We’re given a website to “search about cookies.” Seems like a hint to look deeper into **browser cookies** 🍪  

---

## 🛠️ Steps to Solve

### 1️⃣ Searching for Cookies 🔍
On the page, we tried searching for `snickerdoodle` cookies.  

📸 Screenshot:  
![Search](https://github.com/user-attachments/assets/cf25dc65-98f9-4542-864b-28d16f05fb0a)

The site responded with cookie info:  

📸 Screenshot:  
![Output](https://github.com/user-attachments/assets/1893866f-38fd-4402-a6e4-acb73f06e939)

---

### 2️⃣ Inspecting the Cookies 🍪
Since the challenge name is *Cookies*, we checked the **Application → Cookies** tab in Developer Tools.  

📸 Screenshot:  
![Cookies Inspect](https://github.com/user-attachments/assets/88057e4d-a991-4b03-814e-dc7b8b3841af)

We noticed a value in the cookie named `name` that could be modified.  

---

### 3️⃣ Automating with Burp Intruder ⚡
Instead of changing values manually, we used **Burp Suite Intruder**.  

📸 Screenshot:  
![Intruder Setup](https://github.com/user-attachments/assets/528a4c10-9bc7-47c6-b54d-9ec08d4dc7c1)

- Payload type: **Numbers**  
- Range: **0 → 30**  

📸 Screenshot:  
![Payload Config](https://github.com/user-attachments/assets/f27ac899-597b-4eb0-934e-d8102425f0a3)

Then we started the attack 🚀

---

### 4️⃣ Analyzing Results 📊
After the Intruder attack finished, we compared the **response lengths**.  

📸 Screenshot:  
![Response Lengths](https://github.com/user-attachments/assets/e968cb51-1b89-4cbc-9ba0-769c3af814b5)

- Payload **29** and **30** → invalid, no real content.  
- Payload **18** → different response size. Let’s check it!  

📸 Screenshot:  
![Invalid Payloads](https://github.com/user-attachments/assets/50efde09-5325-4868-8f9b-7afb1b305b10)

---

### 5️⃣ Capturing the Flag 🎉
Inspecting payload **18**, we finally saw the flag!  

📸 Screenshot:  
![Flag Found](https://github.com/user-attachments/assets/d192c3c8-733b-4f28-b2c6-ad23cb640810)

---

## 🎯 Flag
```text
picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}
```
✨ Game over — cookies served ✨
## 🔎 What Went Wrong
- ❌ Cookies were used insecurely to control access/data.  
- ❌ No proper validation on cookie values.  
- ❌ Allowed brute-forcing with automated tools like Burp Intruder.  

---

## 📝 Lessons Learned
- ✅ Never rely solely on cookies for sensitive data.  
- ✅ Always validate cookie values server-side.  
- ✅ Use secure, signed, or encrypted cookies to prevent tampering.  
