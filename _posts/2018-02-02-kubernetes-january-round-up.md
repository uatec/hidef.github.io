---
title: "Kubernetes January Round Up"
author: Robert Stiff
layout: post
tags: ["kubernetes"]
date: 2018-02-02
---

The first month of the year is gone and I have seen some interesting things come across my plate this year. So, I've pulled them together in case anyone else is interested.

<!--more-->

## Operating Kubernetes

### Provisioning

#### kops
[https://github.com/kubernetes/kops](https://github.com/kubernetes/kops)

Part of the Kubernetes project itself, kops targets AWS and GCE, allowing you to spin up a production grade cluster in minutes. It is well documented and well supported, which makes it one of the best tools I have seen. Unfortunately it stores configuration and keys in S3 and requires access to them at runtime, which would make it hard to create push-button deployments for test clusters. That said, if you want to run your own cluster in AWS, this should probably be your first port of call.

#### Kubicorn
[https://github.com/kris-nova/kubicorn](https://github.com/kris-nova/kubicorn)

Although not an official part of the Kubernetes project, Kubicorn aims to make it easier to provision clusters on more providers than just AWS and GCE, while giving your more flexibility. If you want to spin up a test cluster on A.N.Other cloud provider, this is probably what you want. Be aware though, it is not yet rated production ready, as it is lacking some key aspects, including depth of documentation.

### Friendly DNS
[https://github.com/kubernetes-incubator/external-dns](https://github.com/kubernetes-incubator/external-dns)

Kubernetes provides it's own DNS service, but this is intended entirely for service discovery, with meaningful but ugly DNS names like `my-svc.my-namespace.svc.cluster.local` or `pod-ip-address.my-namespace.pod.cluster.local`. If you want to expose a site or API to users, use external-dns to provision vanity URLs for services or ingress. It supports a number of providers including Route53, AzureDNS, CloudFlare and, once the controller has been deployed, you only need to decorate your services to make us of it.

### Custom DNS
[https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)

As mentioned, Kubernetes' DNS service is used for internal service discovery. If you have your own private DNS that you want to integrate with, you can easily extend Kubernetes DNS with your own DNS as a fallback.

## Developing For Kubernetes

### Service Meshes

#### Linkerd
[https://linkerd.io/](https://linkerd.io/)

Tried and tested by the likes of Monzo, Linkerd can transparently handle retries, time outs, rate-limiting, circuit breaking and the like, removing the need for tangled resilience implementations with Polly.net or custom code. It even has multiple service discovery plugins, for example using consul to resolve service hosts directly, removing the need for internal load balancers.

#### Istio
[https://istio.io/](https://istio.io/)

More than a single service, Istio is an entire service mesh project. Combining resilience, service discovery, load balancing etc. with a TLS secured mesh, capable of being extended outside of your cluster. It's not as mature as Linkerd yet, but it has lofty goals and broad community support. One to watch.

### AWS ELB Config Options
[https://gist.github.com/mgoodness/1a2926f3b02d8e8149c224d25cc57dc1](https://gist.github.com/mgoodness/1a2926f3b02d8e8149c224d25cc57dc1)

We live in an AWS world. Most AWS configurations provision ELBs to expose services outside the cluster. Here is a handy cheat sheet for configuring your load balancer, e.g. internal only, SSL certs, time outs.

### Docker for Mac /w Kubernetes
[https://blog.alexellis.io/docker-for-mac-with-kubernetes/](https://blog.alexellis.io/docker-for-mac-with-kubernetes/)

Now Docker for Mac has Kubernetes support built in Alex Ellis, author of OpenFaas, compares it to some of the other options for Kubernetes developer tooling, and a quick view of running OpenFaas (a kubernetes based, vendor agnostic, alternative to Lambda)

### Docker for Windows /w Kubernetes
[https://goo.gl/LUTFjS](https://goo.gl/LUTFjS)

Thanks to Mr. Hanselman for running through the new release of Docker for Windows with one-click Kubernetes built in, as well as a flying example of the Kubernetes Dashboard, and deploying ASP.NET Core in to Kubernetes. 

---

If you want to contribute anything to this list, or my next round up, drop me an email at [roundup@hidefsoftware.co.uk](mailto:roundup@hidefsoftware.co.uk).