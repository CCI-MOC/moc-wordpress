---
ID: 1181
post_title: Cloudlet
author: thakuran
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/cloudlet-anuj-thakur/
published: true
post_date: 2016-05-03 18:28:20
---
<p style="text-align: right">-By <a href="http://massopen.cloud/blog/anuj-thakur/">Anuj Thakur</a></p>
What is Cloudlet?

To state in very simple terms Cloudlet acts as the middle-tier between the Mobile and Cloud. This allows the mobile application's computation which was earlier done on the cloud to be done on the cloudlet.

What is the difference between Cloud and Cloudlet?

Cloudlet as opposed to a Cloud service is deployed one-hop away from the mobile device. This helps in reducing the latency and saves battery life of the mobile device.

Base VM:
1.     Constructed from standard builds or linux and windows 7.
2.    Instance is booted and paused, the resulting VM disk image and memory snapshot serves as the  base disk and base memory.
Launch VM:
1.    Base image + installing and configuring application’s back-end server binaries, and launches the back-end server.
2.    Instance resumed from this image will have the application in state ready to respond to offload requests. No reboot delay.
VM Overlay:
1.    Compressed binary difference between the launch VM image and its base VM image, produced using xdelta3 and LZMA

Figure 2 states -&gt; Mobile device delivers the VM overlay to a cloudlet that already possesses the base VM -&gt; The delivery can be either from the cloud or from the storage on the mobile device. -&gt; The cloudlet decompresses the overlay, applies it to the base the launch VM and then creates a VM instance from it. -&gt; The mobile device can now begin performing offload operations.

What does Openstack++ implement?
It implements the following features:
1.    Import Base VM
2.    Resume Base VM
3.    Create VM overlay
4.    VM Synthesis
5.    VM Handoff

Import Base VM
A single base VM is a zipped file composed of four files internally; a base disk image, a base memory snapshot, a hash value list for the disk image, a hash value list for the memory snapshot.
Further, since OpenStack treats each glance image as an independent entity, the disk image saves the UUIDs of all other associated files to connect all the participating files.

Resume Base VM

For Openstack Resuming a Base VM is analogous to instantiating a new VM instance using a VM snapshot. Therefore, instead of devising new API, modification to the original API for launching the new VM instance is made. At code level this message will arrive at the hypervisor driver class and be passed to the underlying virtualization. CloudletDriver is the hypervisor driver which inherits from the LibvirtDriver. CloudletDriver examines a new request and checks for the cloudlet flag, if the virtual disk image has a cloudlet flag then it resumes the selected base VM instead of starting a new VM instance.

Create VM Overlay

Creating VM overlay can be classified as the same category as reboot VM and resize VM, which applies a specific action to a running VM instance.
Using the existing action URL to declare a new action for creating a VM overlay and passes this command to the virtualization driver.
CloudletAPI, the inherits nova_rpc.ComputeAPI was added.

After this step the mobile application should be able to able to dynamically provision the back-end server at an OpenStack++ cluster by either directly transmitting a VM overlay file saved at the mobile device or by passing an URL of the VM overlay to the OpenStack++ cluster.

VM Provisioning

It is similar to OpenStack’s launch new VM instance at the OpenStack cluster. Therefore, OpenStack’s original VM creation API is leveraged upon and the OpenStack user sends a HTTP POST message to a specific URL.
The URL https://openstackaddr/v2.1/servers is sent a HTTP POST message with JSON payload which describes configurations for the new VM.
Special keyword ‘overlay_url’ is added to the metadata of the message. This keyword helps in differentiating a VM synthesis request from a regular VM creation request.

VM Handoff

VM handoff will migrate a running VM instance from one OpenStack cluster to another. The source OpenStack cluster needs permission to call the API of the destination OpenStack cluster.
New API extending the Action URL using OpenStack Extension. In the JSON payload of the HTTP POST message, https://openstack addr/v2.1/servers/server id/action, the handoff command and detail descriptions are added.

The code base is being shifted under <a href="http://openedgecomputing.org/">OpenEdgeComputing</a> project.

The different modules/repositories useful for the deploying the Cloudlet are:
<ol>
	<li>elijah-provisioning</li>
	<li>elijah-openstack</li>
	<li>elijah-discovery-basic</li>
</ol>
&nbsp;