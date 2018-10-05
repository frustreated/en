# JDCLOUD Stream Computing APIs


## Introduction
Provide related APIs for stream computing operation.


### Version
v1


## API
|Interface name|Request mehod|Function description|
|---|---|---|
|**addOrUpdateJob**|POST|Add or update job|
|**addOrUpdateStorage**|POST|Create or update storage|
|**createNamespace**|POST|Create namespace|
|**deleteJob**|DELETE|Delete job|
|**deleteNamespace**|DELETE|Delete namespace; if there are other subordinate resources associated with it, it is not allowed to delete it|
|**deleteStorage**|DELETE|Delete the assigned input|
|**describeJob**|GET|Query the details of assigned job|
|**describeStorage**|GET|Query the assigned input|
|**getJobList**|GET|Query all the jobs under the assigned applications|
|**getStorageList**|GET|Create or update storage|
|**queryNamespaceDetail**|GET|Query the details of a certain application|
|**queryNamespaces**|GET|Query the application list under the tenant|
|**startJob**|GET|Running job|
|**stopJob**|GET|Stop job running|
|**updateNamespace**|PUT|Update namespace|