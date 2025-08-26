# 🧩 IntroToBurp — Writeup

![Challenge Banner](https://github.com/user-attachments/assets/a3daad1c-f931-4b71-98ed-e83b56fac2e7)

---

## 📌 Challenge Info
- 🎯 **Category:** Web Exploitation  
- 🌱 **Difficulty:** Easy  
- 🏆 **Event:** picoCTF 2025  

**📝 Description:**  
The challenge name hints at **Burp Suite** 🕵️. Let’s fire it up and intercept some requests to uncover the flag.  

---

## 🛠️ Steps to Solve

### 1️⃣ Set Up Burp Suite 🔧
After launching the challenge instance, we opened **Burp Suite → Proxy → Intercept** and used the built-in browser to paste the challenge link.  

📸 Screenshot:  
<img width="358" height="281" alt="image" src="https://github.com/user-attachments/assets/1741e1e9-9c45-4e75-b355-8c0c1f3b6457" />

---

### 2️⃣ Register & Analyze OTP Input 🔍
We tried registering with random details. At the OTP step, we entered a random number to see how the server reacted.  

📸 Screenshot:  
![Random OTP](https://github.com/user-attachments/assets/3469e45a-f532-483c-a604-81fd806a1985)

From here, we began analyzing **HTTP requests and responses** through Burp’s HTTP history.

---

### 3️⃣ Explore Directories 📂
We noticed two main directories: `/` and `/dashboard`.  

📸 Screenshot:  
![Directories](https://github.com/user-attachments/assets/432f529c-b8ca-49ea-93d3-ad0790d619b9)

The request/response flow showed a redirect from `/` to `/dashboard`.  

📸 Screenshot:  
<img width="1533" height="460" alt="image" src="https://github.com/user-attachments/assets/989d1f34-9888-45df-aec3-44b4417e2279" />

And then the `/dashboard` endpoint asked for OTP input.  

📸 Screenshot:  
![Dashboard OTP](https://github.com/user-attachments/assets/133faabe-d730-4637-bfb4-e2c9776feeaf)

---

### 4️⃣ Invalid OTP Behavior ❌
We tested with random numbers at the OTP prompt. The server simply returned **“Invalid OTP.”**  

📸 Screenshot:  
![Invalid OTP](https://github.com/user-attachments/assets/f9748d94-0b63-47d5-8a09-4365d52c4d75)

Even when sending alphabet characters, the server’s response was the same. This hinted that the backend wasn’t performing strong validation — just checking if the field existed.  

---

### 5️⃣ Bypass with Malformed Request 🌀
The challenge hint said:  
> "Try mangling the request, maybe their server-side code doesn't handle malformed requests very well."  

So we removed the OTP parameter entirely (`otp=`).  
If the server only validates when the field is present… then skipping it might bypass the check completely.  

And guess what? 🎉 That worked!  

📸 Screenshot:  
![Flag Capture](https://github.com/user-attachments/assets/8e4bf614-ac0d-4732-8602-bd9f06f85f6f)

---

## 🎯 Flag
picoCTF{#0TP_Bypvss_SuCc3$S_9090d63c}


✨ Game over — OTP bypass successful ✨  

---

## 🔎 What Went Wrong
- ❌ OTP field not properly validated on the server.  
- ❌ Application trusted **presence** of the field instead of validating input.  
- ❌ Malformed request (missing parameter) completely bypassed security.  

---

## 📝 Lessons Learned
- ✅ Always enforce **server-side validation** — never trust client input.  
- ✅ OTPs should have **format + existence checks** on backend logic.  
- ✅ Security through "field presence" is weak and bypassable.  

---

