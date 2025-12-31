Scavenger Hunt

<img width="627" height="380" alt="image" src="https://github.com/user-attachments/assets/fe2a19dd-abf5-489a-90f2-159d8010fd93" />

We were given a website , so the first thing is to inspect the html 

<img width="1919" height="549" alt="image" src="https://github.com/user-attachments/assets/f0ed6169-b295-43c0-96ba-9d24259bc014" />

We can see the first part of the flag in the comment of the html 

<img width="611" height="144" alt="image" src="https://github.com/user-attachments/assets/32e21a97-737a-495b-989b-e714a1e636db" />

First part of the flag 
```
picoCTF{t
```
now lets look at the sources , there are 3 file which is index(that is weird looking usaly have and extension at the end) , myjs.js and mycss.css
<img width="282" height="154" alt="image" src="https://github.com/user-attachments/assets/39fefa95-62a9-45c5-9760-f150500cef21" />

so in the mycss.css we can see the second part of the flag  commented in the css

<img width="897" height="173" alt="image" src="https://github.com/user-attachments/assets/96b0f9e3-2e62-41ae-b888-d36551c54199" />

Second part of the flag
```
h4ts_4_l0
```
looking at myjs.js there is a comment questioning How can I keep Google from indexing my website?

<img width="495" height="156" alt="image" src="https://github.com/user-attachments/assets/06b7f525-872e-46d6-8280-b3eb9be04177" />


so i searched google and it says:
you should use the noindex meta tag <meta name="robots" content="noindex"> in the <head> section of your HTML, or use a noindex HTTP header. For complete blocking, you can create a robots.txt file in your root directory with User-agent: *\nDisallow: / to prevent all crawlers from accessing your site. Remember that a page must be accessible to be crawled for the noindex tag to be seen, so use this directive in conjunction with a properly accessible page. 

so , in the url of the website , we add /robots.txt and walla we found the third flag 

<img width="650" height="170" alt="image" src="https://github.com/user-attachments/assets/010387f4-ddd8-4f7d-9ef1-383752aa61e8" />

Third part of the flag
```
t_0f_pl4c
```

and theres another hint : I think this is an apache server... can you Access the next flag?
So basically the capital A is a wink at .htaccess (HyperText Access)

A .htaccess file is a configuration file used on Apache-based web servers to manage website settings on a per-directory basis, without needing access to the main server configuration file

and boom the forth flag 

<img width="725" height="131" alt="image" src="https://github.com/user-attachments/assets/4c971c57-c1d4-4645-9a1e-d149bbe2f070" />

Forth part of the flag
```
3s_2_lO0k
```
and again a hint (no woories this is the last) so basically In CTFs, when authors mention Mac and Store, it usually means they left a .DS_Store file in the webroot or a subdirectory. These can leak filenames, directory structure
.DS_Store is a hidden file created by macOS's Finder application to store custom attributes and metadata for a folder.

<img width="608" height="120" alt="image" src="https://github.com/user-attachments/assets/746d5a7f-7f8c-43c6-b749-dc3b0888d120" />

Forth part of the flag
```
_fa04427c}
```

Flag = picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_fa04427c}


