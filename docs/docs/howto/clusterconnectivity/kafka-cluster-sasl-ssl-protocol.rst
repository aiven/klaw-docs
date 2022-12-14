Connect to Apache Kafka cluster using SASL protocol
===================================================

This section provides you with information on how to connect Apache Kafka cluster to Klaw using SASL_SSL protocol. 

Prerequisite
------------
* Set up the connection between the Klaw APIs (Core API and Cluster API), see :doc:`klaw-core-with-clusterapi`. This involves configuring the ``klaw.clusterapi.url`` setting in the Klaw UI and testing the connectivity to ensure the two APIs can communicate.

Configure and connect using SASL protocol
-----------------------------------------

Follow the steps below to configure and connect to an Apache Kafka® cluster in Klaw using SASL_SSL protocol:

1. In the Klaw web interface, navigate to **Environments**, and click **Clusters**. 
2. On the **Clusters** page, click **Add Cluster**. 
3. On the **Add Kafka cluster** page, enter the following details: 

- **Cluster Type:** Select **Kafka** from the drop-down list
- **Cluster Name:** Provide a name for the cluster
- **Protocol:** Select SASL_SSL protocol for your cluster
- **Kafka Flavor:** Select Apache Kafka as the flavor
- **Bootstrap server:** Enter  the bootstrap servers details for an Apache Kafka cluster. 

4. Click **Save**. 
5. Add the cluster to the preferred environment. Click **Environments** from the **Environments** drop-down menu.
6. Click **Add Environment** and enter the details to add your Kafka environment. 
7. Enter an environment name, set the cluster you added from the drop-down list, and configure partitions and replication factor, and tenat (set to default). 
8. Copy the **Cluster ID** from the **Clusters** page using the copy icon.
9. Open the ``application.properties`` file located in the `klaw/cluster-api/src/main/resources` directory.
10. Depending on the SASL mechanism you are using, copy one of the below properties, replace ``clusterid`` with the copied cluster id, and save the ``application.properties`` file.
    ::
        clusterid.kafkasasl.jaasconfig.plain=org.apache.kafka.common.security.plain.PlainLoginModule required username='kwuser' password='kwuser-secret';
        clusterid.kafkasasl.jaasconfig.scram=org.apache.kafka.common.security.scram.ScramLoginModule required username='kwuser' password='kwuser-secret';
        clusterid.kafkasasl.jaasconfig.gssapi=com.sun.security.auth.module.Krb5LoginModule required useKeyTab=true storeKey=true keyTab="/location/kafka_client.keytab" principal="kafkaclient1@EXAMPLE.COM";
    
11. Add relevant ACLs on the Kafka cluster (IP/Principal based) to authorize Klaw to create topics and ACLs. This can be done using:
::
    
    --operation All --clusterCluster:kafka-cluster --topic "*"
    
12. Re-deploy the Cluster API with the updated configuration. This will apply the changes and enable Klaw to connect to the Kafka cluster using SSL protocol.
