---
ID: 1154
post_title: Ironic
author: thakuran
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/anuj-thakur-ironic/
published: true
post_date: 2016-04-19 16:27:13
---
<p style="text-align: right">- By <a href="http://massopen.cloud/blog/anuj-thakur/">Anuj Thakur</a></p>

<h3><strong>Introduction:</strong></h3>
Ironic is an OpenStack project which provisions physical hardware as opposed to virtual machines. Ironic provides several reference drivers which leverage common technologies like PXE and IPMI, to cover a wide range of hardware. Ironic’s pluggable driver architecture also allows vendor-specific drivers to be added for improved performance or functionality not provided by reference drivers.

OpenStack’s Ironic project makes physical servers as easy to provision as virtual machines in cloud, which in turn will open up new avenues for enterprises and service providers.

Ironic’s driver replaces the Nova “bare metal” driver (in Grizzly - Juno releases). Ironic is available for use and is supported by the Ironic developers starting with the Juno release. It is officially integrated with OpenStack in the Kilo release.

Ironic currently provisions servers only on flat networks hence providing no network isolation between tenants. Ironic should allow end users to utilize a baremetal instance in the same isolated (e.g. VLAN, VXLAN) networks as their virtual machines are. The spec that talks about this can be found <a href="https://specs.openstack.org/openstack/ironic-specs/specs/not-implemented/ironic-ml2-integration.html">here</a>. Other specs for Ironic is found on <a href="https://specs.openstack.org/openstack/ironic-specs/specs/">this</a> link.

<strong>Why provision Bare Metal:</strong>

Here are a few use-cases for bare metal (physical server) provisioning in cloud; there are doubtless many more interesting ones:
<ul class="simple">
	<li>High-performance computing clusters</li>
	<li>Computing tasks that require access to hardware devices which can’t be virtualized</li>
	<li>Database hosting (some databases run poorly in a hypervisor)</li>
	<li>Single tenant, dedicated hardware for performance, security, dependability and other regulatory requirements</li>
	<li>Or, rapidly deploying a cloud infrastructure</li>
</ul>
<strong>How Ironic replaces other Nova compute drivers:</strong>

Ironic replaces other Nova compute drivers(virtualisation and bare-metal). The change in the Nova configuration file(nova.conf) is made by replacing the compute_driver configuration variable value with Ironic driver.

Nova configuration using Libvirt compute driver:

[DEFAULT]

....

compute_driver=libvirt.LibvirtDriver

Nova configuration to use Ironic compute driver:

[DEFAULT]

...

compute_driver=nova.virt.ironic.IronicDriver

This makes the Nova service to use Ironic compute driver. The logical architecture of libvirt or Xen nova-compute driver is shown below:

<img class="" src="http://cloudgeekz.com/wp-content/uploads/2015/04/nova-logical.png" alt="http://cloudgeekz.com/wp-content/uploads/2015/04/nova-logical.png" width="630" height="418" />

&nbsp;

At a high level the steps to boot a nova virtual instance without any cinder block device looks like this:
<ul>
	<li>Authenticate with keystone</li>
	<li>Send boot request to Nova API</li>
	<li>Send boot request to Nova Scheduler to select a suitable compute host</li>
	<li>Send boot request to compute host to create virtual instance
<ul>
	<li>Get image from Glance</li>
	<li>Configure instance network using Neutron</li>
	<li>Start instance</li>
</ul>
</li>
</ul>
Now let us look at the Nova compute Ironic driver. The logical architecture of the Ironic nova compute driver looks like the following. Note how the hypervisor driver is replaced with the OpenStack Ironic Service.

<img class="transparent" src="http://cloudgeekz.com/wp-content/uploads/2015/04/ironic-logical-e1428861228820.png" alt="http://cloudgeekz.com/wp-content/uploads/2015/04/ironic-logical-e1428861228820.png" width="644" height="549" />

The Ironic service is composed of the following components:
<ol>
	<li>Ironic API – this is the REST API to interact with Ironic for managing bare metal hardware.</li>
	<li>Ironic Conductor – this is one of the key components of Ironic and is responsible for all the actions on the Bare Metal hardware.</li>
	<li>Ironic Drivers – Ironic supports pluggable back-end drivers for different hardware types so as to provide a mechanism to enable hardware specific features. Some of the Ironic drivers are listed below:
<ol>
	<li>IPMI &amp;PXE – commonly used driver for any hardware supporting IPMI &amp; PXE</li>
	<li>Seamicro – driver for SeaMicro servers.</li>
	<li>iLO – driver for HP servers with Integrated Lights Out Management Card</li>
	<li>DRAC – driver for Dell servers with Dell Remote Access Card</li>
</ol>
</li>
	<li>Ironic DB – this is used for storing information on conductor states, host details, drivers etc.</li>
</ol>
There are also some pre-requisites for using the Ironic driver for Bare Metal deployment of a host.
<ol>
	<li>Requires setting up of the nova-compute node with requisite packages – tftp server, dhcp, ipmi tool etc</li>
	<li>Nova flavors corresponding to specific hardware configuration needs to be created</li>
	<li>Deployment images need to be created and uploaded to Glance</li>
	<li>The Bare Metal host needs to be enrolled using the Ironic API.</li>
</ol>
At a high level the steps required to boot a Bare Metal host looks like this:
<ul>
	<li>Authenticate with keystone</li>
	<li>Send boot request to Nova API</li>
	<li>Send boot request to Nova Schedular to select the host for Bare Metal deployment</li>
	<li>Send boot request to the Bare Metal host
<ul>
	<li>Get the image from Glance (this is done by Ironic Conductor)</li>
	<li>Configure network using Neutron</li>
	<li>Set deploy request to Ironic API
<ul>
	<li>Deploy host – the deployment flow is based on the Ironic driver used.</li>
</ul>
</li>
</ul>
</li>
</ul>
<h3><strong>Ironic deploy process:</strong></h3>
<img class="alignnone size-full wp-image-1167" src="http://massopen.cloud/wp-content/uploads/2016/04/deployment_steps.png" alt="deployment_steps" width="1306" height="653" />
<div id="deploy-process" class="section">
<ol class="arabic">
	<li>
<p class="first">A boot instance request comes in via the Nova API, through the message queue to the Nova scheduler.</p>
</li>
	<li>
<p class="first">Nova scheduler applies filter and finds the eligible compute node. Nova scheduler uses flavor extra_specs detail such as ‘cpu_arch’, ‘baremetal:deploy_kernel_id’, ‘baremetal:deploy_ramdisk_id’ etc to match the target physical node.</p>
</li>
	<li>
<p class="first">A spawn task is placed by the driver which contains all information such as which image to boot from etc. It invokes the driver.spawn from the virt layer of Nova compute.</p>
</li>
	<li>
<p class="first">Information about the bare metal node is retrieved from the bare metal database and the node is reserved.</p>
</li>
	<li>
<p class="first">Images from Glance are pulled down to the local disk of the Ironic conductor servicing the bare metal node.</p>

<ol class="arabic simple">
	<li>For pxe_* drivers these include all images: both the deploy ramdisk and user instance images.</li>
	<li>For agent_* drivers only the deploy ramdisk is stored locally. Temporary URLs in OpenStack’s Object Storage service are created for user instance images.</li>
</ol>
</li>
	<li>
<p class="first">Virtual interfaces are plugged in and Neutron API updates DHCP port to support PXE/TFTP options.</p>
</li>
	<li>
<p class="first">Nova’s ironic driver issues a deploy request via the Ironic API to the Ironic conductor servicing the bare metal node.</p>
</li>
	<li>
<p class="first">PXE driver prepares tftp bootloader.</p>
</li>
	<li>
<p class="first">The IPMI driver issues command to enable network boot of a node and power it on.</p>
</li>
	<li>
<p class="first">The DHCP boots the deploy ramdisk. Next, depending on the exact driver used, either the conductor copies the image over iSCSI to the physical node (pxe_* group of drivers) or the deploy ramdisk downloads the image from a temporary URL (agent_* group of drivers), which can be generated by a variety of object stores, e.g. <em>swift</em>, <em>radosgw</em>, etc, and uploaded to OpenStack’s Object Storage service. In the former case, the conductor connects to the iSCSI end point, partitions volume, “dd” the image and closes the iSCSI connection.</p>
The deployment is done. The Ironic conductor will switch pxe config to service mode and notify ramdisk agent on the successful deployment.</li>
	<li>
<p class="first">The IPMI driver reboots the bare metal node. Note that there are 2 power cycles during bare metal deployment; the first time when powered-on, the images get deployed as mentioned in step 9. The second time as in this case, after the images are deployed, the node is powered up.</p>
</li>
	<li>
<p class="first">The bare metal node status is updated and the node instance is made available.</p>
</li>
</ol>
</div>
<h3><strong>Deploying Ironic using Devstack:</strong></h3>
<a href="http://docs.openstack.org/developer/ironic/dev/dev-quickstart.html#deploying-ironic-with-devstack" target="_blank">This</a> Openstack guide should be followed to deploy Ironic and other Openstack services using devstack. The guide provides a example local.conf(configuration file) which is used by devstack to enable Ironic and other Openstack services. The guide in the following section of the configuration file creates three VMs which poses as Ironic's Bare Metal nodes(this VM node creation can be skipped by commenting out this section).
<pre>[[local|localrc]]
...
# Create 3 virtual machines to pose as Ironic's Bare Metal nodes.
IRONIC_VM_COUNT=3
IRONIC_VM_SSH_PORT=22
IRONIC_Bare Metal_BASIC_OPS=True
DEFAULT_INSTANCE_TYPE=Bare Metal
IRONIC_DEPLOY_DRIVER_ISCSI_WITH_IPA=True</pre>
Other Ironic related configurations like the drivers that are enabled, the deployment driver, the minimum specs to create functional nodes is also specified in this configuration file. Parts of the configuration can be commented out if not required. The configuration file disables Nova network and enables the Neutron Openstack network service.
<h3><strong>Enable Ironic on an existing Openstack service:</strong></h3>
<a href="http://docs.openstack.org/developer/ironic/deploy/install-guide.html#install-and-configure-prerequisites" target="_blank">This</a> Openstack guide should be followed to deploy Ironic on an already existing Openstack deployment. This guide takes you through the following major steps that allows Openstack to use the Ironic Bare Metal driver as the Compute driver for Nova.
<ol>
	<li>Configuring Identity service for Bare Metal service.</li>
	<li>Setting up database for Bare Metal service.</li>
	<li>Installing the Bare Metal service.</li>
	<li>Configuring the Bare Metal service(i.e the ironic.conf file.
<ol>
	<li>Configuring for ironic-api service.</li>
	<li>Configuring for ironic-conductor service.</li>
	<li>Configuring ironic-api behind mod_wsgi.</li>
</ol>
</li>
	<li>Configure compute to use the Bare Metal service.</li>
	<li>Configure networking to communicate with Bare Metal nodes.</li>
</ol>
There are other steps you will find yourself performing to get Ironic service working like setting up the drivers for the Bare Metal service, etc.
<h3><strong>References:</strong></h3>
1. Openstack Ironic installation guide:- <a href="http://docs.openstack.org/developer/ironic/deploy/install-guide.html">http://docs.openstack.org/developer/ironic/deploy/install-guide.html</a>

2. Deploying Ironic using devstack:- <a href="http://docs.openstack.org/developer/ironic/dev/dev-quickstart.html" target="_blank">http://docs.openstack.org/developer/ironic/dev/dev-quickstart.html</a>

3. Ironic specifications:- <a href="https://specs.openstack.org/openstack/ironic-specs/specs/" target="_blank">https://specs.openstack.org/openstack/ironic-specs/specs/</a>

4. Introduction to Ironic:- <a href="http://docs.openstack.org/developer/ironic/deploy/user-guide.html" target="_blank">http://docs.openstack.org/developer/ironic/deploy/user-guide.html</a>

5. Ironic wiki:- <a href="https://wiki.openstack.org/wiki/Ironic" target="_blank">https://wiki.openstack.org/wiki/Ironic</a>

&nbsp;

&nbsp;

&nbsp;