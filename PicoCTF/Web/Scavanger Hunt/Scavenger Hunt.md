Scavenger Hunt

We were given a website , so the first thing is to inspect the html 
<img width="549" height="43" alt="image" src="https://github.com/user-attachments/assets/82cb22f3-cd2d-4329-b993-c5ff20e52518" />

We can see the first part of the flag in the comment of the html 
<img width="479" height="42" alt="image" src="https://github.com/user-attachments/assets/5b65a705-9bbc-4f59-a7e1-ff9c4b55d606" />

First part of the flag 
```
picoCTF{t
```
now lets look at the sources , there are 3 file which is index(that is weird looking usaly have and extension at the end) , myjs.js and mycss.css



so in the mycss.css we can see the second part of the flag  commented in the css

 
Second part of the flag
```
h4ts_4_l0
```
looking at myjs.js there is a comment questioning How can I keep Google from indexing my website?

so i searched google and it says:
you should use the noindex meta tag <meta name="robots" content="noindex"> in the <head> section of your HTML, or use a noindex HTTP header. For complete blocking, you can create a robots.txt file in your root directory with User-agent: *\nDisallow: / to prevent all crawlers from accessing your site. Remember that a page must be accessible to be crawled for the noindex tag to be seen, so use this directive in conjunction with a properly accessible page. 

so , in the url of the website , we add /robots.txt and walla we found the third flag 
![Uploading image.png…]()
Third part of the flag
```
t_0f_pl4c
```

and theres another hint : I think this is an apache server... can you Access the next flag?
So basically the capital A is a wink at .htaccess (HyperText Access)

A .htaccess file is a configuration file used on Apache-based web servers to manage website settings on a per-directory basis, without needing access to the main server configuration file

and boom the forth flag 
![Uploading image.png…]()

Forth part of the flag
```
3s_2_lO0k
```
and again a hint (no woories this is the last) so basically In CTFs, when authors mention Mac and Store, it usually means they left a .DS_Store file in the webroot or a subdirectory. These can leak filenames, directory structure
.DS_Store is a hidden file created by macOS's Finder application to store custom attributes and metadata for a folder.

![Uploading image.png…]()

Forth part of the flag
```
_fa04427c}
```

Flag = picoCTF{th4ts_4_l0t_0f_pl4c3s_2_lO0k_fa04427c}


