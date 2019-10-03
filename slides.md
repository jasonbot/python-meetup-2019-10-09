---
theme: "white"
title: "Preparing for Microservices"
---

# Preparing for Microservices

Jason Scheirer

---

## The Unix philosophy

* Small tools, interconnected

---

## Microservices

* Small pieces, interconnected

---

## A Typical "Fat" App



---

## A Typical Microservice Mesh

---

## Microservices = lightweight

* Make them easy to spin up/down
* Create a cookiecutter template for a bare minimum functional service
* Run frequent integration tests to make sure it still works

---

# Moving to Microservices

---

## Interfaces, not entry points

* Think in terms of shared behavior handled by cooperating machines
* Networked endpoints, not local APIs

---

## Find Shared Functionality

* A handful of 

---

## Untangle The Ball of Yarn

* If you have a legacy app, circular dependencies and weird crufty bits become apparent quickly
* Pick a small piece at a time and forklift it out
* Going polyglot _can_ keep you honest
  * If your API calls