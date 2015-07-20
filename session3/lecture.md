Mesosphere Kubernetes Bootcamp
=======

Session 3
----

Kubernetes Basics



Learning Objectives
====

* Orchestration basics (requirements, solutions)
* Kubernetes components (container, pod, label, service, network)
* Kubernetes infrastructure (master, etcd, etc.)
* GKE intro/overview
* Managing nodes, pods and services



Orchestration
====

It's orchestrating resources to match compute work loads.



What is Kubernetes
===

A deployment, scheduling, and management infrastructure for docker containers.



Kubernetes Components
====

* kubectl
* scheduler
* nodes
* kubelet
* pods
* containers
* services and labels?




Kubernetes Structure
====

Push these on your mental "stack":

* kubectl is your entry point to a kubernetes deployment.
* controller manager controls cluster-level functions
* schedulers talks to nodes to allocate resources
* nodes then host pods
* kube-proxy controls the network on nodes
* pods then have groups of docker containers in them
* docker containers share the same logical volume



Simplified Deployment Diagram
====

[Insert Diagram here]



The Kubernetes Stack
===

Let's now pop each component off your mental stack and describe it.



Containers
====

These are just Docker containers running inside Pods.



Pods
====

"A pod (as in a pod of whales or pea pod) corresponds to a colocated group of applications running with a shared context."

A Pod is a group of coordinating containers.

The smallest unit the Scheduler understands.



Nodes
===

Node are most closely associated with "machines on the network".



Services
====

?



Kube-Proxy
====

Each node runs a network proxy and load balancer the controls network access to services.



Scheduler
====

The scheduler then determines which Nodes have available resources and runs
Pods on them.



Controller Manager
====

Controls all other cluster level functions.



kubectl
====

This then talks to the REST endpoint (scheduler/controller manager) to make it do things.



Sequence Diagram
====

Here is a sequence diagram of a simple kubectl command:

[insert diagram here]



Docker
====

Kubernetes then leaves all of the containerization to Docker.



Review
====

* kubectl is your entry point to a Kubernetes deployment.
* controller manager controls cluster-level functions
* schedulers talks to nodes to allocate resources
* nodes then host pods
* kube-proxy controls the network on nodes
* pods then have groups of docker containers in them
* docker containers share the same logical volume



End Of Lecture 
=====

Questions?

