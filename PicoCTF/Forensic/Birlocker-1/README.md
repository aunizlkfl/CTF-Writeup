# Bitlocker-1

<img width="618" height="403" alt="image" src="https://github.com/user-attachments/assets/3726a39c-a661-48d4-9346-231acffdcd3e" />

# 🔐 Forensics – BitLocker 1 (picoCTF)

> **Category:** Forensics  
> **Goal:** Recover the BitLocker volume password and grab the flag from the encrypted disk image.

---

## 🧩 Challenge Files

- `bitlocker-1.dd` – raw BitLocker-encrypted disk image  
---

## ⚙️ Tools Used

- `bitlocker2john` – extract BitLocker hash from the disk image  
- `hashcat` – crack the BitLocker password  
- `rockyou.txt` – dictionary for cracking  
- `dislocker` – decrypt the BitLocker volume  
- `mount` – to attach decrypted filesystem  

---
## 1️⃣ Extract the BitLocker Hash (bitlocker2john)

Before cracking, we must convert the BitLocker disk image into a hash format Hashcat can understand.

```bash
bitlocker2john -i bitlocker-1.dd > bitlocker.txt
```
Why this step?
bitlocker2john parses the BitLocker metadata inside the .dd image and outputs it into a standardized hash format.
This hash is what Hashcat will attempt to crack.

## 2️⃣ Prepare the Wordlist
```bash
head -n 10000 /usr/share/wordlists/rockyou.txt > dictionary.txt
```
### Why?
Rockyou is huge. Taking only the first 10,000 entries makes testing faster and is often enough for CTF-style weak passwords.

### 3️⃣ Crack the BitLocker Password with Hashcat
```bash
hashcat -m 22100 -a 0 bitlocker-1.txt dictionary.txt -w 3
```
What this means:
- ` - m 22100` → Hash mode 22100 = BitLocker (user password / recovery password).
- `-a 0` → Straight dictionary attack (take each word from dictionary.txt and try it as-is).
- `bitlocker-1.txt` → File containing the BitLocker hash/metadata.
- `dictionary.txt` → Our custom mini wordlist.
- `-w 3` → Workload profile 3 (higher performance, uses more system resources).

After some time, Hashcat finds:
<img width="588" height="428" alt="image" src="https://github.com/user-attachments/assets/ba358904-54c3-4f7c-86a2-0d34eaf21550" />

```text
$bitlocker$0$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d:jacqueline
```
Recovered password
```text
jacqueline
```
### 4️⃣ Decrypt the Disk with Dislocker
Make an output directory
```bash
mkdir dislocker_output
```
Why?
Dislocker will generate a decrypted virtual disk file.
Keeping it inside its own folder keeps things tidy and prevents clutter.

Run dislocker
```bash
sudo dislocker bitlocker-1.dd -u jacqueline dislocker_output
```
What happens?
Dislocker produces:
```bash
dislocker_output/dislocker-file
```
This is not yet mounted — it’s just a decrypted NTFS container.

### 5️⃣ Mount the Decrypted NTFS Volume
Create a mountpoint
```bash
mkdir flag_here
```
Why?
Because Linux requires an empty directory as the location to “attach” filesystems.

Mount the decrypted disk
```bash
sudo mount -o loop dislocker_output/dislocker-file flag_here
```
<img width="568" height="152" alt="image" src="https://github.com/user-attachments/assets/87da1fb9-b856-4e5b-bebb-35b907b8d216" />

Even if NTFS is marked “unclean”, read-only access is enough for us.

### 6️⃣ Retrieve the Flag
List the files:
```bash
ls -lha flag_here
```
<img width="565" height="142" alt="image" src="https://github.com/user-attachments/assets/9bc9d4e8-c048-4a18-ace8-5d7b937edd7c" />

Read the flag:
```bash
cat flag_here/flag.txt
```
<img width="368" height="57" alt="image" src="https://github.com/user-attachments/assets/deae132e-b6f3-4f58-9074-f24b2560532c" />

Flag:
```bash
picoCTF{us3_b3tt3r_p4ssw0rd5_pl5!_3242adb1}
```

