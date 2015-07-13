Kubernetes Bootcamp
====

This is the initial repository for the learning materials to the Mesosphere Kubernetes bootcamp.
This bootcamp gives you an end-to-end understanding of Docker and how to orchestrate containers using Kubernetes. 

How To View The Lectures
====

The lecture slides are done with [reveal.js](http://lab.hakim.se/reveal-js/#/) and using their markdown features.
This means if you want to change the slides for session1 you would edit the file session1/lecture.md and *not* 
the file session1/index.html.

Because they use complx javascript from the index.html file you should run this command:

    python -m SimpleHTTPServer

That will start a basic HTTP server on the command line and then you can just go to [http://localhost:8000/](http://localhost:8000/) to start things off.


Current Proposal
====

This is the current outline for the course with expected times to teach each part.  Feel free to 
indicate any part that is not teachable on the GKE platform at this time.


Target Audience
----

* Familiar with Linux/shell
* Hands-on sessions via GKE 


Session 1: Container basics (30-1 hour)
----

* Understand container basics (cgroups, namespaces, chroot) 
* Understand Docker (images, registry, etc.)
* Mostly slides, some instructor-executed examples

Session 2: Using Docker (1 hour)
---

* Find Docker images 
* Launch pre-defined Docker images
* Create a Docker image
* Inspect a running Docker container 
* Logging
* Other topics (volumes, networking, etc.)
* Mostly hands-on by participants

Session 3: Kubernetes basics (lecture)
---

* Orchestration basics (requirements, solutions)
* Kubernetes components (container, pod, label, service, network)
* Kubernetes infrastructure (master, etcd, etc.)
* GKE intro/overview
* Managing nodes, pods and services

Session 4: End-to-end example (end of day)
---

* Architecture (suggestion: app server with node.js or Python/flask with Redis)
* Define/gather Docker images and wiring 
* Kubernetes deployment


Video Demo Wrap Up
---

* Kubernetes install on DCOS
* Kubernetes on DCOS examples
* Audience and Prerequisites
* Familiar with Linux/shell
* Hands-on sessions via GKE 


Instructors
====

Zed Shaw (Mesosphere)
Michael Hausenblas (Mesosphere)
Karl Isenberg (Mesosphere)
Tyler Neely (Mesosphere)
Aaron Bell (Mesosphere)



GKE Install Notes
====

* Everything works up to Hello Node at gcloud docker push gcr.io/<google-project-name>/hello-node
    * You need to run sudo gcloud components update beta first then this works
* resize is DEPRECATED and will be removed in a future version. Use scale instead.
* This kubectl stop hello-node causes error:
    error: you must provide one or more resources by argument or filename



