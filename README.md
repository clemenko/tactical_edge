---
title: Tactical Edge Reference Architecture
author: Andy Clemenko, @clemenko, andy.clemenko@rancherfederal.com
---

# Tactical Edge Reference Architecture

Rancher Federal field engineers get asked all the time about how to deploy at the edge. If we google "edge computing" we will get roughly 301,000,000 results that all same something different. Let use help define what the Tactical Edge is and how to leverage Kubernetes. The goal is increasing speed while decreasing complexity.

> **Table of Contents**:
>
> * [What is the Tactical Edge](#what_is_the_tactical_edge)
>   * [Difficulties with Tactical Edge](#Difficulties_with_Tactical_Edge)
> * [Hardware](#Hardware)
> * [OS](#OS)
> * [Software](#Software)
>   * [Registry](#Registry)
>   * [RKE2](#RKE2)
>   * [Longhorn](#Longhorn)
>   * [Rancher](#Rancher)
> * [Conclusion](#conclusion)

## What is the Tactical Edge

There is an interesting article from Software Engineering Institute and Carnegie Mellon University about [Cloud Computing at the Tactical Edge](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=28021). What is interesting is to see the need for "Tactical Edge" computing 10 years ago. The need for getting applications in the hands of the war fighter or first responder is incredible valuable. From the abstract:

>Handheld mobile technology is reaching first responders, disaster-relief workers, and soldiers in the field to aid in various tasks, such as speech and image recognition, natural-language processing, decision making, and mission planning.

Let's fast forward 10 years and it is amazing to see the amount of compute that can fit inside a coffee can. Case in point ASROCK Industrial has a very powerful [4X4 BOX-5800U](https://www.asrockind.com/en-gb/4X4%20BOX-5800U) computer in a package that is 4 inches by 4 inches by 2 inches. Not only is it tiny, but the power consumption is low enough to run on batteries for some time. Compute had gotten powerful enough and small enough to run big applications in the palm of your hand.

We are defining Tactical Edge as: A small case or backpack that can run a couple of nodes.

![lunchbox](img/lunchbox_close_sml.jpg)

Using this definition we can start to think about real world applications of such a cluster/box/backpack. The technique of "Moving Computation to Where the Data Lives" can be applied. In a lot of the use cases we have seen the organization wants to process data in the field. Then distill it into more meaningful data to be sent back to upstream. Now for some down sides of computing at the Tactical Edge.

### Difficulties with Tactical Edge

There are two major difficulties for computing at the Tactical Edge. The first one being power space and cool. This is difficulty for even the biggest data centers around the world. When looking for hardware pay some attention to the [Thermal Design Power](https://en.wikipedia.org/wiki/Thermal_design_power) or TDP for short. This will indicate how much power is required for that device. Remember that the power requirements will increase when the device is under heavy load.

The second major difficulty is manageability. How are we going to manage the Operating System and Applications far away from "home". There is where we can leverage Kubernetes and GitOps techniques to dramatically increase the velocity of deployment and manageability. We will walk through some of the choices further in this article.

Now that we have defined a few major difficulties we can start looking at the what a Tactical Edge Architecture would look like.

## Hardware

Boy do we have many choices on hardware these days. X86 or Arm? Intel or AMD? My favorite approach for picking hardware is to look at the applications that are needed. We call this approach "working backwards". Once the total amount of compute required is calculated we can start looking at the amount of CPU that will be needed. Adding the appliction CPU and Memory together with the Kubernetes and management overhead we get the "Compute Envelope". Meaning the total amount of CPU cores and Memory. There are other components, like networking, that are worth looking at.

For this reference architecture we have chosen three [ASROCK 4X4 BOX-5800U](https://www.asrockind.com/en-gb/4X4%20BOX-5800U) boards. There is a good balance of cores, memory, storage and TDP. Each board has 8 cores and support up to 64gb of ram. For storage there is NVME (PCIe Gen3x4) support and a SATA3 port. As for power, we were able to use a single 150w power supply for all three boards. Each board has a TDP of 60Watts. Under heavy load there were no issues at all. The boards also have multiple NICs, including a 2.5gb one.

To maximize the portability we added a [Gl.inet Beryl Travel Route](https://www.gl-inet.com/products/gl-mt1300/). The router is great for extending the connectivity to additional devices like a laptop. The router also has a "repeater" function for Wifi. Meaning all the nodes and reach the internet for updating and initial loading of software. And of course an internal DHCP server. As a side note, some of the Gl.iNet routes also have WireGuard (VPN) capabilities.

One last piece to this architecture is a [Netgear GS105](https://www.netgear.com/business/wired/switches/unmanaged/gs105/) five port 1 Gigabit ethernet switch. The switch provides communication between the nodes. The switch could easily be upgrade to 2.5 Gigabit if needed. Also, if was only two nodes the GL.iNet router would be able to handle all the internal traffic.

![lunchbox](img/lunchbox.jpg)

## OS

## Software

### Registry

### RKE2

### Longhorn

### Rancher

## Conclusion
