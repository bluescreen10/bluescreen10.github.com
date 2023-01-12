---
layout: post
title: The missing ternary operator in Go
---

One notable absence in Go's specification is the lack of a ternary operator. Although I understand how abusing it can lead to pretty cryptic code, there are a lot of use cases that could make the code more expressive and succinct.

Fortunately, with the arrival of generics to the language, implementing a ternary operator is pretty straight forward:

```go
package ternary

func T[T any](cond bool, ifTrue, ifFalse T) T {
    switch cond {
    case true:
        return ifTrue
    default:
        return ifFalse
    }
}
```

That's it. This function can make (some) code simpler and more readable. Let's see an example:

```go
package main

//imports

func main() {
    addr := "127.0.0.1"
    if os.GetEnv("ADDR") != "" {
        addr = os.GetEnv("ADDR")
    }
    
    port := "8080"
    if os.GetEnv("PORT") != "" {
        port = os.GetEnv("PORT")
    }

    // more code
    http.ListenAndServe(fmt.Sprintf("%s:%s",addr,port), nil)
}
```

Rewriting this to use our new ternary function it becomes:

```go
package main

//imports

func main() {
    addr := ternary.T(os.GetEnv("ADDR") != "", os.GetEnv("ADDR"), "127.0.0.1")
    port := ternary.T(os.GetEnv("PORT") != "", os.GetEnv("PORT"), "8080")

    // more code
    http.ListenAndServe(fmt.Sprintf("%s:%s",addr,port), nil)
}
```

Even in this small example I believe it makes the code easier to read. If you agree, feel free to use the code above or if you are really lazy you can import the [module](https://github.com/bluescreen10/ternary) I've created.
