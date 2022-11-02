# bi_cn_bigquery_datatransfer_generation Cloud Function

Google Cloud Function used for BigQuery scheduled query(one type of bigquery datatransfer job) generation. Current implementation gets data from input.yaml, delete duplicate scheduled query of all project and create/update scheduled query based on data in input.yaml.

This cloud function will execute one of the following operations:
- get transfer config info - get all info about scheduled query in this project,location
- delete scheduled query - delete one or multiple scheduled queries that duplicate to another scheduled query
- update scheduled query - update current scheduled query using data in input.yaml, the parent id won't change
- create scheduled query - create a new scheduled query using data in input.yaml

Function will be invoked by cloud scheduler and can be manually triggered.

In case of issues, exception will hold the error and raise an error message.


# Development instructions and configuration

Corresponding function configuration for each environment (dev/test/acc/prod) is provided in config folder.
Variables are provided via environment variables defined in config/environment-variables.

Parameters are described in the table below:
+---------------------------------+----------------------------+
|    Parameter                    | Description                |
+---------------------------------+----------------------------+
| project_id                      | GCP project id.            |
| data_transfer_location_id       | Location id                |
+---------------------------------+----------------------------+


# Function prerequirements

This cloud function involves create, update, delete bigquery datatransfer jobs, so some permissions are required to perform this function properly. (Permissions needed to create and invoke cloud functions are not included.)

+---------------------------------+----------------------------+
|    Operation                    | Permissions needed         |
+---------------------------------+----------------------------+
| get tansfer config list         | bigquery.transfers.get     |
+---------------------------------+----------------------------+
| delete scheduled query          | bigquery.transfers.update  |
+---------------------------------+----------------------------+
| update scheduled query          | bigquery.transfers.update  |
|                                 | bigquery.jobs.create       |
+---------------------------------+----------------------------+
|                                 | bigquery.transfers.update  |
| create scheduled query          | bigquery.jobs.create       |
|                                 | bigquery.transfers.get     |
+---------------------------------+----------------------------+


# Function execution

GCF should be invoked by Google Cloud Scheduler.
Scheduler creates event in bi-cn-bigquery-dataransfer-generation-trigger topic.
