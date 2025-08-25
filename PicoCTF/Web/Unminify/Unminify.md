# ✨ Unminify — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/0695c768-2086-4dfe-8bbc-984310de5760)

---

## 📌 Challenge Info
- **Category:** Web Exploitation  
- **Difficulty:** Easy  
- **Event:** picoCTF 2025  

**Description:**  
The challenge hinted that the flag was already on the website. We just needed to uncover it from the messy source code.

---

## 🛠️ Steps to Solve

### 1️⃣ Inspect the Page Source
We inspected the page (`CTRL + U`) and saw a **very long line of minified code**.  

📸 Screenshot:  
![Source Minified](https://github.com/user-attachments/assets/376b8d25-2941-4cad-a96d-cc87cd8b764b)

And yep… it looked like this monster:  

![Long Code](https://github.com/user-attachments/assets/a60d2a09-fff2-4cde-a481-e0dcdbfb1c7e)

---

### 2️⃣ Unminify the Code
Instead of trying to read that spaghetti mess, we copied the code and pasted it into **[Unminify.com](https://unminify.com/)**.  

This tool can **beautify** JavaScript, CSS, HTML, XML, and JSON into something readable.

📸 Screenshot:  
![Unminify Output](https://github.com/user-attachments/assets/d242438a-fad0-40ae-92a4-4e5a8ce9689f)

---

### 3️⃣ Find the Flag
Scrolling through the now-readable code revealed the flag 🎉.

---

## 🔎 Explanation of the Vulnerability
- The flag was hidden inside **minified JavaScript** in the HTML source.  
- Minification makes code harder to read, but it’s not real protection.  
- Using a beautifier/unminifier easily exposes any hidden strings, including secrets or flags.

---

## 📝 Lesson Learned
- **Minification ≠ Security.** It only reduces file size and readability, but the content is still there.  
- Always inspect and unminify suspiciously long lines of code when solving CTFs.  
- Tools like **Unminify.com** or browser extensions make this quick and easy.

---

## 🎯 Flag
```
picoCTF{pr3tty_c0d3_622b2c88}
```

✅ The messy code turned pretty, and the flag was revealed!
