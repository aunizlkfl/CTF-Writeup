# NHAT1
<img width="801" height="289" alt="image" src="https://github.com/user-attachments/assets/4731ec05-eaf3-459b-a930-5ca5d92c552c" />

## Challenge
Investigate the provided artifact and recover the flag.

## Solution
1. Extract `NHAT.7z`.
2. Open the extracted folder in **FTK Imager**:  
   **File → Add Evidence Item → Contents of a Folder** → select the `NHAT` directory.
3. Browse to:
`NHAT\DESKTOP-GBETLNV\C\Users\zach\AppData\Local\Google\Chrome\User Data\Default`
You’ll see the Chrome SQLite database named **History**.
<img width="642" height="287" alt="image" src="https://github.com/user-attachments/assets/332be2c0-37b0-4ca8-8e0a-dce5c952bb51" />

4. View the database (any SQLite viewer works). Examples:
- **Web viewer**: https://sqliteviewer.app/ (quick check).
- **Offline** (recommended): DB Browser for SQLite, or CLI:

  ```
  sqlite3 "History" "SELECT url, title, visit_count, last_visit_time FROM urls ORDER BY last_visit_time DESC LIMIT 100;"
  ```

> Notes: Using an offline viewer keeps evidence local. If needed, export the `urls` table to CSV for easier searching.

5. Inspect the visited URLs; the flag appears in one of the entries.

## Flag
```flag
gctf{w4Rm!ng_Up_WItH_BROw$3r_HIsToRY}
```
<img width="1268" height="631" alt="image" src="https://github.com/user-attachments/assets/04acad63-6e10-4866-9441-56e1d8c05a0a" />


