# Go kit

[TOC]

## Tips

1. Any component that needs to log should treat the logger like a dependency, same as a database connection. So, we construct our logger in our `func main`, and pass it to components that need it. We never use a globally-scoped logger.

##  三层结构

Go kit 分为

1. transport layer（传输层）

2. endpoint layer（端点层）

3. service layer（服务层）

**请求**在第1层进入服务，然后向下流到第3层，而**响应**则采用相反的过程。

## 需要补充的知识点

1. json包
2. log包