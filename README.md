# UGin - Ultimate Gin API
UGin is an API boilerplate written in Go (Golang) with Gin Framework. https://www.yakuter.com/

## Database Support
UGin uses **gorm** as an ORM. So **Sqlite3**, **MySQL** and **PostgreSQL** is supported. You just need to edit **config.yml** file according to your setup. 

**config.yml** content:
```
database:
  driver: "postgres"
  dbname: "database_name"
  username: "user"
  password: "password"
  host: "localhost"
  port: "5432"
```

## Filtering, Search and Pagination
With UGin, anything you need for an API is ready. If you need a filter, search and paginate for the results, you just need to use the parameters below:

Endpoint: /posts/

**Query parameters:**
limit=2
offset=0
name=Third
description=My
order=name|asc

Full: **http://localhost:8081/posts/?limit=2&offset=0&name=Third&description=My&order=name|asc**

## Dependencies
**UGin** uses **Gin** for main framework, **Gorm** for database and **Viper** for configuration.
```
go get -u github.com/gin-gonic/gin
go get -u github.com/jinzhu/gorm
go get -u github.com/jinzhu/gorm/dialects/postgres
go get -u github.com/jinzhu/gorm/dialects/sqlite
go get -u github.com/jinzhu/gorm/dialects/mysql
go get -u github.com/spf13/viper
```
## Middlewares
### 1. Logger and Recovery Middlewares
Gin has 2 important built-in middlewares: **Logger** and **Recovery**. UGin calls these two in default.
```
router := gin.Default()
```

This is same with the following lines.
```
router := gin.New()
router.Use(gin.Logger())
router.Use(gin.Recovery())
```

### 2. CORS Middleware
CORS is really important for API's and UGin has it's own CORS middleware in **include/middleware.go**. CORS middleware is called with the code below.
```
router.Use(include.CORS())
```
There is also a good repo for this: https://github.com/gin-contrib/cors

### 3. BasicAuth Middleware
Almost every API needs a protected area. Gin has **BasicAuth** middleware for protecting routes. Basic Auth is an authorization type that requires a verified username and password to access a data resource. In UGin, you can find an example for a basic auth. To access these protected routes, you need to add **Basic Authorization credentials** in your requests. If you try to reach these endpoints from browser, you should see a window prompting you for username and password.

```
authorized := router.Group("/admin", gin.BasicAuth(gin.Accounts{
    "username": "password",
}))

// /admin/dashboard endpoint is now protected
authorized.GET("/dashboard", controller.Dashboard)
```

If you want to use JWT for authorization in UGin, you can check this: https://github.com/appleboy/gin-jwt

## Default Endpoints
| Method | URI              | Function               |
|--------|------------------|------------------------|
| GET    | /posts/          | controller.GetPosts    |
| POST   | /posts/          | controller.CreatePost  |
| GET    | /posts/:id       | controller.GetPost     |
| PUT    | /posts/:id       | controller.UpdatePost  |
| DELETE | /posts/:id       | controller.DeletePost  |
| GET    | /admin/dashboard | controller.Dashboard   |
