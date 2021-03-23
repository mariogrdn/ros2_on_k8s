# ROS2 on Kubernetes

This repository contains the Dockerfiles, the YAML files and the Bash scripts necessary to deploy a simple ROS2 talker and listener system.
All the Docker images are already available on Docker Hub, but if the necessity arises to build them anew, it is fundamental to change the value of the "image" field in each of the YAML configuration files to the name of the new images.
The Bash scripts and the XML files are not meant to be used manually, since they are copied inside the respective Docker image and automatically used when the image starts running.
Those scripts are set as entrypoints for the Docker images, and are responsible of properly configuring the FastDDS instance running under the hood of ROS2 in order to make it work using a Discovery Server (https://github.com/mariogrdn/Discovery-Server), instead of its traditional multicast-based discovery mechanism. 

# Prerequisites

A running Kubernetes cluster.

# How To Use

In order to use this repository, all that is needed to do is to apply the YAML files into your Kubernetes cluster ("kubectl apply -f <yamlFileName>"). It is recommended, even though not strictly mandatory, to deploy the Discovery Server first and then, as soon as it is up and running (its state can be seen by means of "kubectl get pods". Adding the flag "--watch" will keep monitoring the state of the Pods in the default namespace), deploy the other in no particular order.

# Results

The results can be seen by watching the logs of both the talker and the listener Pod. To do so, we firstly need to see what names Kubernetes assigned to our new Pods, which can be done by executing "kubectl get pods". 
Now, all that remains is to execute the command "kubectl logs <podName> --follow" (the "--follow" flag is used to keep monitoring the logs without having to re-execute the command) on both the talker and the listener.
The expected behavior is:
  - The talker sends messages to the listener and prints to the log their number.
  - The listener receives messages from the talker and prints to the log their number.

We should see, by taking a look at both logs, that the messages are exactly the same. This proves that the two nodes are able to communicate and therefore the discovery mechanism based on the Discovery Server properly works.
