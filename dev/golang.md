## Zero values
Go initialize variables with zero values:
- number types with 0
- boolean with false
- pointer with nil

## Blank identifier
Go has "black hole" where you can assign unused variable:
```go
r, _ := some_func_with_two_return_values()
```

## Strings
- are immutable, use `byte-slices` for mutability

## Hash maps
### gotchas
- impossible to add item to `nil` map

```go
package main

func main() {  
    var m map[string]int
    m["one"] = 1 // runtime panic
}
```

- there is no `capacity` method in maps

```go
package main

func main() {  
    m := make(map[string]int,99)
    cap(m)
    // invalid argument m (type map[string]int) for cap
}
```

## Constants
- constants can be untyped, which means they be converted in place like literals
- iota - constant generator for ordered constants

## Other
- If you can assign sth than you can compare with it
- Incdec statement cannot be used as expression like in C
- on channel closing `select case` is triggered
- channel closing is not blocking, so it can be used instead of writing

# Snippets

## Closing http-response

```go
package main

import (  
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {  
    resp, err := http.Get("https://api.ipify.org?format=json")
    if resp != nil {
        defer resp.Body.Close()
    }

    if err != nil {
        fmt.Println(err)
        return
    }

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Println(err)
        return
    }

    fmt.Println(string(body))
}
```

## Closing http-connection

```go
package main

import (  
    "fmt"
    "net/http"
    "io/ioutil"
)

func main() {  
    req, err := http.NewRequest("GET","http://golang.org",nil)
    if err != nil {
        fmt.Println(err)
        return
    }

    req.Close = true
    //or do this:
    //req.Header.Add("Connection", "close")

    resp, err := http.DefaultClient.Do(req)
    if resp != nil {
        defer resp.Body.Close()
    }

    if err != nil {
        fmt.Println(err)
        return
    }

    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Println(err)
        return
    }

    fmt.Println(len(string(body)))
}
```