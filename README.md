# Eureka Lite

Run `EurekaLiteApplication` which has the `@EnableEurekaLite` annotation.

```
$ http --print=b POST :8762/apps name=myapp instance_id=app1 hostname=localhost port=8081 > /tmp/eureka_myapp_app1.json && cat /tmp/eureka_myapp_app1.json 
$ http POST :8762/apps name=myapp instance_id=app2 hostname=localhost port=8082
$ http POST :8762/apps name=anotherapp instance_id=anotherapp1 hostname=localhost port=8181

# in another terminal
$ watch -n30 http --print=b PUT :8762/apps/myapp/app1 < /tmp/eureka_myapp_app1.json 

# eventually anotherapp/anotherapp1 will auto unregister from eureka because it isn't sending heartbeats

$ http DELETE :8762/apps/myapp/app2

$ http GET :8762/apps
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Date: Mon, 20 Jun 2016 22:35:59 GMT
Server: Apache-Coyote/1.1
Transfer-Encoding: chunked
X-Application-Context: application:8762

[
    {
        "application": {
            "hostname": "localhost",
            "instance_id": "app2",
            "name": "myapp",
            "port": 8082
        },
        "status": "DOWN"
    },
    {
        "application": {
            "hostname": "localhost",
            "instance_id": "app1",
            "name": "myapp",
            "port": 8081
        },
        "status": "UP"
    },
    {
        "application": {
            "hostname": "localhost",
            "instance_id": "anotherapp1",
            "name": "anotherapp",
            "port": 8181
        },
        "status": "UP"
    }
]

$ http GET :8762/apps/anotherapp
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Date: Mon, 20 Jun 2016 22:41:27 GMT
Server: Apache-Coyote/1.1
Transfer-Encoding: chunked
X-Application-Context: application:8762

[
    {
        "application": {
            "hostname": "localhost",
            "instance_id": "anotherapp1",
            "name": "anotherapp",
            "port": 8181
        },
        "status": "UP"
    }
]

$ http GET :8762/apps/myapp/app1
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Date: Mon, 20 Jun 2016 22:44:11 GMT
Server: Apache-Coyote/1.1
Transfer-Encoding: chunked
X-Application-Context: application:8762

{
    "application": {
        "hostname": "localhost",
        "instance_id": "app1",
        "name": "myapp",
        "port": 8081
    },
    "status": "UP"
}

```

## Todo

- [X] Register
- [X] Heartbeat
- [X] Delete
- [X] List
- [X] @EnableEurekaLite
- [ ] Tests
- [ ] Remove Eureka Client
- [ ] Move to Spring Cloud project layout
