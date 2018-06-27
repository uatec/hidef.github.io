---
title: "Kubernetes February Round Up"
author: Robert Stiff
layout: post
tags: ["kubernetes"]
date: 2018-03-03
---

Another month is passed, and it seems like activity around Kubernetes is accelerating exponentially.

<!--more-->

### The New Stack

[https://thenewstack.io/ebooks/](https://thenewstack.io/ebooks/)

The New Stack has a collection of ebooks about adopting Kubernetes in an enterprise environment, covering topics including Monitoring, Security, Orchestration and several others.

### What makes a cluster a cluster

[https://coreos.com/blog/cluster-osi-model.html](https://coreos.com/blog/cluster-osi-model.html)

We deal with so many levels of abstraction that it can be hard to see how things should fit together. This interesting comparison with the OSI model can help get things in line.

## Developing with Kubernetes

### Debugging Crash Loop Backoffs

[https://medium.com/kokster/debugging-crashloopbackoffs-with-init-containers-26f79e9fb5bf](https://medium.com/kokster/debugging-crashloopbackoffs-with-init-containers-26f79e9fb5bf)

It can be infuriating when your container doesn't start, and doesn't even produce any logs. Here are some tips to help you figure out what's going on.

### Speed up development by mapping volumes

[https://github.com/vapor-ware/ksync](https://github.com/vapor-ware/ksync)

Sometimes the whole Code, Build, Test, Code cycle can be too onerous, especially when you're trying to get something working by trial an error. ksync helps you sync your local file system with a remote container to make live-changes. 

## Operating Kubernetes

### GKE vs AKS vs EKS

[https://blog.hasura.io/gke-vs-aks-vs-eks-411f080640dc](https://blog.hasura.io/gke-vs-aks-vs-eks-411f080640dc)

Managed Kubernetes from 3 of the biggest cloud providers. Information on EKS is sadly lacking.

### Paul Maddox - Amazon EKS

[https://blog.uprightvinyl.co.uk/2018/02/08/aws-builders-day-part-3-kubernetes-on-aws-with-amazon-eks/](https://blog.uprightvinyl.co.uk/2018/02/08/aws-builders-day-part-3-kubernetes-on-aws-with-amazon-eks/)

Addressing the lack of information about Amazon's Elastic Kubernetes System, this write up of a talk by Paul Maddox probably covers 90% of what I have seen about EKS.

### Kured

[https://github.com/weaveworks/kured](https://github.com/weaveworks/kured)

Manages node reboots safely, to facilitate things like host OS updates

### Providing IAM Roles to Containers

[https://github.com/jtblin/kube2iam](https://github.com/jtblin/kube2iam)

When coming from AWS services like EC2, where you choose the permissions you want and the relevant credentials are given to you, kube2iam brings this same pattern down in to your Kubernetes cluster.

### KubeAPI AWS Authentication

[https://github.com/heptio/authenticator](https://github.com/heptio/authenticator)

Initially Kubernetes only grants admin access to a single set of SSH keys. If you want to share API access with your team, and want to use AWS IAM users to manage it, this can help you out.
