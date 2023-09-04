# Run user-defined functions using Embedded Python Execution

## Introduction

This lab walks you through the steps to use OML4Py Embedded Python Execution functions to run custom Python code.

Estimated Time: 20 minutes

### About Embedded Python Execution
Embedded Python Execution enables you to run user-defined Python functions in Python engines spawned in the Oracle Autonomous Database environment. These engines run alongside an OML Notebooks Python interpreter session.

The OML4Py Embedded Python Execution functions are:

* `oml.do_eval`&mdash;Calls a Python function in Python engines spawned by the Oracle Autonomous Database environment.
* `oml.group_apply`&mdash;Partitions a database table by the values in one or more columns and runs the provided user-defined Python function on each partition.
* `oml.index_apply`&mdash;Calls a Python function multiple times, passing in a unique index of the invocation to the user-defined function.
* `oml.row_apply`&mdash;Partitions a database table into sets of rows and runs the provided user-defined Python function on the data in each set.
* `oml.table_apply`&mdash;Calls a Python function on data in the database as a single pandas.DataFrame in a single Python engine.

> **Note:** Embedded Python Execution functions are also available through the [OML for Python REST API for EPE](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/1/mlepe/rest-endpoints.html).

### Objectives

In this lab, you will:
* Build an open source scikit-learn linear model and scoring script
* Prepare the same script for use with Embedded Python Execution
* Build one model per group using the `group_apply` function
* Return multiple images as a result from Embedded Python Execution
* Create and run SQL and REST user defined functions

### Prerequisites

1. We need to access and run the OML notebook for this lab.

 > **NOTE:** If you have problems with downloading and extracting the ZIP file in Lab 1 Task 2, please 
 <if type="freetier">[**CLICK HERE** to download the "Lab 5 - Run UDFs with Python API for EPE" notebook DSNB file](<./../notebooks/Lab 5 - Run UDFs with Python API for EPE.dsnb?download=1>)</if><if type="livelabs">[**CLICK HERE** to download the "Lab 5 - Run UDFs with Python API for EPE" notebook DSNB file](<./../notebooks/Lab 5 - Run UDFs with Python API for EPE.dsnb?download=1>)</if><if type="freetier-ocw23">[**CLICK HERE** to download the "Lab Bonus 1 - Run UDFs with Python API for EPE" notebook DSNB file](<./../notebooks/Lab Bonus 1 - Run UDFs with Python API for EPE.dsnb?download=1>)</if><if type="livelabs-ocw23">[**CLICK HERE** to download the "Lab Bonus 1 - Run UDFs with Python API for EPE" notebook DSNB file](<./../notebooks/Lab Bonus 1 - Run UDFs with Python API for EPE.dsnb?download=1>)</if>. This notebook contains the scripts for this Lab. Save it to your local machine and import it like illustrated in **Lab 1, Task 2, Step 1**.

   Go back to the main Notebooks listing by clicking on the "hamburger" menu (the three lines) on the upper left of the screen, and then select **Notebooks EA**. 
   
   ![Go to main Notebooks EA](images/go-back-to-notebooks.png " ")
   
   <if type="freetier">
   Click the **Lab 5** notebook to view it.

   ![Open Lab 5 notebook ft](images/click-on-lab5-ft.png " ") </if>
   
   <if type="livelabs">
   Click the **Lab 5** notebook to view it.

   ![Open Lab 5 notebook ll](images/click-on-lab5-ft.png " ") </if>
   
   <if type="freetier-ocw23">
   Click the **Lab Bonus 1** notebook to view it.

   ![Open Lab Bonus 1 notebook ft](images/click-on-labbo1-ft-ocw23.png " ") </if>
   
   <if type="livelabs-ocw23">
   Click the **Lab Bonus 1** notebook to view it.
   
   ![Open Lab Bonus 1 notebook ll](images/click-on-labbo1-ft-ocw23.png " ") </if>

   OML Notebooks will create a session and make the notebook available for editing.

   You can optionally click the **Run all paragraphs** (![](images/run-all-paragraphs.png =20x*)) icon, and then click **Confirm** to refresh the content with your data, or just scroll down and read the pre-recorded results.  

   <if type="freetier">
   ![Lab 5 main screen](images/lab5-main.png " ")
   </if>
   <if type="livelabs">
   ![Lab 5 main screen](images/lab5-main.png " ")
   </if>
   <if type="freetier-ocw23">
   ![Lab Bonus 1 main screen](images/labbo1-main.png " ")
   </if>
   <if type="livelabs-ocw23">
   ![Lab Bonus 1 main screen](images/labbo1-main.png " ")
   </if>

## Task 1: Import OML4Py and supporting libraries and create data table

1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

Scroll down to the beginning of Task 1.

  ![Import OML4Py and supporting libraries](images/lab5-task1.png " ")  

## Task 2: Build and score a linear model from sklearn in Python
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

Scroll down to the beginning of Task 2.

  ![Build and score a linear model from sklearn in Python](images/lab5-task2.png " ")

## Task 3: Build the model using Embedded Python Execution
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

Scroll down to the beginning of Task 3.

  ![Build the model using EPE](images/lab5-task3.png " ")

## Task 4: Build one model per species using group_apply function
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

Scroll down to the beginning of Task 4.

  ![Build one model per species using group_apply](images/lab5-task4.png " ")

## Task 5: Return multiple images from Embedded Python Execution
1. Follow the flow of the notebook by scrolling to view and run each paragraph of this lab.

Scroll down to the beginning of Task 5.

  ![Return multiple images from EPE](images/lab5-task5.png " ")

<if type="freetier">After you reach the end of Lab 5, you can *proceed to the next lab*.</if>

<if type="livelabs">After you reach the end of Lab 5, you can *proceed to the next lab*.</if>

<if type="freetier-ocw23">After you reach the end of Lab Bonus 1, you can *proceed to the next lab*. </if>

<if type="livelabs-ocw23">After you reach the end of Lab Bonus 1, you can *proceed to the next lab*.</if>


## Learn more

* [Embedded Python Execution](https://docs.oracle.com/en/database/oracle/machine-learning/oml4py/2/mlpug/embedded-python-execution.html#GUID-AF448E56-B843-4749-979A-F89D359A8728)
* [Oracle Machine Learning Notebooks](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/)
* [Oracle Machine Learning Notebooks - Early Adopter](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/omlug/get-started-notebooks-ea-data-analysis-and-data-visualization.html#GUID-B309C607-2232-43E2-B4A1-655DB295B90B)

## Acknowledgements
* **Author** - Marcos Arancibia, Product Manager, Machine Learning; Jie Liu, Data Scientist; Moitreyee Hazarika, Principal User Assistance Developer
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Machine Learning; Sherry LaMonica, Principal Member of Tech Staff, Advanced Analytics, Machine Learning
* **Last Updated By/Date** - Marcos Arancibia, August 2023
