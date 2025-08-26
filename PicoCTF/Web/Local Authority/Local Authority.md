Local Authority
![Uploading image.png…]()

We were given a login page of a Secure Customer Portal. So lets try a default username and password which is admin:admin
![Uploading image.png…]()

so this is the output 
![Uploading image.png…]()

let look inspect and look at the Network , we can see there is a few file which is login.php, style.css and secure.js
so i tried opening the source of each of the file and found in secure.js there is a script 
```
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

now we know the password!! lets try it username :admin password : strongPassword098765

and there you go we got the flag !
##Flag 
```strongPassword098765

```

