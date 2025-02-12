# Create and Run a Data Monitoring Job using Oracle Machine Learning Services

## Introduction

This lab walks you through the steps to create and run a data monitoring job using Oracle Machine Learning Services. 

Estimated Time: 40 minutes

### About Data Monitoring in Oracle Machine Learning Services

Data Monitoring evaluates how your data evolves over time. It helps you with insights on trends and multivariate dependencies in the data. It also gives you an early warning about data drift.

Oracle Machine Learning Services extends OML functionality to support data monitoring. The output data is written to the user specified output tables. This table is created by Oracle Machine Learning Services and its format also depends on the job type. The output schema name is `outputSchemaName`. You can overwrite the default by the `outputSchemaName` attribute. 

### Objectives

In this lab, you will:
* Create and Run a Data Monitoring Job
    * Create a data monitoring job
    * View the job details
    * Enable the job to run
    * View job output

  >**Note:** This example in this lab uses the `HOUSEHOLD POWER CONSUMPTION` dataset.

### Prerequisites

This lab assumes you have:
* OCI Cloud Shell, which has cURL installed by default. If you are using the Workshops tenancy, you get OCI Cloud Shell as part of the reservation. However, if you are in your own OCI tenancy or using a free trial account, ensure you have OCI Cloud Shell or install cURL for your operating system to run the OML Services commands.
* An Autonomous Database instance created in your account/tenancy if you are using your own tenancy or a free trial account. You should have handy the following information for your instance:
    * Your OML user name and password
    * `oml-cloud-service-location-url`
* Completed all previous labs successfully.
* Access to the dataset to monitor

## Task 1: Create and Run a Data Monitoring Job

To create a data monitoring job:

1. Obtain an authentication token by using your Oracle Machine Learning (OML) account credentials to send requests to OML Services. See **Lab 1-Authenticate your OML Account with your Autonomous Database instance to use OML Services** in this workshop on how to obtain the authentication token. 


2. Create a data monitoring job by sending a `POST` request to the `/omlmod/v1/jobs` endpoint in OML Services. 

    >**Note:** OML Services interacts with the `DBMS_SCHEDULER` to perform actions on jobs. 

  The details for data monitoring are specified in `jobProperties` parameter, that includes: 
    * Data monitoring job name and type
    * Autonomous Database service level
    * Table where the data monitoring details will be saved
    * Drift alert trigger
    * Threshold
    * Maximum number of runs
    * Baseline and new data to be used
    * Performance metric
    * Start date (optional ) and end date (optional ) correspond to a `DATE` or `TIMESTAMP` column in the table or view denoted by `newData`, and contained in the `timeColumn` field. If the start and end dates are not specified, the earliest and latest dates and times in the `timeColumn` are used.

  >**Note:** The command uses `jq`, a command-line JSON processor available on Linux and Mac OS systems to extract relevant components from the response. 

  _Example of a data monitoring job request:_
  

  ```
      <copy>
      $ curl -X POST "<oml-cloud-service-location-url>/omlmod/v1/jobs" \
        --header "Authorization: Bearer ${token}" \
        --header 'Content-Type: application/json' \
        --data '{
          "jobSchedule": {

              "jobStartDate": "2024-11-11T20:30:26Z",             
              "repeatInterval": "FREQ=HOURLY",                   
              "jobEndDate": "2024-11-19T23:30:26Z",              
              "maxRuns": "10"                                    
          },
          "jobProperties": {
              "jobName": "HouseholdPowerDataMonitoring",         
              "jobType": "DATA_MONITORING",                      
              "disableJob": false,                      
              "outputData": "householdPowerConsumption",         
              "baselineData": "HOUSEHOLD_POWER_BASE",             
              "newData": "HOUSEHOLD_POWER_NEW",                  
              "inputSchemaName": "OMLUSER",                      
              "outputSchemaName": "OMLUSER",                     
              "jobDescription": "Monitor household power",       
              "jobServiceLevel": "LOW",                          
              "timeColumn": "DATES",                             
              "startDate": "2008-01-01T00:00:00Z",               
              "endDate": "2010-11-26T00:00:00Z",                 
              "frequency": "Year",                               
              "threshold": 0.8,                                  
              "recompute": false,                                
              "caseidColumn": null,                              
              "anchorColumn": null,                              
              "featureList": [                                   
              "jobStartDate": "2024-11-11T20:30:26Z",             
              "repeatInterval": "FREQ=HOURLY",                   
              "jobEndDate": "2024-11-19T23:30:26Z",              
              "maxRuns": "10"                                    
          },
          "jobProperties": {
              "jobName": "HouseholdPowerDataMonitoring",         
              "jobType": "DATA_MONITORING",                      
              "disableJob": false,                               
              "outputData": "householdPowerConsumption",         
              "baselineData": "HOUSEHOLD_POWER_BASE",             
              "newData": "HOUSEHOLD_POWER_NEW",                  
              "inputSchemaName": "OMLUSER",                      
              "outputSchemaName": "OMLUSER",                     
              "jobDescription": "Monitor household power",       
              "jobServiceLevel": "LOW",                          
              "timeColumn": "DATES",                             
              "startDate": "2008-01-01T00:00:00Z",               
              "endDate": "2010-11-26T00:00:00Z",                 
              "frequency": "Year",                               
              "threshold": 0.8,                                  
              "recompute": false,                                
              "caseidColumn": null,                              
              "anchorColumn": null,                              
              "featureList": [                                   

                  "GLOBAL_ACTIVE_POWER",
                  "GLOBAL_REACTIVE_POWER",
                  "VOLTAGE",
                  "SUB_METERING_1",
                  "SUB_METERING_2",
                  "SUB_METERING_3"
                ]
            }
        }' | jq
    

      </copy>
   ```

  The parameters in this command are:
  
  * `jobName` specifies the name of the submitted job.
  * `jobType` specifies the type of job to be run.

    >**Note:** For model monitoring jobs, this parameter is set to `MODEL_MONITORING`
    `outputData` is the output data identifier. The results of the job will be written to a table named `{jobId}_{ouputData}`
  * `baselineData` is a table or view name that contains baseline data to monitor. At least 50 rows per period are required for model monitoring, otherwise analysis is skipped.
  * `newData` is a table or view name with new data to be compared against the baseline. At least 100 rows per period are required for data monitoring, otherwise analysis is skipped.



When your job is submitted successfully, you will receive a response with a `jobid`. Note the `jobId` to use it in submit requests to retrieve job details or to perform any other actions on the job.

    

_Sample Response:_
  Here is an example of a data monitoring job creation response: 

  ```
     {
       "jobId": "OML$7ABB6308_1664_4CB4_84B1_598A6EA599D1",
       "links": [
       {
         "rel": "self",
             "href": "<OML Service URL>/omlmod/v1/jobs/OML%247ABB6308_1664_4CB4_84B1_598A6EA599D1"
       }
     ]
     }
   ```

This completes the task of creating and running a data monitoring job. 

## Task 2: View Details of the Submitted Job

1. To view details of your submitted job, send a GET request to the `/omlmod/v1/jobs/{jobID}` endpoint. Here, `jobId` is the ID provided in response to the successful submission of your data monitoring job in the previous step. 

  _Example of a GET request to view job details:_


    ```
    <copy>
    $ export jobid='OML$7ABB6308_1664_4CB4_84B1_598A6EA599D1'  

    $ curl -X GET "<oml-cloud-service-location-url>/omlmod/v1/jobs/${jobid}"  \
         --header 'Accept: application/json' \
         --header 'Content-Type: application/json' \
         --header "Authorization: Bearer ${token}" | jq
    </copy>

    ```
  
  _Sample Response:_
  Here is the response of the job details request. If your job has already run once earlier, you will see information returned about the last job run.

    ```
    <copy>
      {
        "jobId": "OML$7ABB6308_1664_4CB4_84B1_598A6EA599D1",
          "jobRequest": {
          "jobSchedule": {
          "jobStartDate": "2024-11-11T20:30:26Z",
          "repeatInterval": "FREQ=HOURLY",
          "jobEndDate": "2024-11-19T23:30:26Z",
          "maxRuns": 3
      },
        "jobProperties": {
          "jobType": "DATA_MONITORING",
          "inputSchemaName": "OMLUSER",
          "outputSchemaName": "OMLUSER",
          "outputData": "householdPowerConsumption",
          "jobDescription": "Monitor household power",
          "jobName": "HouseholdPowerDataMonitoring",
          "disableJob": false,
          "jobServiceLevel": "LOW",
          "baselineData": "HOUSEHOLD_POWER_BASE",
          "newData": "HOUSEHOLD_POWER_NEW",
          "timeColumn": "DATES",
          "startDate": "2008-01-01T00:00:00Z",
          "endDate": "2010-11-26T00:00:00Z",
          "frequency": "Year",
          "threshold": 0.8,
          "recompute": false,
          "caseidColumn": null,
          "featureList": [
            "GLOBAL_ACTIVE_POWER",
            "GLOBAL_REACTIVE_POWER",
            "VOLTAGE",
            "SUB_METERING_1",
            "SUB_METERING_2",
            "SUB_METERING_3"
          ],
            "anchorColumn": null
        }
      },
      "jobStatus": "CREATED",
      "dateSubmitted": "2024-11-19T20:23:31.53651Z",
      "links": [
        {
          "rel": "self",
          "href": "<OML Service URL>/omlmod/v1/jobs/OML%247ABB6308_1664_4CB4_84B1_598A6EA599D1"
        }
      ],
        "jobFlags": [],
        "state": "SCHEDULED",
        "enabled": true,
        "runCount": 0,
        "nextRunDate": "204-11-20T20:30:26Z" 
    }

    </copy>
    ```


## Task 3: Query the Output Table to view the Data Monitoring Details 

Once your job has run, either according to its schedule or by the RUN action, you can view its output in the output table. You must specify the table in your job request with the `outputData` parameter. The full name of the table is in the format `{jobid}_{outputData}`.

1. Check if your job is complete by sending a request to view its details.

2. Run the following SQL command to query the output table. Here is the syntax:

    ```
    %sql

    SELECT {column_1}, {column_2}, {column_3}, {column_4}, {column_5}, {column_6}, 
      {column_7}, {column_8} 
    FROM {jobId}_{outputData}
    ORDER BY {column_1}, {column3}
    ```

  _Example to query the output table associated with this example:_
  
    ```
    <copy>
    %sql

    SELECT START_TIME, END_TIME, IS_BASELINE, THRESHOLD, HAS_DRIFT, round(DRIFT, 3), 
         FEATURE_NAME, ROUND(IMPORTANCE_VALUE, 3) 
    FROM OML$7ABB6308_1664_4CB4_84B1_598A6EA599D1_householdPowerConsumption
    ORDER BY FEATURE_NAME, IS_BASELINE DESC

    </copy>
    ```
  After you run the query, scroll down the output table to view if there is information for the `baseline` time period and `newdata` time period for each of the dataset features being monitored for drift. Many of the columns may be empty in the baseline rows, as the data monitoring is done on the new data, not the baseline data. 

This completes the task of creating and running a data monitoring job. You may now **proceed to the next lab.**

## Learn More

* [REST API for Oracle Machine Learning Services](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/index.html)
* [Work with Data Monitoring](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/omls-data-monitoring.html)



## Acknowledgements

* **Author** - Moitreyee Hazarika, Principal UAD, Database User Assistance Development
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Oracle Machine Learning Product Management; Sherry LaMonica, Consulting Member of Technical Staff, Oracle Machine Learning; Marcos Arancibia Coddou, Senior Principal Product Manager, Machine Learning
* **Last Updated By/Date** - Moitreyee Hazarika, February 2025
