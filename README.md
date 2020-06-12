# Logging-a-Kubernetes-Cluster-using-ELK-Stack

I) We would be using **Fluentd** (Similar to Logstash [Acronym for "L"]) for gathering logs in cluster and it would be on a independent pod as a container.

II) We would use **Elasticsearch** (Acronym for "E") for storing the data on its database. Based on Apache lucene.

III) We would use **Kibana** (Acronym for "K") is used for visualizing data using graphs,charts,etc.

We would deploy all the three components on k8s cluster, as each component of ELK as a pod. We would install Fluentd on everynode as a Pod, and Elasticsearch on two pods and Kibana on one pod. Fluentd is needed on three nodes, as we have three nodes (see kops installation) and we need logs from each node. We are using Elasticsearch on two nodes(using Replicasets), as it is important element for storage so if one node down, than it would work on another.

## Reference Links:

1) https://github.com/kubernetes/kubernetes/tree/master/cluster/addons/fluentd-elasticsearch (They have divided each component as service and workload file)

Note: You can apply the above repo files directly, but we have combined the six files into two files, and add a AWS cloud SSD drive as storage instead of local folder in cluster and labeled it as "cloud-ssd" to match Persistent Volume in our case. (See persistent volume section code for more info) and also added a Service output to LoadBalancer so that we can access instead of ClusterIP on original code.

2) https://www.udemy.com/course/kubernetes-microservices/


## Steps:

1) Apply the microservice application files.

2) Apply both the two files : fluentd-config.yml and elastic-stack.yml using ```kubectl apply -f .``` inside the directory of files.

3) To observe the pods are applied or not : ```kubectl get all -n kube-system```
Note: The above pods are created in kube-system namespace instead of default namespace.

You can access the Kibana homepage by going into AWS Load balancer and selecting the right LB with correct Listner Port. Ex= 5601

----------------------------------------------------------------------------------------------------------------------------------------

## Kibana Usage :

*The steps vary based on Kibana version installed*

1) Go to `setup up index patterns` on the top left corner of kibana homepage
Note: There would be a index created on kibana named "Logstash" and it will increment based on the date.
We would use wildcard `logstash*` inside step 1 of defining index pattern and next we need to define time field in logs, hence we would select `@timestamp` in step 2 and last step `create index pattern`.

2) Visit the `Discover` option and you will see all data of **Elasticsearch**. You can search keywords related to the logs like `errors` on the search bar. You can filter the refresh rate by `Auto-Refresh` and `last 15 mins` options on the top right corner.

3) You can populate the log details by clicking `below arrow`

4) You can filter the logs by clicking on one of the `Available Fields` on the bar on the left and click on the `+ maginifier` to add it as a filter.

5) For learning you can delete/ reduce the replica to 1 of a pod and monitor Kibana Dashboard for changes.

----------------------------------------------------------------------------------------------------------------------------------------
## Creating Kibana Dashboards ##

1) You can save a search/keyword from logs Ex="Queue Unavailable" and press `Save` to save the search results. It would be useful for creating dashboards.

2) Than you can visit the `Visualize` section and `Create a Visualization` (Ex= Graphs,Tables) and selecting the `Search Result` 

You can edit the Gauge Range by clicking `Options` and changing the range. You can Save the Gauge Visualization by Clicking `Save` after editing the ranges.

3) Go to `Dashboard` and `Add` and add the Gauge Visualization you saved earlier.

