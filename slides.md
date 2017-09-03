---
title: OpenShift Developer Workshop
---

<!--section -->
# OpenShift Developer Workshop

<!-- .slide: class="master01" -->

---

## Agenda

* Requests and Limits
* Application Monitoring

<!-- .slide: class="master02" -->

---

# Requests and Limits

<!-- .slide: class="master02" -->

---

## Pods

A pod is a group of containers which:

* Run on the same node
* Share the same lifecycle
* Share any persistent volumes
* Share the same IP address

---

<!--section -->
## Container and Pod Requests

* Reserve cpu and/or memory for a container
* Pod requests = sum of container requests
* Are used by the scheduler for pod placement
* Pods are not scheduled if no node can fulfill the requests

---

## Container and Pod Limits
* Limit cpu and/or memory of a container
* Pod limits = sum of container limits
* Containers which exceed memory limit are killed by OOM killer and subjected to restart policy
* Excessive CPU usage is throttled via cgroups

---

## Request and Limit Defaults

* No requests/limits: request/limit = limitrange defaults or unlimited
* Request only: limit = limitrange default or unlimited
* Limit only: request = limit

---

## Quotas

* Are optional
* Configured by cluster admin per project
* Limit number of objects that can be created
* Limit total cpu and memory that can be used

---

## Limit Ranges

* Are optional
* Configured by cluster admin per project
* Limit cpu and memory pods and containers can use
* Can provide default container request and limit

---

## Quality of Service

Requests and limits determine quality of service:

* No request and limit: **BestEffort**
* Request < Limit: **Burstable**
* Request = Limit: **Guaranteed**

---

## Out of Resource Handling

* OpenShift evicts pods till enough memory or disk space has been freed.
* BestEffort pods are killed first, then Burstable pods.
* Guaranteed pods are newer evicted because of other pods.
* Replication controller, if any, starts replacement pods.

---

## Best Practices

* Set **memory request = limit** for production containers to prevent eviction
* Set **1/2 memory limit <= request < limit** for non-production containers to optimize resource usage
* Set appropriate cpu requests and limits

---

## Best Practices

<div style="text-align: left"><p>Set process limit based on container limit:</p></div>

* Depends on technology
* E.g. for Java set heap size = 1/2 memory limit
* Quality images already implement this, e.g.: [OpenShift Java Image](https://developers.redhat.com/blog/2017/02/23/getting-started-with-openshift-java-s2i/)


---

# Application Monitoring

<!-- .slide: class="master02" -->

---

## Container Health Checks

* **Liveness probe**: Checks whether a container is still running.
* Non-live containers are killed and subjected to their restart policy.
* **Readiness probe**: Checks whether a container is ready to service requests.
* Non-ready containers don't receive traffic.

---

## Readiness Probes

* If no containers are ready users get a generic application unavailable message.
* Readiness probe implementation depends on application architecture.
* Also see following canary endpoints.

---

## Probe Types

* **HTTP check**: HTTP request against container, healthy if response code between 200 and 399.
* **Execution check**: Script runs in container, healthy if exit code is 0.
* **TCP socket check**: TCP connection against container socket, healthy if connection successful.

---

## App Monitoring

* Developer can see current memory and cpu usage in Web Console (metrics)
* JVM console is available with default images and **jolokia** container port
* Zabbix can monitor memory and cpu of containers and alert
* Zabbix can monitor canary endpoints and alert

---

## Canary Endpoints

* Simple health endpoint
* Performs no business activity
* Checks status and latency of dependencies, e.g. database

---

## Canary Endpoints

* Returns HTTP 200 when all checks are ok, 503 otherwise
* Other status codes, like 299, may be used to signal warnings
* May be used as readiness probe and for monitoring

---

## Resources

* [OpenShift Quotas and Limit Ranges](https://docs.openshift.com/container-platform/3.5/dev_guide/compute_resources.html)
* [OpenShift JVM Console](https://docs.openshift.com/container-platform/3.5/architecture/infrastructure_components/web_console.html#jvm-console)
* [OpenShift Application Health](https://docs.openshift.com/container-platform/3.5/dev_guide/application_health.html)
* [OpenShift Image Creation Guidelines](https://docs.openshift.com/container-platform/3.5/creating_images/guidelines.html)
* [Health Endpoint in API Design](http://byterot.blogspot.ch/2014/11/health-endpoint-in-api-design-slippery-rest-api-design-canary-endpoint-hysterix-asp-net-web-api.html)
* [Canary Endpoint Monitoring](http://uniknow.github.io/AgileDev/site/0.1.10-SNAPSHOT/canary-endpoint-monitoring)
