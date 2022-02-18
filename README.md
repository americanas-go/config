config
=======

Add flexibility to the way you configure your applications. Load it from files as  JSON, YAML and TOML, command line flags, environment variables and more.
This package is a wrapper for [koanf](https://github.com/knadh/koanf).

Installation
------------

	go get -u github.com/americanas-go/config


Example reading from files
--------
```yaml
# config.yaml
app:
  application:
    name: app_example_file
```
```go
package main

import (
    "log"

    "github.com/americanas-go/config"
)

func init() {
    config.Add("app.application.name", "app_example_file", "description of name")
}

func main() {

    config.Load()

    log.Println(config.String("app.application.name"))
    //output: 2021/05/28 16:52:49 app_example_file
}
```

Example reading from environment variables
--------
```go
package main

import (
    "log"

    "github.com/americanas-go/config"
)

func init() {
    config.Add("k.env", "env", "description of env")
    config.Add("k.camelCase", "camel-case", "description of camel case")
    config.Add("k.camelCaseTwo", "camel_case_two", "description of camel case two")
}

func main() {

    config.Load()

    log.Println(config.String("k.env"))
    //output: 2021/05/28 17:20:03 env
    log.Println(config.String("k.camelCase"))
    //output: 2021/05/28 17:20:03 camel-case
    log.Println(config.String("k.camelCaseTwo"))
    //output: 2021/05/28 17:20:03 camel_case_two
}
```

Example reading from command line
--------
```go
package main

import (
    "log"

    "github.com/americanas-go/config"
)

func init() {
    config.Add("app.application.name", "app_example_file", "description of name")
    config.Add("app.application.enabled", true, "description of enabled")
    config.Add("app.application.duration", 10, "description of duration")
}

func main() {

    config.Load()

    log.Println(config.String("app.application.name"))
    //output: 2021/05/28 17:40:27 app_example_file
    log.Println(config.Bool("app.application.enabled"))
    //output: 2021/05/28 17:40:27 true
    log.Println(config.Int("app.application.duration"))
    //output: 2021/05/28 17:40:27 10
}
```

Example Unmarshalling
--------
```yaml
# config.yaml
app:
  application:
    name: app_example_file
    enabled: true
    duration: 10
```
```go
package main

import (
    "log"

    "github.com/americanas-go/config"
)

type AppConfig struct {
    Application struct {
        Name     string
        Enabled  bool
        Duration int
    }
}

func init() {
    config.Add("app.application.name", "app_example_file", "description of name")
    config.Add("app.application.enabled", true, "description of enabled")
    config.Add("app.application.duration", 10, "description of duration")
}

func main() {

    config.Load()

    c := AppConfig{}

    config.UnmarshalWithPath("app", &c)

    log.Println(c.Application.Name)
    //output: 2021/05/28 17:54:12 app_example_file
    log.Println(c.Application.Enabled)
    //output: 2021/05/28 17:54:12 true
    log.Println(c.Application.Duration)
    //output: 2021/05/28 17:54:12 10
}
```

Contributing
--------
Every help is always welcome. Feel free do throw us a pull request, we'll do our best to check it out as soon as possible. But before that, let us establish some guidelines:

1. This is an open source project so please do not add any proprietary code or infringe any copyright of any sort.
2. Avoid unnecessary dependencies or messing up go.mod file.
3. Be aware of golang coding style. Use a lint to help you out.
4. Add tests to cover your contribution.
5. Add [godoc](https://elliotchance.medium.com/godoc-tips-tricks-cda6571549b) to your code. 
6. Use meaningful [messages](https://medium.com/@menuka/writing-meaningful-git-commit-messages-a62756b65c81) to your commits.
7. Use [pull requests](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests).
8. At last, but also important, be kind and polite with the community.

Any submitted issue which disrespect one or more guidelines above, will be discarded and closed.


<hr>

Released under the [MIT License](LICENSE).
