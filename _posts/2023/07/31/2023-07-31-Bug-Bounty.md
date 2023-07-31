---
date: 2023-07-31 22:14:57 +02:00
categories: [SecurityValley, Web]
tags: [Web, CTF, SecurityValley]
---
This is a writeup of the web challenge The Ticketsystem from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 2, Score: 30
# Premise
Pwnme.org has released a bug bountry program. Let's go on the hunt. All your skills are needed here! Show us that you are a good bug hunter. If you get stuck, you can pick up a hint...(look in the footer how to reach us - or can't you do that either?). At least the username for the portal was leaked. Use "h4x0r" as username. The password should not be hard to bruteforce. Because it was somehow difficult for many, here the password: 'genar53'

**Link:** [https://portal.pwnme.org](https://portal.pwnme.org/)
## Challenge links:

**Link:** [https://portal.pwnme.org](https://portal.pwnme.org/)

# Observations
When visiting the page, we're greeted with a login page, we login with the credentials we're supplied in the challenge description.
Once we're logged in, we're greeted with a flag consisting of Xes.
![page after login](/assets/images/SecVal/bugbount.png)

This doesnt help us much, so we investigate further.
Looking in our session storage, we find that we have a JWT token named access_token:
![JWT token](/assets/images/SecVal/bugbount_access_token.png)

So we head over to [jwt.io](https://jwt.io/) and decode it, and get the following:

![decoded JWT token](/assets/images/SecVal/bugbount_admin_page_in_code.png)

Looking for more usefull leads we look in the source files of the web page, more specifically: `main.7296ba666008455f.js` and search for strings such as admin or flag.

When searching admin, we get some interesting results, there appears to be  another realm at the URL [https://admin.pwnme.org](https://admin.pwnme.org).
Navigating there brings us to an admin login panel.
![admin login panel](/assets/images/SecVal/bugbount_admin_login_pannel.png)

# Solution
When we log in to the user web page, we can intercept the POST request sent to authenticate our user, and investigate it.
```
POST /api/v1/authenticate HTTP/1.1
Host: portal.pwnme.org
Content-Length: 76
Sec-Ch-Ua: 
Accept: application/json, text/plain, */*
Content-Type: application/json
Sec-Ch-Ua-Mobile: ?0
Authorization: Bearer null
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.5735.199 Safari/537.36
Sec-Ch-Ua-Platform: ""
Origin: https://portal.pwnme.org
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://portal.pwnme.org/login
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Connection: close

{"username":"h4x0r","password":"genar53","realm":"https://portal.pwnme.org"}
```

The last cookie here, realm, is of interest to us. If we send this to our burp repeater we can make an interesting change.

Thanks to our previous investigations, we know of the existance of the link `https://admin.pwnme.org`, so we can change this token to that url. This gives us the following result: 

![admin post request](/assets/images/SecVal/bugbount_admin_POST.png)

Decoding this token, we can see its contents:

![admin login panel](/assets/images/SecVal/bugbount_admin_JWT.png)

Which means we now have a JWT token connected to the admin page.

If we now forward the request we previously intercepted, that of the user to log into the user page, we can see an incomming request in burp.

This request is to `/api/v1/dashboard`. If we pass this onto repeater and forward it, we get the following results:
![user no dashboard](/assets/images/SecVal/bugbount_dashboard_user.png)

If we take that request and modify it a little, by exchanging the Authorization token to our previously generated admin JWT token, and change the Referer from `https://portal.pwnme.org/dashboard` to `https://admin.pwnme.org/dashboard`, we get the following result:

![flag](/assets/images/SecVal/bugbount_dashboard_admin.png)

And now, we have our flag.

# Tools and sources used:
 - [jwt.io](https://jwt.io/)
 - [Burp Suite (Community)](https://portswigger.net/burp/communitydownload)
 - [h4x0rs writeup of a simmilar problem](https://medium.com/@h4x0r_dz/23000-for-authentication-bypass-file-upload-arbitrary-file-overwrite-2578b730a5f8)
