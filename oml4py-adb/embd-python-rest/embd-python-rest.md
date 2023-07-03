# Run user-defined functions using the REST API for Embedded Python Execution

## Introduction

This lab walks you through the steps to use the REST API to call OML4Py Embedded Python Execution functions to run custom Python code.

Estimated Time: 20 minutes

Watch the video below for a quick walk-through of the lab.

[Run user-defined functions using Embedded Python Execution](videohub:1_2skqmxjt)

### About Embedded Python Execution
Embedded Python execution enables you to run user-defined Python functions in Python engines spawned by the Autonomous Database environment. The REST API for embedded Python execution with Autonomous Database provides REST endpoints for setting authorization tokens, executing Python scripts, and synchronously and asynchronously running jobs.

To run a script from the REST API, it must reside in the Python script repository. An Oracle Machine Learning username and password must be provided for authentication.

* `do-eval`&mdash;Runs the provided user-defined Python function in a Python engine spawned by the Autonomous Database environment.
* `table-apply`&mdash;Runs the provided user-defined Python function data referenced by an OML DataFrame proxy object in a single Python engine.
* `group-apply`&mdash;Partitions a database table by the values in one or more columns and runs the provided user-defined Python function on each partition, optionally in parallel using multiple Python engines.
* `row-apply`&mdash;Partitions a database table into sets of rows and runs the provided user-defined Python function on the data in each set, optionally in parallel using multiple Python engines.
* `index-apply`&mdash;Runs a Python function multiple times, passing in a unique index of the invocation to the user-defined function, optionally in parallel using multiple Python engines.

To view the full list of endpoints available, refer to the Oracle Machine Learning for Python REST API for Embedded Python Execution

> **Note:** Embedded Python Execution functions are also available through the [Oracle Machine Learning for Python SQL API for Embedded Python Execution](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/1/mlpug/sql-api-embedded-python-execution-oracle-autonomous-database.html#GUID-BFC5E8C9-6ACC-4993-B0E8-41138B2E479E).

### Objectives

In this lab, we provide a workflow for using the OML4Py REST API for embedded Python execution with Oracle Autonomous Database:

* Obtain an authorization token to access the REST API for embedded Python execution
* Create a user defined function (UDF) and store it in the OML4Py script repository
* Run the UDF using embedded Python execution from the OML4Py Python and REST APIs


### Prerequisites

We need to view the markdown in the OML notebook for this lab, and access the OCI Cloud Shell to run the associated cURL commands.

**Access the Lab Notebooks**

1. Go back to the main notebooks listing by clicking on the "hamburger" menu (the three lines) on the upper left of the screen, and then select **Notebooks**.

 ![Oracle Machine Learning Notebooks menu](images/go-back-to-notebooks.png "Oracle Machine Learning Notebooks menu ")

2. Click the **Lab 7 notebook name** to view it.
   <if type="freetier">
   ![Open Lab 7 notebook ft](images/click-on-lab5-ft.png "Lab 6 notebook") </if>
   <if type="livelabs">
   ![Open Lab 7 notebook ll](images/click-on-lab5-ll.png "Lab 6 notebook") </if>

  OML Notebooks will create a session and make the notebook available for editing.

  You can optionally click the **Run all paragraphs** (![](images/run-all-paragraphs.png =20x*)) icon, and then click **OK** to confirm to refresh the content with your data, or just scroll down and read the pre-recorded results.  

  ![Lab 7 Introduction notebook screen capture](images/lab5-main.png " Introduction notebook")

> **NOTE:** If you had problems downloading and extracting the ZIP file for the labs, please [**CLICK HERE** to download the lab7\_embed\_python\_rest.json notebook file](./../notebooks/lab7_embed_python_rest.json?download=1). Download the notebook file for this lab to your local machine and then import it like illustrated in **Lab 1, Task 2**.

**Access the OCI Cloud Shell to run cURL commands:**

To access the OCI Cloud Shell to run the associated cURL commands:

1. To run cURL commands, click the Developer Tools icon on your OCI console and then click Cloud Shell.  

  ![Lab 7 Task 5 cloud shell icon under Developer tools](images/devtools-cloud-shell.png " ")

2. The Oracle Cloud Shell interface opens at the bottom of your OCI console page. Here, you can run your cURL commands.

  ![Lab 7 Task 5 Cloud shell pane](images/cloud-shell-pane.png " ")

## Task 1: Obtain an Access Token

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 1.

  ![Lab 7 Task 1 Obtain the access token](images/1-obtain-access-token.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 1.1.
  ![Lab 7 Task 1.1 Export OML Cloud Service URL to environment variable](images/1-1-export-oml-url-env-variable.png " ")

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 1.2.
  ![Lab 7 Task 1.2 Assign access token to variable token](images/1-2-assign-accesstoken-variable.png " ")

## Task 2:  Obtain a proxy object to the IRIS table
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 2.

  ![Lab 7 Task 2 Obtain a proxy object to the IRIS table](images/2-obtain-proxy-object.png " ")

## Task 3: Build a Scikit-Learn Python model using embedded Python execution
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 3.

  ![Lab 7 Task 3 Build a Scikit-Learn Python model using embedded Python execution](images/3-create-udf-modelbuild.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 3.1.

  ![Lab 7 Task 3.1 View the UDF in the Python script repository](images/3-1-view-udf-inrepo.png " ")

## Task 4: Use the table-apply function to invoke the script from the Python API for embedded Python execution
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to the beginning of Task 4.

  ![Lab 7 Task 4 Use the table-apply function to invoke the script from the Python API for embedded Python execution ](images/4-tablyapply-call-udf.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

 Scroll down to the beginning of Task 4.1.

  ![Lab 7 Task 4.1 View datastore contents from Python and SQL](images/4-1-view-datastore-content.png " ")

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

 Scroll down to the beginning of Task 4.2

  ![Lab 7 Task 4.2 Python script to view datastore content](images/4-2-python-script-view-dscontent.png " ")

4. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

 Scroll down to the beginning of Task 4.3.

  ![Lab 7 Task 4.3 SQL Script to view datastore content](images/4-3-sql-script-view-dscontent.png " ")

## Task 5: Run the same function using the REST API table function table-apply
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 5.

  ![Lab 7 Task 5 Run the same function `build_lm` using the REST API table function table-apply](images/5-run-restapi-function.png " ")

  >> Note: To run cURL commands, see **Access the OCI Cloud Shell to run cURL commands** in the **Prerequisites** section.

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 5.1.

  ![Lab 7 Task 5.1 Display object name and class residing in datastore](images/5-1-display-objectname-class.png " ")

## Task 6: Create a UDF to score using system-supported data-parallelism

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 6.
   ![Lab 7 Task 6 Create a UDF to score using system-supported data-parallelism](images/6-create-udf-score.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 6.1.
   ![Lab 7 Task 6.1 Use row_apply to test the UDF from Python using data parallelism](images/6-1-test-udf.png " ")


3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 6.2.
   ![Lab 7 Task 6.2 Use row-apply to run the UDF from REST API using data parallelism](images/6-2-run-udf-from-restapi.png " ")

   >> Note: To run cURL commands, see **Access the OCI Cloud Shell to run cURL commands** in the **Prerequisites** section.

## Task 7: Work with Asynchronous Jobs

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 7.
   ![Lab 7 Task 7 Asynchronous REST API jobs](images/7-async-restapi-jobs.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 7.1.
   ![Lab 7 Task 7.1  Perform an asynchronous REST call with a timeout of 300 seconds, or 5 minutes](images/7-1-run-rest-call-timeout.png " ")

  >> Note: To run cURL commands, see **Access the OCI Cloud Shell to run cURL commands** in the Prerequisites section.

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 7.2.
   ![Lab 7 Task 7.2 Poll the job status](images/7-2-poll-jobstatus.png " ")

4. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to the beginning of Task 7.3.
   ![Lab 7 Task 7.3 Retrieve result from completed job](images/7-3-retrieve-job-results.png " ")

### Congratulations !!!

You reached the end of the lab.  

You can explore additional workshops related to Oracle Machine Learning from the link in the **Learn More** section.  

## Learn more

* [Automated Machine Learning](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/1/mlpug/automatic-machine-learning.html#GUID-4B240E7A-1A8B-43B6-99A5-7FF86330805A)
* [Oracle Machine Learning Notebooks](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/)
* [Additional Workshops for Oracle Machine Learning](https://apexapps.oracle.com/pls/apex/dbpm/r/livelabs/livelabs-workshop-cards?c=y&p100_product=70)

## Acknowledgements
* **Authors** - Marcos Arancibia, Product Manager, Machine Learning; Jie Liu, Data Scientist; Moitreyee Hazarika, Principal User Assistance Developer
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Machine Learning; Sherry LaMonica, Principal Member of Tech Staff, Machine Learning
* **Last Updated By/Date** - Marcos Arancibia, Sherry LaMonica, Moitreyee Hazarika May 2023
