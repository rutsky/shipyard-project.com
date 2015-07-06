+++
Categories = []
Description = ""
Tags = []
title = "API"
date = 2014-09-10T03:30:04Z
+++

At the core of Shipyard is the API.  The API is used to manage everything in the cluster.

# Contents

## Authorization
* [Login](#auth-login)

## Accounts
* [List Accounts](#get-accounts)
* [Create Account](#post-accounts)
* [Delete Account](#delete-account)

## Roles
* [List Roles](#get-roles)
* [Get Role Details](#get-role)
* [Create Role](#post-roles)
* [Delete Role](#delete-role)

## Containers
* [List Containers](#get-containers)
* [Deploy Container](#post-containers)
* [Inspect Container](#get-container)
* [Destroy Container](#delete-container)
* [Stop Container](#stop-container)
* [Restart Container](#restart-container)
* [Scale Container](#scale-container)
* [Container Logs](#get-container-logs)

## Engines
* [List Engines](#get-engines)
* [Inspect Engine](#get-engine)
* [Add Engine](#post-engines)
* [Remove Engine](#delete-engine)

## Service Keys
* [List Service Keys](#get-service-keys)
* [Create Service Key](#post-service-keys)
* [Delete Service Key](#delete-service-key)

## Webhook Keys
* [List Webhook Keys](#get-webhook-keys)
* [Create Webhook Key](#post-webhook-keys)
* [Delete Webhook Key](#delete-webhook-key)

## Events
* [List Events](#get-events)

## Cluster Info
* [Get Cluster Info](#get-cluster-info)

## Docker Hub
* [Deployment](#docker-hub-deployment)

# Login
<a name="auth-login"></a>
## POST /auth/login
This will return an authorization token.  This is not the same as a service key.  Authorization tokens are used for user login not services.

Request

```json
POST /auth/login HTTP/1.1
Content-Type application/json

{
  "username": "foo",
  "password": "bar",
}
```

Response

```json
{
  "auth_token": "$2a$10$QKbbcXM5pUUXM/0bTwC8re/PU2jB5wqLEyUcRZtHcavjwdfmW/y4W",
  "user_agent":"curl/7.35.0"
}
```

# API Authentication
To access the Shipyard API, you must be authenticated.  The recommended way is to use service keys.  To create a [Service Key](/docs/servicekeys/).  All requests need to have the header `X-Service-Key` with your service key.

```json
curl -s -H "X-Service-Key: Lpc.usH1skelCuqwvjAtF.lJsWGwaKiwey2K" \
    http://<shipyard-url>:8080/api/cluster/info
{
  "reserved_memory": 1024,
  "reserved_cpus": 0.32,
  "image_count": 17,
  "engine_count": 1,
  "container_count": 4,
  "memory": 4096,
  "cpus": 4
}
```

# Accounts

<a name="get-accounts"></a>
## GET /api/accounts
Gets a list of accounts

Request

`GET /api/accounts HTTP/1.1`

Response

```json
[
  {
    "role": {
      "name": "admin",
      "id": "8c10fe3d-bf10-4bb9-ab4c-002502b71f13"
    },
    "password": "$2a$10$UvzJ8tssyF0Uvpaxmr06QOVbyJn5gTS4gDLapwo",
    "username": "admin",
    "id": "5f8e6567-9244-4fb2-bde9-24fb5e630b63"
  },
  {
    "role": {
      "name": "user",
      "id": "ddc9a1b1-771b-4a07-807a-42cd027922a5"
    },
    "password": "$2a$10$EEkqHiOjC.018pnqr1giDe0ODVcGaDUC/2lXg",
    "username": "test",
    "id": "44a22dd3-ae74-4fe3-bdf6-58bbd0094f98"
  }
]
```

<a name="post-accounts"></a>
## POST /api/accounts
Create an account

Request

```json
POST /api/accounts HTTP/1.1
Content-Type application/json

{
  "username": "foo",
  "password": "bar",
  "role": {
    "name": "user"
  }
}
```

Response

`HTTP/1.1 204 No Content`

<a name="delete-account"></a>
## DELETE /api/accounts
Delete an account

Request

```json
DELETE /api/accounts HTTP/1.1
Content-Type application/json

{
  "username": "foo"
}
```

Response

`HTTP/1.1 204 No Content`

# Roles

<a name="get-roles"></a>
## GET /api/roles
Gets a list of roles

Request

`GET /api/roles HTTP/1.1`

Response

```json
[
  {
    "name": "admin",
    "id": "448ebe2d-89d9-412c-aab9-ad774d1ec78f"
  },
  {
    "name": "user",
    "id": "2243edf5-55f1-43ad-9ebb-c36c5233f007"
  }
]
```

<a name="get-role"></a>
## GET /api/roles/\<username\>
Gets a role

Request

`GET /api/roles/admin HTTP/1.1`

Response

```json
{
  "name": "admin",
  "id": "448ebe2d-89d9-412c-aab9-ad774d1ec78f"
}
```

<a name="post-roles"></a>
## POST /api/roles
Create a role

Request

```json
POST /api/roles HTTP/1.1
Content-Type application/json
{
  "name": "test"
}
```

Response

`HTTP/1.1 204 No Content`

<a name="delete-role"></a>
## DELETE /api/roles
Delete an role

Request

```json
DELETE /api/roles HTTP/1.1
Content-Type application/json

{
  "name": "test"
}
```

Response

`HTTP/1.1 204 No Content`

# Containers

<a name="get-containers"></a>
## GET /api/containers
Get list of containers

Response

```json
GET /api/containers HTTP/1.1

[
  {
    "state": {
      "started_at": "2014-09-12T00:48:23.824260519Z",
      "pid": 5845,
      "running": true
    }, 
    "ports": [
      {
        "container_port": 8080,
        "port": 49159,
        "proto": "tcp"
      }
    ],
    "engine": {
      "labels": [
        "local",
        "dev"
      ],
      "memory": 4096,
      "cpus": 4,
      "addr": "http://172.16.1.50:2375",
      "id": "local"
    },
    "image": {
      "restart_policy": {},
      "labels": [
        ""
      ],
      "type": "service",
      "hostname": "cbe68bf32f1a",
      "environment": {
        "GOROOT": "/goroot",
        "GOPATH": "/gopath"
      },
      "memory": 256,
      "cpus": 0.08,
      "name": "ehazlett/go-demo:latest"
    },
    "id": "cbe68bf32f1a08218693dbee9c66ea018c1a99c75c463a76b"
  },
  {
    "state": {
      "started_at": "2014-09-12T00:48:23.824260519Z",
      "pid": 5846,
      "running": true
    }, 
    "ports": [
      {
        "container_port": 8080,
        "port": 49158,
        "proto": "tcp"
      }
    ],
    "engine": {
      "labels": [
        "local",
        "dev"
      ],
      "memory": 4096,
      "cpus": 4,
      "addr": "http://172.16.1.50:2375",
      "id": "local"
    },
    "image": {
      "restart_policy": {},
      "labels": [
        ""
      ],
      "type": "service",
      "hostname": "eca254ecd76e",
      "environment": {
        "GOROOT": "/goroot",
        "GOPATH": "/gopath"
      },
      "memory": 256,
      "cpus": 0.08,
      "name": "ehazlett/go-demo:latest"
    },
    "id": "eca254ecd76eb9d887995114ff811cc5b7c14fe13630"
  }
]
```

<a name="post-containers"></a>
## POST /api/containers
Deploy a container

Request

```json
POST /api/containers HTTP/1.1
Content-Type application/json

{
  "name": "ehazlett/go-demo",
  "container_name": "demo",
  "cpus": 0.1,
  "memory": 32,
  "type": "service",
  "hostname": "",
  "domain": "",
  "labels": ["local"],
  "args": [],
  "environment": {
    "FOO": "bar"
  },
  "restart_policy": {
    "name": "always"
  },
  "bind_ports": [
    {
      "host_ip": "10.1.2.3",
      "proto": "tcp",
      "container_port": 8080
    },
    {
      "proto": "tcp",
      "port": 80,
      "container_port": 8080
    }
  ],
  "links": {
    "redis": "db"
  }
}
```

Response

`HTTP/1.1 201 Created`

```json
[
  {
    "state": {
      "started_at": "2014-09-12T00:48:23.824260519Z",
      "pid": 5890,
      "running": true
    }, 
    "ports": [
      {
        "container_port": 8080,
        "host_ip": "10.1.2.3",
        "port": 49172,
        "proto": "tcp"
      }
    ],
    "engine": {
      "labels": [
        "local",
        "dev"
      ],
      "memory": 4096,
      "cpus": 4,
      "addr": "http://172.16.1.50:2375",
      "id": "local"
    },
    "image": {
      "restart_policy": {
        "name": "always"  
      },
      "labels": [
        "local"
      ],
      "bind_ports": [
        {
          "proto": "tcp",
          "port": 49153,
          "container_port": 8080
        },
        {
          "proto": "tcp",
          "port": 80,
          "container_port": 8080
        }
      ],
      "links": {
        "redis": "db"
      },
      "type": "service",
      "memory": 32,
      "cpus": 0.1,
      "name": "ehazlett/go-demo"
    },
    "id": "4a5da04b8428e7241a9d9993699513d11b89948399dedfa12"
  }
]
```
<a name="get-container"></a>
## GET /api/containers/\<id\>
Inspect a container

Request

`GET /api/containers/3e532b HTTP/1.1`

Response

```json
{
  "state": {
    "started_at": "2014-09-12T00:48:23.824260519Z",
    "pid": 5891,
    "running": true
  }, 
  "ports": [
    {
      "container_port": 8080,
      "port": 49155,
      "proto": "tcp"
    }
  ],
  "engine": {
    "labels": [
      "local",
      "dev"
    ],
    "memory": 4096,
    "cpus": 4,
    "addr": "http://172.16.1.50:2375",
    "id": "local"
  },
  "image": {
    "restart_policy": {},
    "labels": [
      "local"
    ],
    "type": "service",
    "hostname": "demo-1",
    "environment": {
      "GOROOT": "/goroot",
      "GOPATH": "/gopath"
    },
    "memory": 256,
    "cpus": 0.08,
    "name": "ehazlett/go-demo:latest"
  },
  "id": "3e532b000891e90e93ca3781031e7c1ddb76d8378dfdfd3"
}
```

<a name="delete-container"></a>
## DELETE /api/containers/\<id\>
Destroy a container

Request

`DELETE /api/containers/3e532 HTTP/1.1`

Response

`HTTP/1.1 204 No Content`

<a name="stop-container"></a>
## GET /api/containers/\<id\>/stop
Stop a container

Request

`GET /api/containers/3e532/stop HTTP/1.1`

Response

`HTTP/1.1 204 No Content`

<a name="restart-container"></a>
## GET /api/containers/\<id\>/restart
Restart a container

Request

`GET /api/containers/3e532/restart HTTP/1.1`

Response

`HTTP/1.1 204 No Content`

<a name="scale-container"></a>
## GET /api/containers/\<id\>/scale?count=\<count\>
Scale a container to the desired number of instances

Request

`GET /api/containers/3e532/scale?count=10 HTTP/1.1`

Response

`HTTP/1.1 204 No Content`

<a name="get-container-logs"></a>
## GET /api/containers/\<id\>/logs
Get container logs

Request

`GET /api/containers/3e532b/logs HTTP/1.1`

Response

```
listening on :8080
```

# Engines

<a name="get-engines"></a>
## GET /api/engines
Gets engines

Request

`GET /api/engines HTTP/1.1`

Response

```json
GET /api/engines HTTP/1.1

[
  {
    "engine": {
      "labels": [
        "local",
        "dev"
      ],
      "memory": 4096,
      "cpus": 4,
      "addr": "http://172.16.1.50:2375",
      "health": {
        "status": "up",
        "response_time": 100
      }
      "id": "local"
    },
    "id": "99095f5f-7579-4a70-9369-04ad73c21312"
  }
]
```

<a name="get-engine"></a>
## GET /api/engines/\<id\>
Inspect engine

Request

`GET /api/engines/local HTTP/1.1`

Response

```json
GET /api/engines/local HTTP/1.1

{
  "engine": {
    "labels": [
      "local",
      "dev"
    ],
    "memory": 4096,
    "cpus": 4,
    "addr": "http://172.16.1.50:2375",
    "id": "local"
  },
  "id": "99095f5f-7579-4a70-9369-04ad73c21312"
}
```

<a name="post-engines"></a>
## POST /api/engines
Add an engine to the cluster

Request

```json
POST /api/engines HTTP/1.1
Content-Type application/json

{
  "id": "local",
  "ssl_cert": "",
  "ssl_key": "",
  "ca_cert": "",
  "engine": {
    "id": "local",
    "addr": "http://10.1.2.3:2375",
    "cpus": 4.0,
    "memory": 8192,
    "labels": [
      "local",
      "dev"
    ]
  }
}
```

Response

`HTTP/1.1 201 Created` 

<a name="delete-engine"></a>
## DELETE /api/engines/\<id\>
Remove an engine from the cluster

Request

`DELETE /api/engines/99095f5f-7579-4a70-9369-04ad73c21312 HTTP/1.1`

Response

`HTTP/1.1 204 No Content`

# Service Keys

<a name="get-service-keys"></a>
## GET /api/servicekeys
Get list of service keys

Request

`GET /api/servicekeys HTTP/1.1`

Response

```json
GET /api/servicekeys HTTP/1.1

[
  {
    "description": "test",
    "key": "3pYgOl4K7vlkymoi1TMLIAQIJqcYhkGWY04."
  },
  {
    "description": "demo",
    "key": "Lpc.usH1skelCuqwvjAtF.lJsWGwaKiwey2K"
  }
]
```

<a name="post-service-keys"></a>
## POST /api/servicekeys
Create a service key

Request

```json
POST /api/servicekeys HTTP/1.1
Content-Type application/json

{
  "description": "test key"
}
```

Response

```json
{
  "description": "test key",
  "key": "zuoWetDKDRhPyUNRZro5cLo7yaFLKgzcqijW"
}
```

<a name="delete-service-key"></a>
## DELETE /api/servicekeys
Delete a service key

Request

```json
DELETE /api/servicekeys HTTP/1.1
Content-Type application/json

{
  "key": "zuoWetDKDRhPyUNRZro5cLo7yaFLKgzcqijW"
}
```

Response

`HTTP/1.1 204 No Content`

# Webhook Keys
Webhook keys are used to allow deployment via the Docker Hub.

<a name="get-webhook-keys"></a>
## GET /api/webhookkeys
Get list of webhook keys

Request

`GET /api/webhookkeys HTTP/1.1`

Response

```json
GET /api/servicekeys HTTP/1.1

[
  {
    "key": "fc563339166b3c69",
    "image": "ehazlett/go-demo",
    "id": "d7c68d6f-65bc-4e0c-8134-8e281a6b4d9b"
  }
]
```

<a name="post-webhook-keys"></a>
## POST /api/webhookkeys
Create a webhook key

Request

```json
POST /api/webhookkeys HTTP/1.1
Content-Type application/json

{
  "image": "ehazlett/redis"
}
```

Response

```json
{
    "image": "ehazlett/redis",
    "key":"8d710a03c7d965aa"
}
```

<a name="delete-webhook-key"></a>
## DELETE /api/webhooks/\<id\>
Delete a webhook key

Request

```json
DELETE /api/webhooks/8d710a03c7d965aa HTTP/1.1

Response

`HTTP/1.1 204 No Content`

# Events

<a name="get-events"></a>
## GET /api/events
Gets cluster events

Request

`GET /api/events HTTP/1.1`

Response

```json
GET /api/events HTTP/1.1

[
  {
    "tags": [
      "cluster",
      "security"
    ],
    "message": "description=demo",
    "time": "2014-09-10T20:41:43Z",
    "type": "add-service-key"
  },
  {
    "tags": [
      "docker"
    ],
    "time": "2014-09-10T14:04:45Z",
    "engine": {
      "labels": [
        "local",
        "dev"
      ],
      "memory": 4096,
      "cpus": 4,
      "addr": "http://172.16.1.50:2375",
      "id": "local"
    },
    "container": {
      "ports": [
        {
          "container_port": 8080,
          "port": 49161,
          "proto": "tcp"
        }
      ],
      "engine": {
        "labels": [
          "local",
          "dev"
        ],
        "memory": 4096,
        "cpus": 4,
        "addr": "http://172.16.1.50:2375",
        "id": "local"
      },
      "image": {
        "restart_policy": {},
        "name": "ehazlett/go-demo:latest",
        "cpus": 0.08,
        "memory": 256,
        "environment": {
          "INTERLOCK_DATA": "'{\"alias_domains\":[\"foo.com\"]}'",
          "GOROOT": "/goroot",
          "GOPATH": "/gopath"
        },
        "hostname": "foo",
        "domain": "example.com",
        "type": "service",
        "labels": [
          ""
        ]
      },
      "id": "53791ec5c3a6f8100c79447c5114ca0be67320d68d851d5e7ac578d1c2665532"
    },
    "type": "create"
  }
]
```

# Info

<a name="get-cluster-info"></a>
## GET /api/cluster/info
Gets cluster info

Request

`GET /api/cluster/info HTTP/1.1`

Response

```json
GET /api/cluster/info HTTP/1.1

{
  "reserved_memory": 768,
  "reserved_cpus": 0.24,
  "image_count": 6,
  "engine_count": 1,
  "container_count": 3,
  "memory": 4096,
  "cpus": 4
}
```

# Docker Hub

<a name="docker-hub-deployment"></a>
## POST /hub/webhook/
Auto deploy via Docker Hub webhook.  This will pull the latest image from the Docker Hub and re-launch all containers in the cluster with the same image name.  See a demo [here](https://asciinema.org/a/12434).

To set up a repo for auto-deploy, add a Webhook in the Docker Hub repo settings and point it to `http://<shipyard-url>/hub/webhook/`

Request

`POST /hub/webhook HTTP/1.1`

```json
POST /hub/webhook HTTP/1.1
Content-Type application/json

{
   "push_data":{
      "pushed_at":1385141110,
      "images":[
         "imagehash1",
         "imagehash2",
         "imagehash3"
      ],
      "pusher":"username"
   },
   "repository":{
      "status":"Active",
      "description":"my docker repo that does cool things",
      "is_trusted":false,
      "full_description":"This is my full description",
      "repo_url":"https://registry.hub.docker.com/u/username/reponame/",
      "owner":"username",
      "is_official":false,
      "is_private":false,
      "name":"reponame",
      "namespace":"username",
      "star_count":1,
      "comment_count":1,
      "date_created":1370174400,
      "dockerfile":"my full dockerfile is listed here",
      "repo_name":"username/reponame"
   }
}
```

Response

`HTTP/1.1 200 OK`

