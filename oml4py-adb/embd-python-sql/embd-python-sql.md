# Run user-defined functions using the SQL API for Embedded Python Execution

## Introduction

This lab walks you through the steps to use the SQL API to call OML4Py Embedded Python Execution functions to run custom Python code.

Estimated Time: 20 minutes

### About Embedded Python Execution
Embedded Python execution enables you to run user-defined Python functions in Python engines spawned by the Autonomous Database environment. The SQL API for embedded Python execution with Autonomous Database provides SQL interfaces for setting authorization tokens, managing access control list (ACL) privileges, executing Python scripts, and synchronously and asynchronously running jobs.

The OML4Py embedded Python execution SQL API functions are:

* `pyqEval`&mdash;Runs the provided user-defined Python function in a Python engine spawned by the Autonomous Database environment.
* `pyqTableEval`&mdash;Runs the provided user-defined Python function data referenced by an OML DataFrame proxy object in a single Python engine.
* `pyqGroupEval`&mdash;Partitions a database table by the values in one or more columns and runs the provided user-defined Python function on each partition, optionally in parallel using multiple Python engines.
* `pyqRowEval`&mdash;Partitions a database table into sets of rows and runs the provided user-defined Python function on the data in each set, optionally in parallel using multiple Python engines.
* `pyqIndexEval`&mdash;Runs a Python function multiple times, passing in a unique index of the invocation to the user-defined function, optionally in parallel using multiple Python engines.

> **Note:** Embedded Python Execution functions are also available through the [Oracle Machine Learning for Python](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/1/mlpug/about-embedded-python-execution.html#GUID-A15F3A62-736A-4276-83F2-7C54BE026639) and the [REST APIs for Embedded Python Execution](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/1/mlepe/rest-endpoints.html)

### Objectives
In this lab, we provide a workflow for using the OML4Py SQL API for embedded Python execution with Oracle Autonomous Database:

As the ADMIN user:
* Add the OML user to cloud host Access Control List (ACL)

As the OML user:
* Obtain an authorization token to access the SQL API for embedded Python execution
* Create a UDF and store it in the OML4Py script repository
* Run the UDF using embedded Python execution from the OML4Py Python and SQL APIs

### Prerequisites
* Add the user to the host ACL
* Access and run the OML notebook for this lab.

1. We need to access and run the OML notebook for this lab.

    > **NOTE:** If you have problems with downloading and extracting the ZIP file in Lab 1 Task 2, please 
    <if type="freetier">[**CLICK HERE** to download the "Lab 6 - Run UDFs with SQL API for EPE" notebook DSNB file](<./../notebooks/Lab 6 - Run UDFs with SQL API for EPE.dsnb?download=1>)</if><if type="livelabs">[**CLICK HERE** to download the "Lab 6 - Run UDFs with SQL API for EPE" notebook DSNB file](<./../notebooks/Lab 6 - Run UDFs with SQL API for EPE.dsnb?download=1>)</if><if type="freetier-ocw23">[**CLICK HERE** to download the "Lab Bonus 2 - Run UDFs with SQL API for EPE" notebook DSNB file](<./../notebooks/Lab Bonus 2 - Run UDFs with SQL API for EPE.dsnb?download=1>)</if><if type="livelabs-ocw23">[**CLICK HERE** to download the "Lab Bonus 2 - Run UDFs with SQL API for EPE" notebook DSNB file](<./../notebooks/Lab Bonus 2 - Run UDFs with SQL API for EPE.dsnb?download=1>)</if>. This notebook contains the scripts for this Lab. Save it to your local machine and import it like illustrated in **Lab 1, Task 2, Step 1**.

    Go back to the main Notebooks page by clicking on the Cloud menu icon ![](images/cloud-menu-icon.png) on the upper left of the screen to open the left navigation pane, and then click **Notebooks**. 

    ![Go to main Notebooks EA](images/go-back-to-notebooks-rw.png " ")

    <if type="freetier">
    Click the **Lab 5** notebook to view it.

    ![Open Lab 5 notebook ft](images/click-on-lab6-ft.png " ") </if>

    <if type="livelabs">
    Click the **Lab 5** notebook to view it.

    ![Open Lab 5 notebook ll](images/click-on-lab6-ft.png " ") </if>

    <if type="freetier-ocw23">
    Click the **Lab Bonus 2** notebook to view it.

    ![Open Lab Bonus 2 notebook ft](images/click-on-labbo2-ft-ocw23.png " ") </if>

    <if type="livelabs-ocw23">
    Click the **Lab Bonus 2** notebook to view it.

    ![Open Lab Bonus 2 notebook ll](images/click-on-labbo2-ft-ocw23.png " ") </if>

    OML Notebooks will create a session and make the notebook available for editing.

    You can optionally click the **Run all paragraphs** (![](images/run-all-paragraphs.png =20x*)) icon, and then click **Confirm** to refresh the content with your data, or just scroll down and read the pre-recorded results.  

    <if type="freetier">
    ![Lab 6 main screen](images/lab6-main.png " ")
    </if>
    <if type="livelabs">
    ![Lab 6 main screen](images/lab6-main.png " ")
    </if>
    <if type="freetier-ocw23">
    ![Lab Bonus 2 main screen](images/labbo2-main.png " ")
    </if>
    <if type="livelabs-ocw23">
    ![Lab Bonus 2 main screen](images/labbo2-main.png " ")
    </if>


## Task 1: Add the OML user to the cloud host ACL

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

    Scroll down to Task 1.
    > Note: You must be an ADMIN user to perform this task.

    ![Add the OML user to the cloud host ACL](images/1-appendhost.png " ")  

    ```
    <copy>
    exec pyqAppendHostAce('OMLUSER','adb.us-ashburn-1.oraclecloudapps.com');
    ```

    > Note: In this example, `us-ashburn-1` is the region. Make sure to replace it with the region where your Autonomous Database is located.

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to end of Task 1.1.

  ![Confirm the host ACL contains an entry](images/1-1-add-omluser-acl-list.png " ")

    ```
    <copy>
    set serveroutput on
  
    DECLARE 
      hostname VARCHAR2(4000);
    BEGIN
      hostname := pyqGetHostACE('OMLUSER'); 
      DBMS_OUTPUT.put_line ('hostname: ' || hostname);
    END;
    /
    <copy>
    ```

## Task 2: Obtain an authorization token to access the SQL API for embedded Python execution

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 2.

  ![Obtain an authorization token](images/2-obtain-auth-token.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 2.1.
  ![Define get_token2 function](images/2-1-define-function-to-obtain-accesstoken.png " ")

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 2.2.

    > IMPORTANT: Replace the Password and the URL with your own, otherwise the rest of the Lab will not run successfully.

  ![Script to create ACCESS_DETAILS](images/2-2-create-access-details.png " ")

## Task 3 Set the token in the Token Store and check its status

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to see the code
  ![Obtain and set the auth token](images/2-3-obtain-set-auth-token.png " ")

## Task 4: Obtain a proxy object to the IRIS table

Here, you run the following script to obtain a proxy object for the IRIS table.
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 3.

  ![Obtain a proxy object to the IRIS table](images/3-proxy-object-iris.png " ")

## Task 5: Build a Scikit-Learn Python model using embedded Python execution

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

    Scroll down to Task 4.

  ![Build a Scikit-Learn Python model using EPE](images/4-define-function.png " ")


1.  Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

    Scroll down to Task 4.1. The function confirms the existence of the function build_lm in the script repository.

    ![View the UDF build_lm in the Python script repository](images/4-1-check-udf.png " ")


## Task 6: Use the table-apply function to call the script from the Python API for embedded Python execution

> **Note:** Here, we are only running the function in the Python API for embedded Python execution as a test step before running it from the SQL API for embedded Python execution.

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 5.
  ![Use the table-apply function from the Python API for EPE](images/5-call-udf-specify-ds.png " ")

  The returned value is a DataFrame containing the coefficients of the predictors.

## Task 7: View Datastore content using Python and SQL
  The model `regr` is now stored in datastore `ds1`. You can view the datastore content, either from Python or SQL. First, display  Then, display the same information by querying the `USER_PYQ_DATASTORES` view.

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 6.
  ![View datastore contents from Python and SQL](images/6-view-ds-content.png " ")
2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 6.1.

 ![Python Script to view datastore content](images/6-1-view-dscontent-python.png " ")

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 6.2.

 ![SQL Script to view datastore content](images/6-2-view-dscontent-sql.png " ")

## Task 8: Run the same function `build_lm` using the SQL API table function pyqTableEval

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 7.
  ![Run the same function using the SQL API function pyqTableEval](images/7-run-sqlapi-func.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 7.1.   
 ![Return output as a structured table](images/7-1-output-struc-table.png " ")

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 7.2.

  ![Display model object name and class in datastore](images/7-2-display-modelobject-class-ds.png " ")

4. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 7.3.
  ![Return output as a JSON string](images/7-3-output-json-string.png " ")

5. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
    Scroll down to Task 7.4.
  ![Return output as a XML string](images/7-4-output-xml-string.png " ")

## Task 9: Create a UDF to score using system-supported data-parallelism

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 8.

  ![Create UDF to score using system-supported data-parallelism](images/8-udf-scoring.png " ")

2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 8.1.

  ![Use row_apply for UDF score_mod from Python with data parallelism](images/8-1-test-udf.png " ")

  Test the UDF working with embedded Python execution, use the `oml.row_apply` function to score the IRIS data using the model in the datastore.

  ![Output](images/8-1-udf-output.png " ")  

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 8.2.

  ![Run function score_mod using the SQL API table function pyqRowEval](images/8-2-run-function-pyqroweval.png " ")

## Task 10: Return PNG Output

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 9.

  ![Return PNG output](images/9-return-png-output.png " ")
2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 9.1.
  ![Create the density plot UDF in the script repository](images/9-1-script-density-plot.png " ")
3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 9.2.

  ![Call the UDF from Python and return PNG image](images/9-2-invoke-udf-densityplot.png " ")

4. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 9.3.
  ![Corresponding SQL call to return PNG images](images/9-3-invoke-densityplot-sql.png " ")

## Task 11: Work with Asynchronous SQL API jobs

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

   Scroll down to Task 10.

   ![Work with Asynchronous SQL API jobs](images/10-async-sqlapi-jobs.png " ")


2. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 10.1.
  ![Call pyqRowEval asynchronously and store the JOB_ID](images/10-1-create-job-table.png " ")

3. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 10.2.
  ![View the JOB_ID](images/10-2-view-jobid.png " ")

4. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.
  Scroll down to Task 10.3.
  ![Script to allow the asynchronous job to complete](images/10-3-jobcomplete-view.png " ")
5. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

  Scroll down to Task 10.4.
  ![Check job status](images/10-4-view-job-status.png " ")

6. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.   
   Scroll down to Task 10.5.
  ![Script to retrieve results from completed jobs](images/10-5-retrieve-results.png " ")


### Congratulations !!!

You reached the end of the lab.  

You can explore additional workshops related to Oracle Machine Learning from the link in the **Learn More** section.  

## Learn more

* [Embedded Python Execution](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/2/mlpug/embedded-python-execution.html#GUID-AF448E56-B843-4749-979A-F89D359A8728)
* [Oracle Machine Learning Notebooks](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/)


## Acknowledgements
* **Authors** - Marcos Arancibia, Product Manager, Machine Learning; Jie Liu, Data Scientist; Moitreyee Hazarika, Principal User Assistance Developer; Dhanish Kumar, Senior Member of Technical Staff
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Machine Learning; Sherry LaMonica, Principal Member of Tech Staff, Machine Learning
* **Last Updated By/Date** - Dhanish Kumar, July 2025