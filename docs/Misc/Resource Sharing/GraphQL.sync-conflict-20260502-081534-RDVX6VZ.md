GraphQL is an API query language that is designed to facilitate efficient communication between clients and servers. It enables the user to specify exactly what data they want in the response, helping to avoid the large response objects and multiple calls that can sometimes be seen with REST APIs.

GraphQL services define a contract through which a client can communicate with a server. The client doesn't need to know where the data resides. Instead, clients send queries to a GraphQL server, which fetches data from the relevant places. As GraphQL is platform-agnostic, it can be implemented with a wide range of programming languages and can be used to communicate with virtually any data store.

## 1. Principle

目标 GraphQL API 限制可绕过

### 1.1. Location

调用了 GraphQL API 的位置

```
/graphql
/api
/api/graphql
/graphql/api
/graphql/graphql
/graphql/v1
/api/v1
/api/graphql/v1
/graphql/api/v1
/graphql/graphql/v1
```

## 2. Test



---

**References**

- [GraphQL API vulnerabilities](https://portswigger.net/web-security/graphql)
- 