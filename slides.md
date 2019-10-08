---

theme: "white"
title: "Preparing for Microservices"
transition: "fade"
transitionSpeed: "fast"
highlightTheme: "github"

---

# Preparing for Microservices

Jason Scheirer

| | | 
| --- | --- |
| GH: | https://github.com/jasonbot |
| Web: | https://www.jasonscheirer.com |
| Slides: | https://git.io/JeW41 |

_(I'm hireable!)_

---

## Microservices Are (_Largely_) Equated with "Modern" Web Architecture Best Practices

* You can take the parts you like from this and apply them to what you've already got
* edge routing, service discovery, formal tool-specified specified interfaces, data store isolation are all associated with microservices but you can use them anywhere

---

## The Unix philosophy

* Small tools, interconnected by pipes
* Separation of concerns

---

## Microservices

* Small pieces, interconnected by interfaces
* Separation of concerns

---

| Monolith | Microservice |
| --- | --- |
| Same process space/permissions | Own process space/permissions |
| Redeploying the entire world | Redeploying small components |
| Touching a route touches _everything_ | Touching a route touches only one component |
| Horizonally scaling scales _everything_ | Horizontally scaling à la carte |
| One programming language | Multiple programming languages |
| Encourages one stop shopping | Encourages layers |

---

## A Typical "Fat" App Setup

![monolith](monolith.svg)

---

## A Typical Microservice Setup

![monolith](microservice.svg)

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
  * Changes to routes/policies don't require service redeploys

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

## What can we take away from this?

* Two sub-services (`/api/v1/service1` and `/api/v1/service2`) talk to `service1`
  * Forklifting
* It all looks like one single cohesive set of routes to the outside world
* If `service1` has `/internal/v1/service` it's hidden from the outside world but consumable in your cluster

---

# Thanks