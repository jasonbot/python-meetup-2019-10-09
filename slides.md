---

theme: "white"
title: "Preparing for Microservices"
transition: "none"
transitionSpeed: "fast"
highlightTheme: "github"

---

# Preparing for Microservices

Jason Scheirer

_(I'm hireable!)_

---

## The Unix philosophy

* Small tools, interconnected by pipes
* Separation of concerns

---

## Microservices

* Small pieces, interconnected by interfaces
* Separation of concerns 

---

## A Typical "Fat" App Setup

---

## A Typical Microservice Setup

---

## Microservices = lightweight

* Make them easy to spin up/down
* Create a cookiecutter template for a bare minimum functional service
* Run frequent integration tests to make sure it still works

---

# Moving to Microservices

---

## Just because it's popular doesn't mean it's right for you

---

## It's okay to have a monolith

---

## Interfaces, not entry points

* Think in terms of shared behavior handled by cooperating machines
* Networked endpoints, not local APIs
* Swagger and gRPC are formal environments to do this

---

## Not everything Is An HTTP Endpoint

* Long-running tasks
* Streaming via Websockets/HTTP2/gRPC
* Message busses

---

# Untangle The Ball of Yarn

---

## Untangle the Ball of Yarn: Find the Big Knots

* If you have a legacy app, circular dependencies and weird crufty bits become apparent quickly
* Pick a small piece at a time and forklift it out
* Consume your views via a client rather than calling them directly or using their underlying data stores
* Going polyglot _can_ keep you honest
  * For example, Django models with lots of custom behavior won't interoperate with a Go service

---

## Untangle the Ball of Yarn: Make a new ball

* Find a piece of new functionality you want to spec out and make it a service
* Consume it via a client in your monolith and vice-versa  via your interfaces
* You now have a prototype for moving legacy code to microservices (or not)

---

## Untangle the Ball of Yarn: Get Behind the Edge

* Use an edge proxy to get into your services from the internet
  * Traefik, OpenResty, Envoy
* Separation of concerns
  * TLS or HTTP2 not on your service

---

```yaml
routes:
    - match:
        prefix: "/api/v1/service1"
        route:
            cluster: service1
    - match:
        prefix: "/api/v1/service2"
        route:
            cluster: service1
    - match:
        prefix: "/api/v1/service3"
        route:
            cluster: service3
```

---

# Thanks