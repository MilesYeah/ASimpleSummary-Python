# Session 会话对象

会话对象让你能够跨请求保持某些参数。它也会在同一个 Session 实例发出的所有请求之间保持 cookie， 期间使用 urllib3 的 connection pooling 功能。
所以如果你向同一主机发送多个请求，底层的 TCP 连接将会被重用，从而带来显著的性能提升。 (参见 HTTP persistent connection).

我们来跨请求保持一些 cookie:
```py
s = requests.Session()

s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
r = s.get("http://httpbin.org/cookies")

print(r.text)
# '{"cookies": {"sessioncookie": "123456789"}}'
```


## 为请求方法提供缺省数据
会话也可用来为请求方法提供缺省数据。这是通过为会话对象的属性提供数据来实现的：
```py
s = requests.Session()
s.auth = ('user', 'pass')
s.headers.update({'x-test': 'true'})

# both 'x-test' and 'x-test2' are sent
s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})
```

任何你传递给请求方法的字典都会与已设置会话层数据合并。方法层的参数覆盖会话的参数。

不过需要注意，就算使用了会话，方法级别的参数也不会被跨请求保持。


下面的例子只会和第一个请求发送 cookie ，而非第二个：
```py
s = requests.Session()

r = s.get('http://httpbin.org/cookies', cookies={'from-my': 'browser'})
print(r.text)
# '{"cookies": {"from-my": "browser"}}'

r = s.get('http://httpbin.org/cookies')
print(r.text)
# '{"cookies": {}}'
```

如果你要手动为会话添加 cookie，就使用 Cookie utility 函数 来操纵 Session.cookies。


## 会话还可以用作前后文管理器：
```py
with requests.Session() as s:
    s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
```
这样就能确保 with 区块退出后会话能被关闭，即使发生了异常也一样。




## session 自动记录 cookie
如下两段代码的作用是一样的。

1. 采用 session 自动保存 cookie，后续的请求再不需要填写 cookie。
    ```py
    import requests

    data = {
        'formhash': 'c42aafbf',
        "cookietime":'2592000',
        'loginfield':'username',
        'username':'MilesYeah',
        'password':'free123456'
    }

    s = requests.session()
    r = s.post(url='https://bbs.bccn.net/logging.php?action=login&loginsubmit=true', data=data)
    print(r)

    r1 = s.get("https://bbs.bccn.net/memcp.php")
    r2 = s.get("https://bbs.bccn.net/memcp.php?action=profile&typeid=2")

    pass
    ```


2. 每次请求都显式使用 cookie 才能访问
    ```py
    import requests

    data = {
        'formhash': 'c42aafbf',
        "cookietime":'2592000',
        'loginfield':'username',
        'username':'MilesYeah',
        'password':'free123456'
    }

    r = requests.post(url='https://bbs.bccn.net/logging.php?action=login&loginsubmit=true', data=data)
    print(r)

    r1 = requests.get("https://bbs.bccn.net/memcp.php") # 提示登录后再获取用户详情页
    r2 = requests.get("https://bbs.bccn.net/memcp.php?action=profile&typeid=2") # 提示登录后再获取用户个人信息页

    r3 = requests.get("https://bbs.bccn.net/memcp.php", cookies=r.cookies) # 成功获取用户详情页
    r4 = requests.get("https://bbs.bccn.net/memcp.php?action=profile&typeid=2", cookies=r.cookies) # 成功获取用户个人信息页

    pass
    ```












