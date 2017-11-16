---
ID: 1723
post_title: Current work on Cloud Dataverse
author: thakuran
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/current-work-on-cloud-dataverse/
published: true
post_date: 2016-10-04 17:28:18
---
<p style="text-align: right">-by <a href="http://massopen.cloud/blog/anuj-thakur/">Anuj Thakur</a></p>
<p style="text-align: left">This blog talks about the work that is going on the Cloud Dataverse project. The Cloud Dataverse is the Dataverse installation on MOC OpenStack VM. This installation has the capability to harvest metadata and replicate the data files from a OAI Set of any Dataverse installation. To do so you need to follow the guide for creating a OAI Client and run the normal harvest. At the end of this harvest the data files for every datasets is stored locally. Locally in this scenario is defined as the storage area for the OpenStack VM i.e the Swift Service endpoint of MOC production environment.</p>
<p style="text-align: left">The immediate work is being done on improving the performance of the upload functionality and on the concept of "Cloud enabled dataset". The Cloud enabled dataset will be marked cloud enabled and the back-end will reflect the same. Once a dataset is cloud enabled it will be included into a OAI set created specifically for replication of the datasets to Cloud Dataverse installation. The Cloud Dataverse installation will synchronize for the changes that are made on the Dataverse installation side every time the harvest is run.</p>