# 🌐 WebDecode — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/b7fe4608-83a2-4857-91d2-7d1dbd7b9efb)

---

## 📌 Challenge Info
- **Category:** Web Exploitation  
- **Difficulty:** Easy  
- **Event:** picoCTF 2025  

**Description:**  
We were given a simple multi-page website and needed to dig through the pages to uncover the hidden flag.

---

## 🛠️ Steps to Solve

### 1️⃣ Inspect the Source
First step → Inspect the webpage (`CTRL + U`).  

📸 Screenshot:  
![Source Inspection](https://github.com/user-attachments/assets/5dde374a-46fd-44b1-bece-17f68037aebc)

From the code, we noticed the site has **3 HTML pages**:
- `index.html`  
- `about.html`  
- `contact.html`  

---

### 2️⃣ Explore the Pages
The main `index.html` didn’t show anything useful.  
But navigating to **about.html**, we found something interesting inside a `section class="about"`:  

```html
<section class="about" notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfZjZmNmI3OGF9">
  <h1>Try inspecting the page!! You might find it there</h1>
</section>
```

Here, the attribute `notify_true` looked like **encoded text**.

---

### 3️⃣ Decode the Hidden Value
When unsure about encoded text → throw it into **CyberChef** and let the **Magic** function do its work ✨.  

📸 Screenshot:  
![CyberChef Decode](https://github.com/user-attachments/assets/aa0db2d9-2d71-4826-b53a-99deee33da7a)

After decoding, we got the flag 🎉.

---

## 🔎 Explanation of the Vulnerability
- Developers sometimes hide information inside **HTML attributes** or comments.  
- Even if it looks obfuscated, it’s often just **Base64** or simple encoding.  
- This shows why **viewing source code** is one of the first steps in web challenges.

---

## 📝 Lesson Learned
- Always inspect page source and hidden attributes.  
- Don’t trust that “hidden” = “secure.”  
- Tools like **CyberChef** are your best friend for quick decoding.

---

## 🎯 Flag
```
picoCTF{web_succ3ssfully_d3c0ded_f6f6b78a}
```

✅ Easy win — decoding success!
