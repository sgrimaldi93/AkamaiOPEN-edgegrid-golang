# EdgeGrid for GoLang

[![Build Status](https://travis-ci.org/njuettner/edgegrid.svg?branch=master)](https://travis-ci.org/njuettner/edgegrid)
[![Coverage Status](https://coveralls.io/repos/github/njuettner/edgegrid/badge.svg?branch=master)](https://coveralls.io/github/njuettner/edgegrid?branch=master)

This library implements an Authentication handler for [net/http](https://golang.org/pkg/net/http/)
that provides the [Akamai {OPEN} Edgegrid Authentication](https://developer.akamai.com/introduction/Client_Auth.html) 
scheme. For more information visit the [Akamai {OPEN} Developer Community](https://developer.akamai.com).

```go
  package main

  import (
    "fmt"
    "github.com/njuettner/edgegrid"
    "io/ioutil"
    "net/http"
  )

  func main() {
    client := http.Client{}
    baseURL := "https://xxxxxx.luna.akamaiapis.net"
    req, _ := http.NewRequest("GET", fmt.Sprintf("%s/siteshield/v1/maps", baseURL), nil)
    config := edgegrid.Config{
      ClientToken:  "xxxx-xxxxxxxxxxx-xxxxxxxxxxx",
      ClientSecret: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      AccessToken:  "xxxx-xxxxxxxxxxx-xxxxxxxxxxx",
      MaxBody:      1024,
      HeaderToSign: []string{
        "X-Test1",
        "X-Test2",
        "X-Test3",
      },
      Debug:        false,
    }

    req = edgegrid.AddRequestHeader(config, req)
    resp, _ := client.Do(req)
    byt, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(byt))
  }
```

Alternatively, your program can read the settings from an INI file.

```go
  package main

  import (
    "fmt"
    "github.com/njuettner/edgegrid"
    "io/ioutil"
    "net/http"
  )

  func main() {
    client := http.Client{}
    baseURL := "https://xxxxxx.luna.akamaiapis.net"
    req, _ := http.NewRequest("GET", fmt.Sprintf("%s/siteshield/v1/maps", baseURL), nil)

    config := edgegrid.InitConfig("edgerc")

    req = edgegrid.AddRequestHeader(config, req)
    resp, _ := client.Do(req)
    byt, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(byt))
  }
```

## Installation

```bash
  $ go get github.com/njuettner/edgegrid
```

## Contribute

1. Fork [the repository](https://github.com/njuettner/edgegrid) to start making your changes to the **master** branch
2. Send a pull request.

## Author

[Nick Juettner](mailto:hello@juni.io)
