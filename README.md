# webdav

![Build](https://github.com/dbian/webdav/workflows/Tests/badge.svg)
[![Go Report Card](https://goreportcard.com/badge/github.com/dbian/webdav?style=flat-square)](https://goreportcard.com/report/dbian/webdav)
[![Version](https://img.shields.io/github/release/dbian/webdav.svg?style=flat-square)](https://github.com/dbian/webdav/releases/latest)
[![Docker Pulls](https://img.shields.io/docker/pulls/dbian/webdav)](https://hub.docker.com/r/dbian/webdav)


## Prelude

Inpired by [hacdias](https://github.com/hacdias/webdav), i forked his repo and do some own development.

### Mainly Features
1. Specify as many folders as you can, and map to their virtual folder names, see the examples below. In Chinese: 可管理多个文件夹，映射到各自的虚拟目录




## Install

Please refer to the [Releases page](https://github.com/dbian/webdav/releases) for more information. There, you can either download the binaries or find the Docker commands to install WebDAV.

## Usage

```webdav``` command line interface is really easy to use so you can easily create a WebDAV server for your own user. By default, it runs on a random free port and supports JSON, YAML and TOML configuration. An example of a YAML configuration with the default configurations:

```yaml
# Server related settings
address: 0.0.0.0
port: 0
auth: true
tls: true # better to set to true, cause the basic auth is easy to be hacked
cert: cert.pem
key: key.pem

# Default user settings (will be merged)
scope: .
modify: true
rules: []

folders:
  - path: /public/a/ # windows: C:\persist
    mapto: /folder_a # uri: https://a.com/folder_a
  - path: /public/b/
    mapto: /folder_b # uri: https://a.com/folder_b

users:
  - username: encrypted
    password: "{bcrypt}$2y$10$zEP6oofmXFeHaeMfBNLnP.DO8m.H.Mwhd24/TOX2MWLxAExXi4qgi"
  - username: "{env}ENV_USERNAME"
    password: "{env}ENV_PASSWORD"
  - username: basic
    password: basic
     
```

There are more ways to customize how you run WebDAV through flags and environment variables. Please run `webdav --help` for more information on that.

### Systemd

An example of how to use this with `systemd` is on [webdav.service.example](/webdav.service.example).

### CORS

The `allowed_*` properties are optional, the default value for each of them will be `*`. `exposed_headers` is optional as well, but is not set if not defined. Setting `credentials` to `true` will allow you to:

1. Use `withCredentials = true` in javascript.
2. Use the `username:password@host` syntax.

### Reverse Proxy Service
When you use a reverse proxy implementation like `Nginx` or `Apache`, please note the following fields to avoid causing `502` errors
```text
location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
    }
```
