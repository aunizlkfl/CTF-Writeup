# 🏛️ Local Authority — Writeup

<img width="562" height="373" alt="image" src="https://github.com/user-attachments/assets/f4a999f1-8103-4781-ab38-558558d02af7" />



---

## 📌 Challenge Info
- 🎯 **Category:** Web Exploitation  
- 🌱 **Difficulty:** Easy  
- 🏆 **Event:** picoCTF 2025  

---

## 🛠️ Steps to Solve

### 1️⃣ Initial Login Attempt
We were given a **login page** of a *Secure Customer Portal*.  
So let’s try a default username and password: **admin : admin**  

📸 Screenshot:  


---

### 2️⃣ Output from Wrong Credentials
The login attempt failed.  

📸 Screenshot:  


---

### 3️⃣ Inspecting the Code 🔍
By opening **Developer Tools → Network**, we saw several files being loaded:  
- `login.php`  
- `style.css`  
- `secure.js`  

So we inspected the source of each file. Inside **secure.js**, we found the following script:

```javascript
function checkPassword(username, password)
{
  if( username === 'admin' && password === 'strongPassword098765' )
  {
    return true;
  }
  else
  {
    return false;
  }
}
```
### 4️⃣ Discovering the Credentials 🔑
From the script, the correct login is clearly:  
- **Username:** `admin`  
- **Password:** `strongPassword098765`

---

### 5️⃣ Successful Login 🎉
We tried the credentials:  
- Username: **admin**  
- Password: **strongPassword098765**  

📸 Screenshot:  


And we successfully logged in!

---

## 🎯 Flag
```text
picoCTF{j5_15_7r4n5p4r3n7_b0c2c9cb}
```
✨ Game over — flag captured ✨
