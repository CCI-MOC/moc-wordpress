---
ID: 1648
post_title: Cloud Dataverse
author: thakuran
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/cloud-dataverse/
published: true
post_date: 2016-08-30 16:54:47
---
<p style="text-align: right">-  by <a href="http://massopen.cloud/blog/anuj-thakur/">Anuj Thakur</a></p>

<h3 style="text-align: justify"><strong>Background:</strong></h3>
<span style="font-weight: 400">First, some clarification of terms:</span>

<b>Dataverse Installation: </b>

<span style="font-weight: 400">This is an installation of the Dataverse software, that is a dataverse site serving as a data repository</span>

<b>A dataverse:</b>

<span style="font-weight: 400">A container of datasets. This is an object that has some a minimal set of metadata and can contain datasets or other dataverses. The Dataverse installation is the Root dataverse.</span>

<b>Dataset:</b>

<span style="font-weight: 400">An object that contains metadata (description of the data), access and usage rules (terms of use, permissions to access data), and the data files.</span>

<b>Data Files:</b>

<span style="font-weight: 400">The actual data associated with a dataset object.</span>

<b>Dataset versions:</b>

<span style="font-weight: 400">New versions at the dataset level, either because the metadata has changed or the files have changed. Dataverse does not support versions at the data file level (at this point)</span>

<b>Harvesting Metadata:</b>

<span style="font-weight: 400">Making a copy of the metadata from datasets of a Dataverse installation (A) to another Dataverse installation (B). The datasets from A become searchable in B, but from the search results the user is directed to the dataset landing page in A and the data files are in A.</span>

<span style="font-weight: 400">Cloud Dataverse adds a couple of features to the Metadata Export and Harvesting on Swift storage (4.5-export-harvest-swift) code base. The features are namely:</span>
<ol style="text-align: justify">
	<li><span style="font-weight: 400"> Caching of Data files on the swift in addition to the metadata for harvested datasets.</span></li>
	<li><span style="font-weight: 400"> Using MOC OpenStack environment user credentials to authenticate to the Keystone endpoint.</span></li>
</ol>
<h3 style="text-align: justify"><strong>What is Metadata Export and Harvesting?</strong></h3>
<p style="text-align: justify"><span style="font-weight: 400">Metadata Export will allow for federation and interoperability with other systems to help make Dataverse more widely and easily discoverable. Another key function of the export is not only to share metadata but to store it in the file system in a preservation format. Harvesting allows the metadata from datasets from another site to be imported so they appear to be local, though data files remain on the remote site. This makes it possible to access content from data repositories and other sites with interesting content as long as they support the protocols Dataverse uses. Additionally, harvesting allows for Dataverse installations and other repositories to share metadata with each other to create a data sharing community and provide more access to the datasets stored in each repository.</span></p>

<h3 style="text-align: justify"><strong>What does Cloud Dataverse do?</strong></h3>
<p style="text-align: justify"><span style="font-weight: 400">Cloud dataverse provides the existing Metadata Export and Harvesting functionality with additional features. Cloud dataverse in addition to harvesting metadata it also caches the data files. The data files are cached on Swift storage. The OpenStack user credentials are used to store the Datafile on Swift.</span></p>
<p style="text-align: justify"><span style="font-weight: 400">Keystone authentication is used to store data on Swift. Authentication is done using the OpenStack credentials for a user. These credentials are stored in the swift.properties file.</span></p>

<h3 style="text-align: justify"><strong>How it actually works:</strong></h3>
<p style="text-align: justify"><span style="font-weight: 400">The process of caching the data file begins after the metadata is harvested for the OAI set (this is a set of datasets allowed to be harvested). Each harvested datasets have a list of data file that has to be cached in the proces. The location of the data files can be retrieved by the member variable storage identifier. The storage identifier before caching holds the url to the data file on the harvesting server.</span></p>
<p style="text-align: justify"><span style="font-weight: 400">Using this Url the data file is downloaded from the harvesting server to the /tmp directory of the OpenStack VM instance. The storage identifier of the data file is changed to indicate the files new location. </span></p>
<p style="text-align: justify"><span style="font-weight: 400">The data file is then copied from the temporary directory to it’s permanent location. The permanent location of the data file, which in this case is Swift by default. After uploading the data file to the Object store service endpoint, the storage identifier is updated to the location of the data file on Swift i.e the Object store service endpoint of the MOC OpenStack environment. </span></p>
<p style="text-align: justify"><img class="aligncenter wp-image-1653" src="http://massopen.cloud/wp-content/uploads/2016/08/Dataverse-to-Cloud-Dataverse-interaction.jpg" alt="Dataverse to Cloud Dataverse interaction" width="600" height="777" /></p>
<p style="text-align: justify">In the figure above, the Dataverse Installation has a OAI Sever. OAI Server has different OAI sets with multiple Datasets each. When the Cloud Dataverse creates a OAI client for a particular OAI set of that OAI server. After the harvesting is completed all the data files for the datasets are present on the Swift service endpoint.</p>
<p style="text-align: justify"><a href="http://joss.javaswift.org/index.html"><span style="font-weight: 400">JOSS</span></a><span style="font-weight: 400">, a Java library for OpenStack storage i.e Swift, is used to communicate with the Swift endpoint. Two modes of authentication is supported by JOSS library, out of which </span><a href="http://joss.javaswift.org/authentication.html#keystone"><span style="font-weight: 400">Keystone authentication mechanism</span></a><span style="font-weight: 400"> is used to </span><a href="http://joss.javaswift.org/authentication.html#tenant-name"><span style="font-weight: 400">pass the tenant name</span></a><span style="font-weight: 400"> and authenticate. The container is created  and accessed after the authentication is done. For this mechanism the required details need to be added to the swift.properties file.</span></p>

<h3 style="text-align: justify"><strong>Performance:</strong></h3>
The process of caching the file on Swift storage endpoint from the Harvard Dataverse Installation took 65 minutes during the August 4th demo. This set of datasets was 5GB of size. Out of the 65 minutes time, 61-62 minutes was taken to upload the data files onto the Swift endpoint. Running the same deployment after Ceilometer was turned off in the OpenStack production environment at MOC took around 35-37 minutes for the same process for the same set of datasets.

Using the concept of java multithreading and running multiple threads to upload data files on Swift endpoint now the process takes 15-16 minutes. The code that does that needs to be added to the pull request after some clean-up.