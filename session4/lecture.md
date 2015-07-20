Mesosphere Kubernetes Bootcamp
=======

Session 4
----

End-to-End Example



Learning Objectives
====

* Deploy two sample applications from Google to GKE.
* Walk through the gcloud and kubectl commands used.
* Find the Kubernetes documentation for reference.
* Spend the rest of class deploying and playing with another application.



Getting GKE Working
====

We'll follow this:

https://cloud.google.com/container-engine/docs/before-you-begin



Required Commands!
====

You must run these to really get GKE:

    gcloud components update beta
    gcloud components update kubectl
    gcloud config set project k8s-bootcamp
    gcloud config set compute/zone us-central1-f



Kubernetes Podwan Level 1
====

Get it? Podwan?

Anyway we'll put your Node.js application online.



Push To Gcloud
====

    gcloud compute ssh my-docker-vm --zone us-central1-f
    gcloud docker push gcr.io/<google-project-name>/hello-node



Create A Container Cluser
====

    gcloud beta container clusters create hello-world \
        --num-nodes 1 \
        --machine-type g1-small

    gcloud compute instances list



Now It's Kubernetes Time
====



Create Your Pod
====

We need a pod, and an image, which is your hello-node image.

    kubectl run hello-node \
        --image=gcr.io/k8sbootcamp/hello-node --port=8080



Put It On The Internet
====

You can also use gcloud to expose ports, but it sometimes doesn't work:

    kubectl get nodes

Get the part up-to-and-including the -node



Firewall Rules
====

    gcloud compute firewall-rules create hello-world-8080 \
        --allow tcp:8080 \
        --target-tags <node-name-prefix>
    kubectl expose rc hello-node --create-external-load-balancer=true
    kubectl get services hello-node



Visit It
====

Visit the second IP address at port 8080.



Review The Steps
====

1. Craft a Docker *image*.
2. Push it to google's registry with *google docker push*.
3. Create a container cluster (not compute cluster).
4. Use *kubectl* to run the image as containers in the pod.
5. Enable access in the GUI.



All Commands Used
====

    gcloud docker push gcr.io/<google-project-name>/hello-node
    gcloud beta container clusters create hello-world \
        --num-nodes 1 \
        --machine-type g1-small
    gcloud compute instances list
    kubectl run hello-node --image=gcr.io/<cloud-project-name>/hello-node --port=80



Firewall Commands
====

    kubectl get nodes
    gcloud compute firewall-rules create hello-world-8080 \
        --allow tcp:8080 \
        --target-tags <node-name-prefix>
    kubectl expose rc hello-node --create-external-load-balancer=true
    kubectl get services hello-node



Where These Are Documented
====

https://cloud.google.com/sdk/gcloud/

https://cloud.google.com/container-engine/docs/kubectl/



Command Line Help
====
    
    gcloud help
    kubectl help




Kubernetes Podwan Level 2
====

Next is how to make persistent disk storage for databases and such.

We'll make two compute nodes with persistent disk.

One for wordpress, one for MySQL.



Compute Nodes vs. Pod Nodes
====

GCloud has compute nodes.

Kubernetes though has pods that run on nodes.

Connected but different concepts.



Create The Cluster
====

    gcloud beta container clusters create wppd --num-nodes 2



Create The MySQL Disk
====

    gcloud compute disks create --size 200GB mysql-disk



Create The Wordpress Disk
====

    gcloud compute disks create --size 200GB wordpress-disk



YAML Kubernetes Configuration
====

In the git repo:

    cd session4/code/wppd/
    ls



Viewing The MySQL YAML
====

    less mysql.yaml
    less mysql-service.yaml



Deploying The MySQL YAML
====

    kubectl create -f mysql.yaml
    kubectl get pod mysql
    kubectl create -f mysql-service.yaml
    kubectl get service mysql



View And Deploy Wordpress
====

    less wordpress.yaml wordpress-service.yaml
    kubectl create -f wordpress.yaml
    kubectl get pod wordpress
    kubectl create -f wordpress-service.yaml
    kubectl get service wpfrontend



Expose Wordpress
====

    kubectl get nodes
    gcloud compute firewall-rules create wppd-80 --allow tcp:80 \
        --target-tags gke-wppd-XXX-node
    kubectl get services wpfrontend



End Of Quick Introduction
====



Kubernetes Jedi In Training
====

You've deployed from a docker and from YAML configurations.



End Of Lecture 
=====

Questions?

