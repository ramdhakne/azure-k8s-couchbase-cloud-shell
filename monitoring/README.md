# Conguring Container Monitoring Solution

Configure monitoring of your AKS cluster using the [Containers solution for Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-containers)

Click on Create Resource and search icon pops up and enter "Container Monitoring Solution"
![Alt text](/monitoring/images/pic1.png?raw=true "Container Monitoring Solution")

Click on Create

![Alt text](/monitoring/images/CMS-1.png?raw=true "Container Monitoring Solution")

Create a new OMS workspace, or select an existing one. The OMS Workspace form guides you through this process.

![Alt text](/monitoring/images/pic3.png?raw=true "Container Monitoring Solution")

Click on Create New Workspace, enter relevant details, choose the same resource group which has the K8s cluster

![Alt text](/monitoring/images/pic4.png?raw=true "Container Monitoring Solution")

Click Ok in the end

Next step is to use the OMS workspace created in previous steps click on "create" for Container Monitor Solution pane

![Alt text](/monitoring/images/pic5.png?raw=true "Container Monitoring Solution")

It should look like below

![Alt text](/monitoring/images/pic6.png?raw=true "Container Monitoring Solution")

Once the workspace has been created, it is presented to you in the Azure portal.

![Alt text](/monitoring/images/CMS-2.png?raw=true "OMS Workspace")

Download the yaml file to create a deamon set

```monitoring/oms-yaml/oms-deamonset.yaml```

save the file as ```oms-deamonset.yaml```

The Log analytics Workspace ID and Key are needed for configuring the solution agent on the Kubernetes nodes.
To retrieve these values, Select OMS Workspace from the container solutions left-hand menu. 
Select Advanced settings and take note of the WORKSPACE ID and the PRIMARY KEY.

![Alt text](/monitoring/images/CMS-3.png?raw=true "OMS Primary Key")

Replace the placeholder values for WSID and KEY with your Log Analytics Workspace ID and Key

```kubectl create -f oms-deamonset.yaml```

```kubectl get -f oms-deamonset.yaml```

```kubectl get daemonset```

# Access Monitoring Data
Select the Log Analytics workspace that has been pinned to the portal dashboard. Click on the Container Monitoring Solution tile and click on overview. 
We should be able to see how many containers we are running in various states.

![Alt text](/monitoring/images/CMS-4.png?raw=true "Containers Dashboard")





