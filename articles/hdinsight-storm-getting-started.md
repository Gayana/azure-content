<properties 
   pageTitle="Getting started using Apache Storm on HDInsight | Azure" 
   description="Learn how to use  Apache Storm to process data in realtime with Storm on HDInsight" 
   services="hdinsight" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="02/18/2015"
   ms.author="larryfr"/>


#Getting started using Storm on HDInsight (Hadoop)

Apache Storm is a scalable, fault-tolerant, distributed, real-time computation system for processing streams of data. With Storm on HDInsight, you can create a cloud-based Storm cluster that performs data analysis in real-time.

##Before you begin

You must have the following to successfully complete this tutorial.

* An Azure Subscription

##Create an Azure Storage account

Storm on HDInsight uses Azure Blob Storage for storing log files and topologies submitted to the cluster. Use the following steps to create an Azure Storage account for use with your cluster.

1. Sign in to the [Azure Management Portal](http://manage.windowsazure.com/).

2. Click **NEW** on the lower left corner, point to **DATA SERVICES**, point to **STORAGE**, and then click **QUICK CREATE**.

	![Azure portal where you can use Quick Create to set up a new storage account.](./media/hdinsight-storm-getting-started/HDI.StorageAccount.QuickCreate.png)

3. Enter **URL**, **LOCATION** and **REPLICATION**, and then click **CREATE STORAGE ACCOUNT**. Do not select an affinity group when creating storage for HDInsight. You will see the new storage account in the storage list.

	>[AZURE.NOTE]  The quick-create option to provision an HDInsight cluster, like the one we use in this tutorial, does not ask for a location while provisioning the cluster. Instead, it by default co-locates the cluster in the same data center as the storage account. So, make sure you create your storage account in the locations supported for the cluster, which are:  **East Asia**, **Southeast Asia**, **North Europe**, **West Europe**, **East US**, **West US**, **North Central US**, **South Central US**.

4. Wait until the **STATUS** of the new storage account is changed to **Online**.

For more information on creating Storage Accounts, see
<a href="../storage-create-storage-account/" target="_blank">How to Create a Storage Account</a>.

##Provision a Storm cluster on the Azure portal

When you provision an HDInsight cluster, you provision Azure compute resources that contains Apache Storm and related applications. You can also create Hadoop clusters for other versions using the Azure portal, HDInsight PowerShell cmdlets, or the HDInsight .NET SDK. For instructions, see [Provision HDInsight clusters using custom options][hdinsight-provision]. For information about different HDInsight versions and their SLA, see [HDInsight component versioning](http://azure.microsoft.com/documentation/articles/hdinsight-component-versioning/) page.

[WACOM.INCLUDE [provisioningnote](../includes/hdinsight-provisioning.md)]

1. Sign in to the [Azure Management Portal][azureportal]

2. Click **HDInsight** on the left, and then **+NEW** in the lower left corner of the page.

3. Click on the HDInsight icon in the second column, and then select **Quick Create**.

	![quick create](./media/hdinsight-storm-getting-started/quickcreate.png)

4. Enter a unique **Cluster name**, and a unique **Password** for the admin account. Also select the **Storage Account** created previously.

	Select a **Cluster Size** of **1 Data Node** to use for this cluster. This is to minimize the cost associated with the cluster. For production use, you would create a larger cluster.

	> [AZURE.NOTE] The administrator account for the cluster is named **admin**. The password you enter is the password for this account. You will require this information to perform actions with the cluster, such as submitting or managing Storm topologies.

5. Finally, select the **Checkmark** beside **Create HDInsight cluster** to create the cluster.

> [AZURE.NOTE] Cluster provisioning takes some time, usually under 15 minutes, to create the cluster, configure software, and install sample data and topologies. 

##Using Storm on HDInsight

Each Storm on HDInsight cluster comes with the Storm Dashboard, which can be used to upload and run Storm topologies on the cluster. Each cluster also comes with sample topologies that can be run directly from the Storm Dashboard

###<a id="connect"></a>Connect to the dashboard

The dashboard is located at **https://&lt;clustername>.azurehdinsight.net//**, where **clustername** is the name of the cluster. You can also find a link to the dashboard at the bottom of the Azure Portal page for your cluster.

![Azure Portal with Storm Dashboard link](./media/hdinsight-storm-getting-started/dashboard-link.png)

> [AZURE.NOTE] When connecting to the dashboard, you will be prompted to enter a user name and password. This is the administrator name (**admin**) and password used when you created the cluster.

Once the Storm Dashboard has loaded, you will see the **Submit Topology** form.

![Storm Dashboard with submit topology form](./media/hdinsight-storm-getting-started/submit.png)

The **Submit Topology** form can be used to upload and run Jar files containing Storm topologies. It also includes several basic samples that are provided with the cluster.

###<a id="run"></a>Run the wordcount sample

The samples provided with the cluster include several variations of a word counting topology. These samples include a **spout** that randomly emits sentences, and **bolts** that break each sentence into individual words, then count how many times each word has occurred. These samples are from the <a href="https://github.com/apache/storm/tree/master/examples/storm-starter" target="_blank">Storm Starter samples</a>, which are a part of Apache Storm.

Perform the following steps to run one of the samples.

1. Select **StormStarter - WordCount** from the **Jar File** dropdown. This should populate the **Class Name** and **Additional Parameters** fields with the parameters for this sample.

	![Storm Dashboard with wordcount selected](./media/hdinsight-storm-getting-started/submit.png)

	* **Class Name** - the class in the Jar file that submits the topology
	* **Additional Parameters** - any parameters required by the topology. In this example, it is used to provide a friendly name for the submitted topology

2. Click the **Submit** button. After a moment, the **Results** field will display the command used to submit the job, as well as the results of the command. The error field will display any errors that occur when submitting the topology.

	![Submit button and results](./media/hdinsight-storm-getting-started/submit-results.png)

	> [AZURE.NOTE] The results do not indicate that the topology has completed - **a Storm topology, once started, runs until you stop it.** The WordCount topology will generate random sentences, and keep a count of how many times it encounters each word, until you stop it.

###<a id="monitor"></a>Monitor the topology

The Storm UI can be used to monitor the topology.

1. Select **Storm UI** from the top of the Storm Dashboard. This will display summary information for the cluster and all running topologies.

	![storm ui showing topology summary](./media/hdinsight-storm-getting-started/stormui.png)

	From the page above, you can see the time the topology has been active, as well as the number of workers, executors, and tasks being used.

	> [AZURE.NOTE] The **Name** column contains the friendly name supplied earlier using the **Additional Parameters** field.

4. Under **Topology summary**, select the **wordcount** entry in the **Name** column. This will display more information about the topology.

	![storm ui](./media/hdinsight-storm-getting-started/topology-summary.png)

	This page provides the following information.

	* **Topology stats** - basic information on the topology performance, organized into time windows

		> [AZURE.NOTE] Selecting a specific time window changes the time window for information displayed in other sections of the page.

	* **Spouts** - basic information about spouts, including the last error returned by each spout

	* **Bolts** - basic information about bolts

	* **Topology configuration** - detailed information about the topology configuration

	This page also provides actions that can be taken on the topology.

	* **Activate** - resumes processing of a deactivated topology
	
	* **Deactivate** - pauses a running topology
	
	* **Rebalance** - adjusts the parallelism of the topology. You should rebalance running topologies after you have changed the number of nodes in the cluster. This allows the topology to adjust parallelism to compensate for the increased/decreased number of nodes in the cluster. For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parllelism of a Storm topology</a>
	
	* **Kill** - terminates a Storm topology after the specified timeout

5. From this page, select an entry from the **Spouts** or **Bolts** section. This will display information about the selected component.

	![storm ui](./media/hdinsight-storm-getting-started/component-summary.png)

	This page displays the following information.

	* **Spout/Bolt stats** - basic information on the component performance, organized into time windows

		> [AZURE.NOTE] Selecting a specific time window changes the time window for information displayed in other sections of the page.

	* **Input stats** (bolt only) - information on components that produce data consumed by the bolt

	* **Output stats** - information on data emitted by this bolt

	* **Executors** - information on instances of this component

	* **Errors** - errors that produced by this component

5. When viewing the details of a spout or bolt, select an entry from the **Port** column in the **Executors** section to view details for a specific instance of the component.

		2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
		2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
		2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
		2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
		2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
		2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
		2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
		2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957] 

	From this data you can see that the word **seven** has occured 1493957 times. That is how many times it has been encountered since this topology was started.

###Stop the topology

Return to the **Topology summary** page for the wordcount topology, then select the **Kill** button from the **Topology actions** section. When prompted, enter 10 for the seconds to wait before killing the topology. After the timeout period, the topology will no longer appear when you visit the **Storm UI** section of the dashboard.

##Summary

In this tutorial, you learned how to create a Storm on HDInsight cluster and use the Storm Dashboard to deploy, monitor, and manage Storm topologies.

##<a id="next"></a>Next steps

* **HDInsight Tools for Visual Studio** - The HDInsight Tools allow you to use Visual Studio to submit, monitor, and manage Storm topologies similar to the Storm Dashboard mentioned earlier. HDInsight Tools also provide the ability to create C# Storm Topologies, and includes sample topologies that you can deploy and run on your cluster.

	For more information, see <a href="../hdinsight-hadoop-visual-studio-tools-get-started/" target="_blank">Get Started using the HDInsight Tools for Visual Studio</a>.

* **Sample files** - The HDInsight Storm cluster provides several examples in the **%STORM_HOME%\contrib** directory. Each example should contain the following.

	* The source code - for example, storm-starter-0.9.1.2.1.5.0-2057-sources.jar

	* The Java docs - for example, storm-starter-0.9.1.2.1.5.0-2057-javadoc.jar

	* The example - for example, storm-starter-0.9.1.2.1.5.0-2057-jar-with-dependencies.jar

	Use the 'jar' command to extract the source code or Java docs. For example, 'jar -xvf storm-starter-0.9.1.2.1.5.0.2057-javadoc.jar'.

	> [WACOM.NOTE] Java docs consist of web pages. Once extracted, use a browser to view the **index.html** file.

	To access these samples, you must enable Remote Desktop for the Storm on HDInsight cluster, then copy the files from **%STORM_HOME%\contrib**.

* The following are other examples that can be used with Storm on HDInsight.

	* [Analyzing sensor data with Storm on HDInsight](/en-us/documentation/articles/hdinsight-storm-sensor-data-analysis)

	* [Trending hashtags on Twitter with Storm on HDInsight](../hdinsight-storm-twitter-trending/)

* To learn more about developing Storm topologies, see the following.

	* [Develop Java topologies for Apache Storm on HDInsight](../hdinsight-storm-develop-java-topology/)

	* [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](/en-us/documentation/articles/hdinsight-storm-develop-csharp-visual-studio-topology/)


[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: ../hdinsight-provision-clusters/
