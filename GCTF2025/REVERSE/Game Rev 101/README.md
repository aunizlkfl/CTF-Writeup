# Game Rev 102

>only include the file i used for this challenge cause it's kinda big to upload here

<img width="807" height="275" alt="Screenshot 2025-09-14 183900" src="https://github.com/user-attachments/assets/181b59b3-be7b-43d5-b512-5bceba169cc1" />

## Challenge

We were given a Unity-based game build with the hint:  
*“I can’t reach the …. Uhukss”*  

I first tried playing the game normally, but there was no clear way to reach the flag.  
Next, I inspected the game folder(gameRev102_Data) with [AssetRipper](https://github.com/AssetRipper/AssetRipper).

---

## Solution
After loading the game into AssetRipper, I could see the overall project structure:  

There we can see the overall game structure 

<img width="505" height="693" alt="Screenshot 2025-09-14 191704" src="https://github.com/user-attachments/assets/73a1af97-ef7d-46e3-8dec-b0e5ffa9481b" />
 
Unity games store each scene as `levelX`.  
- `level0` → mapped to the main game scene.

  <img width="672" height="722" alt="image" src="https://github.com/user-attachments/assets/6a65d9f5-6b19-4bb3-9870-95bc69b831ca" />

- `level1` → mappedd to scene Win

  <img width="660" height="763" alt="image" src="https://github.com/user-attachments/assets/e591219e-8101-4c2f-bca5-aceb1cd77d10" />


So I opened **level1** file in AssetRipper.  
Inside the **Sprite Data Storage**, I found a sprite named **goal**. This immediately stood out as the place where the flag could be hidden.  

<img width="1060" height="343" alt="Screenshot 2025-09-14 195711" src="https://github.com/user-attachments/assets/1926ba68-82a3-420a-b095-0fa9565d4e49" />

Opening the sprite in the **Image tab** revealed the flag text embedded directly inside the asset.

<img width="1130" height="314" alt="Screenshot 2025-09-14 195854" src="https://github.com/user-attachments/assets/07f26e55-4f0f-4b4f-a596-222d64a9db05" />

---

## Flag

```
GCTF25{uhmm_why_h4ck_G4m3_3141592654}
```
