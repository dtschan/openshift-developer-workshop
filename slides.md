---
title: OpenShift Developer Workshop
---

<!--section -->
# OpenShift Developer Workshop

<!-- .slide: class="master02" -->

---

## Agenda

* Container Requests and Limits
* Metrics, inkl. Java
* Application Monitoring
* Host settings

---

## Objectives


---

<!--section -->
## Container Requests and Limits

* **Request**: reserves cpu or memory for a container
* **Limit**: limits cpu or memory of a container
* Pod requests = sum of container requests
* Pod limits = sum of container limits

---

## Quality of Service

Requests and limits determine quality of service:

* No request and limit: **BestEffort**
* Request < Limit: **Burstable**
* Request = Limit: **Guaranteeed**

---

## Container exceeds memory limit

* OOM killer kills the container.
* OpenShift restarts container according to its restart policy.

---

## Node runs out of memory
* OpenShift evicts pods till enough memory has been freed.
* BestEffort pods are killed first, then Burstable pods.
* Guaruanteed pods are newer evicted because of other pods.
* The replication controller, if any, starts replacement pods.

---

## Best Practices

* Set **request = limit** for production containers to prevent eviction
* Set **1/2 limit <= request < limit** for non-production containers to optimize resource usage

---

## Best Practices

Set process limit based on container limit

* Depends on technology
* E.g. for Java set heap size = 1/2 limit
* Quality images already implement this, e.g.: [OpenShift Java Image](https://developers.redhat.com/blog/2017/02/23/getting-started-with-openshift-java-s2i/)

## Java Console

https://docs.openshift.com/container-platform/3.5/architecture/infrastructure_components/web_console.html#jvm-console


## App Monitoring

https://twiki.puzzle.ch/bin/view/Puzzle/AppMonitoring
