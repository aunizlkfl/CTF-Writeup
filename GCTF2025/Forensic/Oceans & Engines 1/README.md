
# Oceans & Engines 1
<img width="805" height="320" alt="image" src="https://github.com/user-attachments/assets/a7d784fd-52da-47fb-a044-01cf9d6f2940" />

## Challenge
A password-protected ZIP contains a forensic image. Recover the flag from the user’s files inside the image.

## Solution

Open the zip password file with the password given: `GGctf25#@!`

Open the extracted forensic image in **FTK Imager** (File → Add Evidence Item → Image File). The flag would be in `C:\Users\ahmad\Download\TechnicalChallengeDetail.txt`

<img width="884" height="768" alt="image" src="https://github.com/user-attachments/assets/33f75c28-a500-4102-ab69-3def6e5fdda3" />

Copy and paste the encoded text to a Base 64 decoder and you got the flag !

<img width="628" height="630" alt="image" src="https://github.com/user-attachments/assets/ed959725-3c0b-4240-83ff-d14677ea0fe6" />

## FLag 
```Flag
GCTF25{winrawr-cve-kinda-c00l}
```
