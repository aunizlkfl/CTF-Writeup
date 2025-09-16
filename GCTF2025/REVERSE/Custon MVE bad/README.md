# Fan Translator

<img width="833" height="279" alt="image" src="https://github.com/user-attachments/assets/2ad4bd22-e80c-4acc-9843-43eb816fed2b" />

<img width="688" height="533" alt="image" src="https://github.com/user-attachments/assets/0b82cbf6-d65f-45a5-9ce5-c1fddd760bf8" />

## Challenge

A tiny RPG Maker maze game. We do not need to finish the maze; the flag is somewhere in the game data. Because it’s MV, all maps and event dialogs live in www/data/Map*.json.

## Solution

Unpack nw_100_percent.zip and looked in www/data.

In www/data/Map001.json, event ID 18 at (x=26, y=11) shows a text line that literally contains the flag

<img width="1675" height="778" alt="image" src="https://github.com/user-attachments/assets/6fb5e6d4-2b62-460e-aa28-c6978dae4aef" />

## Flag
```
GCTF25{7r4n5l470r_w0rk_h4rd}
```
