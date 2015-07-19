Mesosphere K8S Bootcamp
=======

Session 2
----

Using Docker



Learning Objectives
====

* Create a Docker image
* Inspect a running Docker container 
* Find Docker images 
* Launch pre-defined Docker images



Crafting Our First Docker
====

We'll use the example Google provides for a simple Node.js app.




The Node.js Code
====

In the github repository do:

    cd session2/code/
    



Configure A Compute Instance
====

    gcloud compute instances create my-docker-vm \
    --image container-vm \
    --zone us-central1-f \
    --scopes storage-rw \
    --machine-type g1-small



Setup The Instance
====

    gcloud compute copy-files node-docker my-docker-vm:
    gcloud compute ssh my-docker-vm --zone us-central1-f
    cd node-docker
    sudo gcloud components update



Docker Commands For Managing Images
====
    docker build
    docker rmi
    docker images



Building An Image
====

    sudo docker build -t gcr.io/k8s-bootcamp/hello-node .



Exploring What You Made
====

    sudo bash
    docker images
    docker save -o hello-node.tar 059b9105cabc
    tar -tvf hello-node.tar
    mkdir temp
    cd temp
    tar -xvf ../hello-node.tar
    find . -name "*.tar" -exec tar -tvf {} \; | less


Docker Containers
====

How they differ from images.



Running It
====

    docker run --publish 80:8080 --name hello-node -d 059b9105cabc
    docker ps
    curl http://localhost/



Make it Accessible
====

Compute Engine > VM Instances > my-docker-vm > Allow HTTP Traffic

Go to External IP with browser.



Managing Containers
====

    docker ps
    docker top hello-node
    docker restart hello-node
    docker stop hello-node
    docker rm hello-node
    docker rmi hello-node



Docker Gotchas
====

* images vs. containers
* volumes are not cleaned up by default
* tools to remove volumes and manage them
* volumes eat up disk space



Review
====

* We crafted a docker image for a Node.js application.
* We inspected the image to see what is in it.
* We got it running and managed it.
* We did an exercise of using Docker Hub to do the image management.



End Of Lecture 
=====

Questions?

