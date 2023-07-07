uri = "http://dev-03-app-01.chengdudev.edetekapps.cn/api/external/token"

```python
def external_token(data):  
    uri = "http://dev-03-app-01.chengdudev.edetekapps.cn/api/external/token"  
    headers = {"Content-Type": "application/json;charset=UTF-8"}  
    resp = requests.post(url=uri, json=data, headers=headers)  
    return resp.json().get("payload").get("token")

if __name__ == '__main__':  
    token = external_token(dict(appId="erasca", security="erasca"))
```
参数填写 appId , security 从 [[Client Management]] 设置获取