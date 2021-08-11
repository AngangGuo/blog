---
title: "Notes - Configuration With Viper"
date: 2021-07-23T12:30:04-07:00
draft: true
---

Viper is a complete configuration solution for Go applications from spf13. 


## Tips
* Viper configuration keys are case-insensitive.
* Priorities: Environment variables > Variables from configuration file > Default
* When working with ENV variables, it’s important to recognize that Viper treats ENV variables as case-sensitive.
* The ENV variable values will be read each time it is accessed.
* Viper can access a nested field by passing a `.` delimited path of keys
* Viper can access array indices by using numbers in the path: `GetInt("host.ports.1")`
```json
{
    "host": {
        "address": "localhost",
        "ports": [
            5799,
            6029
        ]
    },
    ...
}

 GetInt("host.ports.1") // returns 6029   
```

### Get Methods
Each `Get` function will return a zero value if the key is not found. 
Use `IsSet()` method to check if a given key exists.
* `Get(key string) : interface{}`
* `GetBool(key string) : bool`
* `GetFloat64(key string) : float64`
* `GetInt(key string) : int`
* `GetIntSlice(key string) : []int`
* `GetString(key string) : string`
* `GetStringMap(key string) : map[string]interface{}`
* `GetStringMapString(key string) : map[string]string`
* `GetStringSlice(key string) : []string`
* `GetTime(key string) : time.Time`
* `GetDuration(key string) : time.Duration`
* `IsSet(key string) : bool`
* `AllSettings() : map[string]interface{}`

### How to set default values?
```go
viper.SetDefault("ContentDir", "content")
viper.SetDefault("Taxonomies", map[string]string{"tag": "tags", "category": "categories"})
```

### How to load configuration from a file?
config.yaml
```yaml
facility:
  - toronto:
      begin: "2020-10-05"
      end: "2021-07-12"
  - vancouver:
      begin: "2020-10-20"
      end: "2021-07-08"
updated: "2021-07-22"
```
main.go
```go
    viper.SetConfigName("config")
    viper.SetConfigType("yaml")
    // or simply using file name as this
    // viper.SetConfigFile("config.yaml")
    viper.AddConfigPath(".")
    
    viper.AutomaticEnv()
    // To set ENV variable from within go program
    // os.Setenv("ENVONLY","ENV")
    
    err := viper.ReadInConfig()
    if err != nil {
        if _,ok:=err.(viper.ConfigFileNotFoundError);ok{
            // create a config file according to the default values
            // or report the error and exit
        } else {
            log.Fatal(err)
        }
    }

    fmt.Println(viper.AllSettings())
    
    // output:
    // map[facility:[map[toronto:map[begin:2020-10-05 end:2021-07-12]] map[vancouver:map[begin:2020-10-20 end:2021-07-08]]] updated:2021-07-22]
```

## Config File Format
Viper supports the these file format: JSON, TOML, YAML, ENV, INI...

### ENV File
```
TITLE_DOTENV="DotEnv Example"
TYPE_DOTENV=donut
NAME_DOTENV=Cake
```

### JSON File
```json
{
    "id": "0001",
    "type": "donut",
    "name": "Cake",
    "ppu": 0.55,
    "batters": {
        "batter": [
            { "type": "Regular" },
            { "type": "Chocolate" },
            { "type": "Blueberry" },
            { "type": "Devil's Food" }
        ]
    }
}
```

### INI File
```ini
; Package name
NAME = ini
; Package version
VERSION = v1
; Package import path
IMPORT_PATH = gopkg.in/%(NAME)s.%(VERSION)s

# Information about package author
# Bio can be written in multiple lines.
[author]
NAME = Unknown  ; Succeeding comment
E-MAIL = fake@localhost
GITHUB = https://github.com/%(NAME)s
BIO = """Gopher.
Coding addict.
Good man.
"""  # Succeeding comment
```

### TOML File
```toml
title = "TOML Example"

[owner]
organization = "MongoDB"
Bio = "MongoDB Chief Developer Advocate & Hacker at Large"
dob = 1979-05-27T07:32:00Z # First class dates? Why not?
```

### YAML File
```yaml
Hacker: true
name: steve
hobbies:
- skateboarding
- snowboarding
- go
clothing:
  jacket: leather
  trousers: denim
  pants:
    size: large
age: 35
eyes : brown
beard: true
```

### HCL, Remote, Properties and other File
Omit. See [viper_test.go](https://github.com/spf13/viper/blob/master/viper_test.go)
