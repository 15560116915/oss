# Cross-region replication {#concept_zjp_31z_5db .concept}

This topic describes the application scenarios and limits of cross-region replication.

Bucket Cross-Region Replication enables automatic and asynchronous replication of objects across buckets in different OSS data centers \(regions\), which synchronizes operations \(such as creating, updating, and deleting\) performed on objects in the source bucket to the target bucket in a different region. This feature could be a boon to customers looking for cross-region disaster recovery for their buckets or data replication. Objects in the target bucket are precise copies of objects in the source bucket. They have the same object name, metadata, and content \(such as creation time, owner, user-defined metadata, object ACL, and object content\).

## Application scenarios {#section_vlj_3ad_6mm .section}

You may configure cross-region replication for your buckets for a variety of reasons, including:

-   Compliance requirements: Although, by default, OSS stores multiple copies of each object on a physical disk, compliance requirements may dictate that you store a copy of the data at a further distance. Cross-region replication allows you to replicate data between distant OSS data centers to satisfy these compliance requirements.
-   Minimize latency: Your customers are in two geographic locations. To minimize latency in accessing objects, you can maintain object copies in OSS data centers that are geographically closer to your users.
-   Data backup and disaster recovery: You have high requirements on data security and availability, and you want to explicitly maintain copies of all written data in a second data center. In case one OSS data center is damaged by a catastrophic event like earthquake and tsunami, you can use backup data from the other one.
-   Data replication: For business reasons, you may need to migrate data from one OSS data center OSS to another.
-   Operational reasons: You have computing clusters in two different data centers that analyze the same set of objects. You may choose to maintain object copies in these regions.

## Operation method {#section_bdy_cv3_kgb .section}

|Operation method|Description|
|----------------|-----------|
|[OSS console](../../../../intl.en-US/Console User Guide/Manage buckets/Configure cross-region replication.md#)|Web application that is easy to use|
|[Java SDK](../../../../intl.en-US/SDK Reference/Java/Cross-region replication.md#)|Various and complete SDK demos in different languages|

## Usage {#section_m0u_6kx_3wi .section}

Cross-region replication supports synchronization of buckets with different names. If the two buckets are in different regions, you can use this feature to synchronize the data of the source bucket to the target one in real time. This feature now offers the following capabilities:

-   Real-time synchronization: This provides the ability to monitor data additions, deletions, and modifications in real time and synchronize these changes to the target bucket. For files of 2 MB in size, data is synchronized in a matter of minutes to guarantee data consistency between the source and the target.
-   Historical data migration: This provides the ability to synchronize historical data from a bucket to form two identical data copies.
-   Real-time display of synchronization progress: This shows the point in time of the last synchronization for real-time data synchronization and the percentage of completion for historical data migration.
-   Easy configuration: The OSS console provides an easy-to-use interface for configuration management.
-   Mutual synchronization: You can achieve mutual synchronization between bucket A and bucket B as follows: Configure the synchronization from bucket A to bucket B first, and then configure the synchronization from bucket B to bucket A.

## Restrictions {#section_zoi_czi_ejm .section}

-   For two buckets that are in synchronization, because you can operate on both buckets at the same time, copying an object from the source bucket may overwrite the object with the same name in the target bucket. Be cautious when using this feature.
-   Because Bucket Replication uses an asynchronous copying method, it can take from minutes to hours to copy data to the target bucket, depending on the size of the objects being replicated.
-   Cross-region synchronization only works when no synchronization to a third bucket is enabled for the two buckets to be synchronized. For example, if synchronization to Bucket B is enabled for Bucket A, you can no longer enable synchronization to Bucket C for Bucket A, unless you delete the former configuration first. Similarly, if synchronization to Bucket B is enabled for Bucket A, it is not allowed to enable synchronization from Bucket C to Bucket B.
-   Synchronization is supported only between two buckets in different regions.

