# The Shopkeeper's Math Error
<img width="800" height="381" alt="image" src="https://github.com/user-attachments/assets/f159b6b0-9267-4406-9401-9c8ef4091dbc" />

Challenge
You are given a POS-style shop binary and a remote service. It sells:
- Health Potion (price = 50)
- Magic Sword (price = 10,000)

Some message hints mention an “integer overflow.” The program reads flag.txt when a special condition is met.

## Solution
1. Recon the binary
   ```
   strings shopkeeper | grep -i flag
   ```
   You will see flag.txt, confirming a flag-read path exists.
   
3. Understand the bug
   The cost is computed with a 32-bit signed multiply:
   ```
   total_cost = price * quantity
   ```
   If the product exceeds INT_MAX (2,147,483,647), it wraps to a negative number, which the program treats as “weird math” and then prints the flag.

3. Pick the item that overflows easily
   - Magic Sword price = 10,000
   - Smallest overflowing quantity:
    ```
    q = floor(2,147,483,647 / 10,000) + 1 = 214,749
    ```
4. Exploit via netcat (replace IP:PORT if your instance differs)
   - Linux:
     ```
     printf "2\n214749\n" | nc 188.166.183.187 34688
     ```
     Input :
     - 2 → choose Magic Sword
     - 214749 → forces overflow
     
       <img width="680" height="574" alt="image" src="https://github.com/user-attachments/assets/c9b1c99e-7249-44cc-9d99-3822e6bfdcbc" />

## Flag

```flag
GCTF25{INT3GER_0V3RFLOW_M4G1C_82f973387fc1}
```
