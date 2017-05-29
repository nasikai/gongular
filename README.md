![gongular](https://raw.githubusercontent.com/mustafaakin/gongular/master/logo.png)

[![Coverage Status](https://coveralls.io/repos/github/mustafaakin/gongular/badge.svg?branch=master)](https://coveralls.io/github/mustafaakin/gongular?branch=master)
[![Build Status](https://travis-ci.org/mustafaakin/gongular.svg?branch=master)](https://travis-ci.org/mustafaakin/gongular)
[![Go Report Card](https://goreportcard.com/badge/github.com/mustafaakin/gongular)](https://goreportcard.com/report/github.com/mustafaakin/gongular)
[![GoDoc](https://godoc.org/github.com/mustafaakin/gongular?status.svg)](https://godoc.org/github.com/mustafaakin/gongular)

**Note:** gongular recently updated, and if you are looking for the previous version it is tagged as [v.1.0](https://github.com/mustafaakin/gongular/tree/v1.0) 

gongular is an HTTP Server Framework for developing APIs easily. It is like Gin Gonic, but it features Angular-like (or Spring like) dependency injection and better input handling. Most of the time, user input must be transformed into a structured data then it must be validated. It takes too much time and is a repetitive work, gongular aims to reduce that complexity by providing request-input mapping with tag based validation.

**Note:** gongular is an opinionated framework and it heavily relies on reflection to achieve these functionality. While there are tests to ensure it works flawlessly, I am open to contributions and opinions on how to make it better. 

## Features

* Automatic Query, POST Body, URL Param binding to structs with easy validation
* Easy and simple dependency injection i.e passing DB connections and other values
* Custom dependency injection with user specified logic, i.e as User struct from a session
* Route grouping that allows reducing duplicated code
* Middlewares that can do preliminary work before routes, groups which might be helpful for authentication checks, logging etc.
* Static file serving 
* Very fast thanks to httprouter

## Simple Usage

gongular aims to be simple as much as possible while providing flexibility. The below example is enough to reply user with its IP.

```go
type WelcomeMessage struct {}
func(w *WelcomeMessage) Handle(c *gongular.Context) error {
    c.SetBody(c.Request().RemoteAddr)
}

g := gongular.NewEngine()
g.GET("/", &WelcomeMessage{})
g.ListenAndServe(":8000")
```

## How to Use

All HTTP handlers in gongular are structs with `Handle(c *gongular.Context) error` function or in other words `RequestHandler` interface, implemented. Request handler objects are flexible. They can have various fields, where some of the fields with specific names are special. For instance, if you want to bind the path parameters, your handler object must have field named `Param` which is a flat struct. Also you can have a `Query` field which also maps to query parameters. `Body` field lets you map to JSON body, and `Form` field lets you bind into form submissions with files.

```go
type MyHandler struct {
    Param struct {
        UserID int       
    }
    Query struct {
        Name  string
        Age   int
        Level float64
    }
    Body struct {
        Comment string
        Choices []string
        Address struct {
            City    string
            Country string
            Hello   string            
        }
    }
}
func(m *MyHandler) Handle(c *gongular.Context) error {
    c.SetBody("Wow so much params")
    return nil
}
```

## Path Parameters

We use julienschmidt/httprouter to multiplex requests and do parametric binding to requests. So the format :VariableName, *somepath is supported in paths. Note that, you can use valid struct tag to validate parameters.

```go
type PathParamHandler struct {
    Param struct {
        Username string
    }
}
func(p *PathParamHandler) Handle(c *Context) error {
    c.SetBody(p.Param.Username)
    return nil
}
```

## Query Parameters

Query parameter is very similar to path parameters, the only difference the field name should be `Query` and it should also be a flat struct with no inner parameters or arrays.

```go
type QueryParamHandler struct {
    Query struct {
        Username string
        Age int
    }
}
func(p *QueryParamHandler) Handle(c *Context) error {
    println(p.Param.Age)
    c.SetBody(p.Param.Username)
    return nil
}
```

## JSON Request Body 

## Forms and File Uploading

## Routes and Grouping

Routes can have multiple handlers, called middlewares, which might be useful in grouping the requests and doing preliminary work before some routes. For example, the following grouping and routing is valid:  

## Field Validation

We use asaskevich/govalidator as a validation framework. If the supplied input does not pass the validation step, http.StatusBadRequest (400) is returned the user with the cause. Validation can be used in Query, Param, Body or Form type inputs.

## Dependency Injection

One of the thing that makes gongular from other frameworks is that it provides safe value injection to route handlers. It can be used to store database connections, or some other external utility that you want that to be avilable in your handler, but do not want to make it global, or just get it from some other global function that might pollute the space. Supplied dependencies are provided as-is to route handlers and they are private to supplied router, nothing is global.



## gongular.Context struct
 


