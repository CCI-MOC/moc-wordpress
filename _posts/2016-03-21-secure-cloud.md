---
ID: 746
post_title: Secure Cloud
author: piyanai
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/secure-cloud/
published: true
post_date: 2016-03-21 19:44:34
---
&nbsp;

[toc]
<h3><strong><span style="text-decoration: underline;">Overview</span></strong></h3>
Secure Cloud is a project within the Mass Open Cloud (MOC) that will leverage cryptography and secure hardware to provide guarantees about the state of resources within the cloud.  It will provide a robust, available bare-metal framework upon which clients can build their infrastructure with ‘mechanisms’ in place for them to meet their specific security requirements.  In the event of a national disaster, nodes in the cloud can be rapidly and securely reclaimed and reprovisioned into an isolated environment to be used for disaster response.

<hr />

<h3><strong><span style="text-decoration: underline;">Motivation</span></strong></h3>
The aim is to rapidly allocate and deallocate physical machines while providing assurance that the machines booted, and remain, in a known good state.  These machines will be provided in clusters, which remain isolated from the general cloud and can rapidly scale up or down depending on demand.  This provides infrastructure which is amenable to handling security critical computing in an open cloud setting.

<hr />

<h3><span style="text-decoration: underline;"><strong>Architecture</strong></span></h3>
Secure Cloud utilizes the Hardware Isolation Layer (<a href="http://massopen.cloud/blog/project-hil/">HIL</a>) and Bare Metal Imaging (<a href="http://massopen.cloud/blog/bare-metal-imaging/">BMI</a>) to perform the rapid allocation and deallocation of physical machines.  The open cloud framework will allow for heterogeneous architecture where tenants can obtain dedicated infrastructure to meet their specific needs.  Rapid allocation and deallocation of nodes will also allow for the reclamation of critical infrastructure in the case of an emergency.

While the current HIL infrastructure provides the base isolation needed for security-sensitive tenants, these tenants will need some way to securely distribute secrets (e.g., cryptographic keys) to the different nodes they control, to allow them to encrypt subsequent operations. MIT LL has developed an open-source bootstrapping and attestation infrastructure, which we plan to integrate and enhance for this purpose.

HIL and the attestation infrastructure are critical in order to allow tenants to be protected from other concurrently running tenants. However, we also need to make sure that no attacks are possible by malicious tenants compromising the firmware on a server and then impacting subsequent tenants. Nearly every device in the system has some sort of persistent programmable firmware, some of which can be measured into the TPM, and many of which are outside the view of the measurement process.  With commodity hardware and commercial UEFI firmware it is possible to measure the CPU’s firmware and perform remote attestation with signed TPM quotes, but this does not encompass the BMC’s firmware, nor the firmware in devices like the NICs, power supplies, disk drives, front panels, and so on.

The initial project plan is to use the features available in the off-the-shelf commodity systems (Intel Boot Guard’s “Measured Boot” functionality, discrete TPM quotes, signed Option ROMs and tboot’s TXT payload) to ensure that the CPU is running the code that it is supposed to be executing. This infrastructure can be built with existing hardware and relatively minor modification of the nodes’ boot process.  While the machines can’t be made entirely stateless due to the firmware issues, they will not have any local storage and will fetch their kernels over the network.

A parallel phase aims to replace the motherboard’s firmware with CoreBoot, an Open Source BIOS replacement formerly known as LinuxBIOS. Porting CoreBoot to the server architecture serves several purposes: it replaces millions of lines of closed source firmware with auditable open source, it allows security fixes to be applied immediately rather than waiting for vendors to fix them, it is significantly faster to boot, and it enables more flexible boot processes since the full power of Linux is available.  The Baseboard Management Controller (BMC) also has a significant amount of closed source firmware.  Replacing it with OpenBMC, a project from Facebook’s Open Compute Project, also allows more flexibility in managing the system.  The BMC firmware is currently un-measured and coming up with a solution to include it in the attestation is an open research project.

<hr />

<h3><span style="text-decoration: underline;"><strong>Project Team</strong></span></h3>
<span style="text-decoration: underline;"><strong>Core Project Team</strong></span>
<ul>
 	<li>Orran Krieger</li>
 	<li>Gene Cooperman</li>
 	<li>Kyle Hogan</li>
 	<li>Jason Hennessey</li>
 	<li>Gerado Ravago</li>
 	<li>Trammell Hudson (Two Sigma)</li>
 	<li>Nick Matsuura (USAF)</li>
 	<li>Patrick Cable (MIT Lincoln Labs)</li>
 	<li>Nabil Schear (MIT Lincoln Labs)</li>
</ul>
<span style="text-decoration: underline;"><strong>Contributors</strong></span>
<ul>
 	<li>Mayank Varia (Boston University)</li>
 	<li>Steve Orrin (Intel)</li>
 	<li>Scott Kelso (Lenovo)</li>
 	<li>BMI Team</li>
 	<li>HIL Team</li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Planning and Getting Involved</strong></span></h3>
For more information please contact <a href="mailto:moc_secure_cloud@googlegroups.com">moc_secure_cloud@googlegroups.com</a>