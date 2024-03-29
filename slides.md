---

theme: "White"
customTheme : "style-tweaks"
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

## The Unix Philosophy

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

---

## A Typical "Fat" App Setup

![monolith](monolith.svg)

---

## A Typical Microservice Setup

![microservice](microservice.svg)

---

## Microservices = Lightweight

* Make them easy to spin up/down
* Create a cookiecutter template for a bare minimum functional service
* Run frequent integration tests to make sure it still works

---

## Interfaces, Not Entry Points

* Think in terms of shared behavior handled by cooperating machines
* Networked endpoints, not local APIs
* Swagger and gRPC are formal environments to do this
* Individual services talk communicate via interfaces; find each other via service discovery

---

## Not everything Is An HTTP Endpoint

* Long-running tasks
* Pub/sub producers/workers
* Streaming via Websockets/HTTP2/gRPC/Message Buses

---

# Moving to Microservices

---

## Just Because It's Popular Doesn't Mean It's Right For You

---

## It's Okay to Have a Monolith

---

# Untangle The Ball of Yarn

---

## Untangle the Ball of Yarn: Find the Big Knots

* Pick a small piece at a time and forklift it out
* Consume your views via a client rather than calling them directly or using their underlying data stores
* Going polyglot _can_ keep you honest
  * Helps find pain points/language-specific "magic"
* I've done this going from Node to Go

---

## Untangle the Ball of Yarn: Make a New Ball

* Find a piece of new functionality you want to spec out and make it a service
* Consume it via a client in your monolith and vice-versa via your interfaces
* You now have a prototype for moving legacy code to microservices (or not)
* I've done this going from Django to Flask

---

## Untangle the Ball of Yarn: Get Behind the Edge

* Use an edge proxy to get into your services from the internet
  * Traefik, Kong, OpenResty, Envoy
* Separation of concerns
  * TLS or HTTP2 outside of your services simplifies implementation
  * Changes to routes/policies don't require service redeploys

---

## Edge Routing: Proxying++
  * You can run code at the edge
  * Do things like authentication in a uniform way
  * Whitelist/blacklist malicious headers

---

Excerpt from an Envoy Config 

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

## What Can We Take Away From This?

* Two sub-services (`/api/v1/service1` and `/api/v1/service2`) talk to `service1`
  * Forklifting in action
* It all looks like one single cohesive set of routes to the outside world
* If `service1` has `/internal/v1/service` it's hidden from the outside world but consumable in your infrastructure

---

## So

* You don't need to go all in
* Microservices speed up development and deployment
  * Smaller parts: smaller footprints, less fear, smaller/more frequent deploys
* Try: write new functionality as new microservices
* Also try: move existing functionality out part-by-part to microservices
* Make it possible to transparently swap out old stuff with the new

---

# Thanks