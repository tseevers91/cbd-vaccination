# Use Case: World vaccination progress

## Database Design
The following images gives a slight overview about our database structure.
Mainly we are using four tables, which holds the complete data of the project.

The tables `states` and `vaccines` are preloaded with content to ensure that the application runs properly.

The remaining tables are feed with dynamic user generated batch calculated data, which are calculated and inserted via kafka and batch processing.

![Database Design](https://raw.githubusercontent.com/smertins27/cbd-vaccination/master/documentation/images/MySQL_Database.jpg)

## Generating data

The user generated data consists of `vaccinescode`, `statesiso` and `vac_amount` which are filled with information from a form.
To simulate the Big Data character and to stress the batch processing a hugh amount of data entries can be generated randomly by a Javascript function.  

```
{ 
	vaccinescode: 'bnt', 
	statesiso: 'NI',
	vac_amount: '1500',
}
```

## Prerequisites

### Required software
To develop and deploy the project in a local Kubernetes some software requirements needs to be installed.
This case shows the commands for macOS

Minikube: `brew install minikube`

Skaffold: `brew install skaffold`

Helm: `brew install helm`

Enable ingress `minikube addons enable ingress`

A running Strimzi.io Kafka operator

```bash
helm repo add strimzi http://strimzi.io/charts/
helm install my-kafka-operator strimzi/strimzi-kafka-operator
kubectl apply -f https://farberg.de/talks/big-data/code/helm-kafka-operator/kafka-cluster-def.yaml
```

A running Hadoop cluster with YARN (for checkpointing)

```bash
helm repo add stable https://charts.helm.sh/stable
helm install --namespace=default --set hdfs.dataNode.replicas=1 --set yarn.nodeManager.replicas=1 --set hdfs.webhdfs.enabled=true my-hadoop-cluster stable/hadoop
```

## Deploy

To develop using [Skaffold](https://skaffold.dev/), use `skaffold dev`. 
