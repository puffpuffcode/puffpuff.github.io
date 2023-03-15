---
title: Gin Web-参数绑定篇
date: 2023-01-11 17:49:30
tags: [Gin, Golang]
categories: Web
post_link:
description:
header:
mathjax:
copyright:
sticky:
reward_setting:
---
<!-- more -->
# 使用 Gin 框架绑定参数的正确姿势

> 本文参考自 [Gin 官方文档](https://gin-gonic.com/zh-cn/docs/)。

## Multipart/Urlencoded 绑定
Gin 框架中提供了模型绑定，将表单数据和模型进行绑定，方便参数校验和使用。

`c.ShouldBind(obj any)` 根据请求头中的 `Content-Type` 选择合适的方法绑定结构体。
```go
// shouldbind demo1
type LoginForm struct {
	User     string `form:"user" binding:"required"`
	Password string `form:"password" binding:"required"`
}

func main() {
	router := gin.Default()
	router.POST("/login", func(c *gin.Context) {
		// 你可以使用显式绑定声明绑定 multipart form：
		// c.ShouldBindWith(&form, binding.Form)
		var form LoginForm
		// 在这种情况下，将自动选择合适的绑定
		if c.ShouldBind(&form) == nil {
			if form.User == "user" && form.Password == "password" {
				c.JSON(200, gin.H{"status": "you are logged in"})
			} else {
				c.JSON(401, gin.H{"status": "unauthorized"})
			}
		}
	})
	router.Run(":8080")
}
```
`c.ShouldBind(obj any)` 与 `c.Bind(obj any)` 的区别？

后者在没有正确绑定时会返回 `400 error` 并且在响应头中设置 `Content-Type header "text/plain"` 并且 Abort。
<!--more-->
## Multipart/Urlencoded 表单
当参数比较少，获取表单数据时还可以这么做。
```go
func main() {
	router := gin.Default()

	router.POST("/form_post", func(c *gin.Context) {
		// 只获取表单中一个参数，如果没有成功获取则为""
		message := c.PostForm("message")
		// 获取一个带默认值的参数
		nick := c.DefaultPostForm("nick", "anonymous")

		c.JSON(200, gin.H{
			"status":  "posted",
			"message": message,
			"nick":    nick,
		})
	})
	router.Run(":8080")
}
```

## 同时获取不同位置的参数
```
POST /post?id=1234&page=1 HTTP/1.1
Content-Type: application/x-www-form-urlencoded

name=manu&message=this_is_great
```

```go
// query 和 post form

func main() {
	router := gin.Default()

	router.POST("/post", func(c *gin.Context) {

		id := c.Query("id")
		page := c.DefaultQuery("page", "0")
		name := c.PostForm("name")
		message := c.PostForm("message")

		fmt.Printf("id: %s; page: %s; name: %s; message: %s", id, page, name, message)
	})
	router.Run(":8080")
}
```

## 获取 Url 参数
如将参数写在了 url 中，如下。
```go
type User struct{
	Id string `form:"id"`
	Name string `form:"name"`
}

user := new(User)

r.GET("user/:id/:name", func(c *gin.Context) {
		if err := c.ShouldBindUri(user); err != nil {
			panic(err)
		}
		c.JSON(http.StatusOK, gin.H{
			"id":   user.ID,
			"name": user.Name,
		})
	})

router.Run(":8080")
```
