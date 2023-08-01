
---

# Google [![img](https://camo.githubusercontent.com/adb54264fe2ad5067d07d0752fc32600b4e6250073b01ce8c386575b431e3f06/68747470733a2f2f7777772e677374617469632e636f6d2f6c616d64612f696d616765732f66617669636f6e5f76315f31353031363063646466663766323934636533302e737667)](https://bard.google.com/) Bard API

[![img](https://github.com/dsdanielpark/Bard-API/raw/main/assets/bard_api.gif)](https://github.com/dsdanielpark/Bard-API/blob/main/assets/bard_api.gif)

If you will not provide the language parameter (use `english`, `korean`, `japanese` only as input text):

```Python
pip install bardapi

```

If you wish to use the Bard API, including various features:

```python
pip install git+https://github.com/dsdanielpark/Bard-API.git
```

## Authentication

> Warning Do not expose the `__Secure-1PSID`

1. Visit <https://bard.google.com/>
2. F12 for console
3. Session: Application → Cookies → Copy the value of  `__Secure-1PSID` cookie.

Note that while I referred to `__Secure-1PSID` value as an API key for convenience, it is not an officially provided API key. Cookie value subject to frequent changes. Verify the value again if an error occurs. Most errors occur when an invalid cookie value is entered.

If you need to set multiple Cookie values

- [Bard Cookies](https://github.com/dsdanielpark/Bard-API/blob/main/README_DEV.md#bard-which-can-get-cookies) - After confirming that multiple cookie values are required to receive responses reliably in certain countries, I will deploy it for testing purposes. Please debug and create a pull request



## Usage

[![Open In Colab](https://camo.githubusercontent.com/84f0493939e0c4de4e6dbe113251b4bfb5353e57134ffd9fcab6b8714514d4d1/68747470733a2f2f636f6c61622e72657365617263682e676f6f676c652e636f6d2f6173736574732f636f6c61622d62616467652e737667)](https://colab.research.google.com/drive/1zzzlTIh0kt2MdjLzvXRby1rWbHzmog8t?usp=sharing)

Simple Usage

```
from bardapi import Bard

token = 'xxxxxxx'
bard = Bard(token=token)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

Or you can use this

```
from bardapi import Bard
import os
os.environ['_BARD_API_KEY']="xxxxxxx"

Bard().get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

To get reponse dictionary

```
import bardapi
import os

# set your __Secure-1PSID value to key
token = 'xxxxxxx'

# set your input text
input_text = "나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘"

# Send an API request and get a response.
response = bardapi.core.Bard(token).get_answer(input_text)
```

Addressing errors caused by delayed responses in environments like Google Colab and containers. If an error occurs despite following the proper procedure, utilize the timeout argument.

```
from bardapi import Bard
import os
os.environ['_BARD_API_KEY']="xxxxxxx"

bard = Bard(timeout=30) # Set timeout in seconds
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

## Further

### Behind a proxy

If you are working behind a proxy, use the following.

```
from bardapi import Bard
import os

# Change 'http://proxy.example.com:8080' to your http proxy
# timeout in seconds
proxies = {
    'http': 'http://proxy.example.com:8080',
    'https': 'https://proxy.example.com:8080'
}

bard = Bard(token='xxxxxxx', proxies=proxies, timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

### Reusable session object

You can continue the conversation using a reusable session.

```
from bardapi import Bard
import os
import requests
os.environ['_BARD_API_KEY'] = 'xxxxxxx'
# token='xxxxxxx'

session = requests.Session()
session.headers = {
            "Host": "bard.google.com",
            "X-Same-Domain": "1",
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36",
            "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8",
            "Origin": "https://bard.google.com",
            "Referer": "https://bard.google.com/",
        }
session.cookies.set("__Secure-1PSID", os.getenv("_BARD_API_KEY")) 
# session.cookies.set("__Secure-1PSID", token) 

bard = Bard(token=token, session=session, timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']

# Continued conversation without set new session
bard.get_answer("What is my last prompt??")['content']
```



Transship from https://github.com/dsdanielpark/Bard-API

Found it when viewing https://twitter.com/nevrekaraishwa2/