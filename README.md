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
```

## How to Use

All HTTP handlers in gongular are structs with `Handle(c *gongular.Context) error` function implemented. 