# Provision and Setup #

## Introduction ##

This lab is all about getting your environment set up correctly. Here we will show you how to provision two different autonomous database instances, create a user, enable the schema for the user and load data into a table.

Estimated time: 20 - 30 minutes

Watch this short video to preview how to provision and setup your environment.

[](youtube:NP-svu6dXwI)

### About Oracle Machine Learning

Simplifying greatly, you can separate the machine learning process into three parts: data preparation, model building, and putting that model to work in production systems, like applications, dashboards, and business processes. In general, training a machine learning model on large volume data – depending on the algorithm selected – can take significant computing resources. You should not run that kind of work in an existing data warehouse, data mart, or transactional systems without consulting your administrator to avoid impacting SLAs.

For this Alpha Office scenario, we focus on the modeling and deployment process and provision two different autonomous databases. The autonomous data warehouse is used to build your model. Think of it as a stand-in for your own data warehouse or data mart. Alternatively, given how easy it is to provision a data mart, think of this ADW as the best way for you to do machine learning without impacting production systems. Setting up a dedicated data mart for machine learning may be the right thing for you to do outside of this lab.

But it’s not just about building the model - you also need to deploy it, and there are some options. You could deploy a model into a data warehouse where it’s available for analytics. Or maybe you want to deploy it into a production transaction processing system so that you can score each new transaction. Either way, you will need to know how to export and import a model. So, we show you that in this lab using two different databases.

What Alpha Office wants is to deploy this machine learning model in a production database that supports their Client Service application. When an existing customer comes into the branch, we can pull up their name and check their credit in real-time. The marketing department can also load batch data into this system and run a process to score a new set of customers in bulk. So, this setup of just two different services allows you to build, train, and deploy a machine learning model into an application so that customer-facing and marketing employees can work with it.

### Objectives

-   Learn how to provision an ADW instance
-   Learn how to provision an ATP instance
-   Create a user in ADW and ATP and grant privileges and user access to the SQL Developer Web, and to storage.
-   Load data into a table

### Prerequisites

This lab assumes you have completed the following labs:
* Login to Oracle Cloud/Sign Up for Free Tier

*Note: You may see differences in account details (eg: Compartment Name is different in different places) as you work through the labs. This is because the workshop was developed using different accounts over time.*

In this section, you will be provisioning an ADW database and an ATP database using the cloud console.

## Task 1: Create an ADW Instance

First, we are going to create an ADW Instance.

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, and select **Autonomous Data Warehouse**.

	![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-adw.png " ")

2.  From the Oracle Cloud Infrastructure console, on the Oracle Autonomous Database page choose your region and select a compartment by clicking on the drop-down list. Click **Create Autonomous Database**. This opens the Create Autonomous Database page.

    ![Create ADW page](./images/create-adw2.png  " ")

3.  Select **Compartment** by clicking on the drop-down list. (Note that yours will be different - do not select **ManagedCompartmentforPaaS**) and then enter **Display name**, **Database name**. Here, we're using the display name _ADWLab_ and database name _ORCL_.

    ![Login](./images/database-name.png  " ")

4.  Under **Choose a workload type** and **Choose a deployment type**, select **Data Warehouse**, and **Shared Infrastructure** respectively.

    ![Select the options](./images/workload-type.png  " ")

5.  Under **Configure the database**, accept the default values for **Choose database version** and **Storage (TB)** and **OCPU Count**.

    ![Configure the Database](./images/configure.png  " ")

6.  Add a password. Copy the password down in a notepad, you will need it later in labs.

    ![Add password](./images/password.png  " ")

7.  In **Choose network access**, keep the default access type **Allow secure access from everywhere**. Under **Choose a license type**, select **License Included** and click **Create Autonomous Database**. Leave the **Provide contacts for operational notifications and announcements** field blank.

    ![Create ADW](./images/provision.png  " ")

8.  Now, Autonomous Data Warehouse starts provisioning. Once it finishes provisioning, you can view the instance details.

    ![ADW provisioning](./images/provision-adw.png  " ")

    ![ADW provisioned](./images/adw-available.png  " ")

You now have created your first ADW instance. Now, we are going to work on very similar steps to create an ATP Database.

## Task 2: Create an ATP Instance

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, and select **Autonomous Transaction Processing**.

	![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")

2.  Choose your **Compartment** by clicking on the drop-down list and then click **Create Autonomous Database**.

    ![Create ATP page](./images/atp-create.png  " ")

4.  Select **Compartment**. If you are using a LiveLabs environment, be sure to select the compartment provided by the environment. (Note that yours will be different - do not select **ManagedCompartmentforPaaS**) and then enter **Display Name**, **Database Name**.
    ![Login](./images/display-name.png  " ")

5.  Under Choose a workload type and Choose a deployment type, select **Transaction Processing**, and **Shared Infrastructure** respectively.
    ![Select the options](./images/provision-atp.png  " ")

6.  Under **Configure the database**, accept the default values for **Choose database version** , **Storage (TB)**, and **OCPU Count**.
    ![Configure the Database](./images/configure.png  " ")

7.  Add a password. Copy the password in a notepad, you will need it later in labs.
    ![Add password](./images/password.png  " ")

8. In **Choose network access**, keep the default access type **Secure access from everywhere**. Under **Choose License and Oracle Database Edition**, select **Bring Your Own License (BYOL)**. The **Oracle Database Enterprise Edition** option gets selected automatically. Click **Create Autonomous Database**. Leave the **Provide contacts for operational notifications and announcements** field blank.

    ![Create ATP](./images/atp-provision.png  " ")

9. Now, Autonomous Transaction Processing starts provisioning. Once it finishes provisioning, you can view the instance details.
    ![ATP provisioning](./images/atp-provisioned.png  " ")

    ![ATP provisioned](./images/atp-available.png  " ")
You now have created your first ATP instance.

## Task 3: Create Machine Learning User in ADW

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, select **Autonomous Data Warehouse**.

	![cloud menu](https://oracle-livelabs.github.io/common/images/console/database-adw.png " ")

2.  Navigate to your ADW instance.
    ![Navigate to your instance](./images/adw-instance.png " ")

3.  Select **Tools** on the Autonomous Database Details page.
    ![Tools](./images/tools.png " ")

4. To allow us to create OML users, **Open Oracle ML User Administration** under the tools.
    ![OML user admin page](./images/open-ml-admin.png " ")

5. Sign in as **Username - ADMIN** and with the password, you used when you created your Autonomous instance.
    ![](./images/ml-login.png  " ")

6. Click **Create** to create a new machine learning user.
    ![Create machine learning user](./images/create.png  " ")

7. On the Create User form, enter **Username - machine learning\_USER**, an e-mail address (you can use admin@oracle.com), un-check **Generate password**, and enter a password you will remember. You can use the same password you used for the ADMIN account. Then click **Create**.

    ![Login](./images/create-ml-user.png  " ")

8. Notice that the **ML\_USER** is created.

    ![User create](./images/ml-user-created.png " ")

## Task 4: Grant Privileges to ADW ML_USER to Access Database Actions

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, select **Autonomous Data Warehouse**.

	![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-adw.png " ")

2. Navigate to your ADW instance.
  ![Navigate to your instance](./images/adw-instance.png " ")

3. Click **Database Actions**.

    ![Database Actions](./images/open-database-actions.png  " ")

	A **Launch DB Actions** screen appears.

	![Launch DB Actions](./images/launch-db-actions.png)

4. On the Database Actions login page, if prompted, log in with your ADW credentials, provide the **Username - ADMIN** and click **Next**. Then, provide the <if type="freetier">**Password** you created for the Autonomous instance.</if><if type="livelabs">password **WELcome__1234**</if> and click **Sign in**.

    ![Login](./images/ml-admin.png " ")

    ![Enter password](./images/ml-admin-password.png " ")

5. From the Database Actions Development menu, select **SQL**.

    ![Click SQL](./images/sql.png " ")

6. Dismiss the Help by clicking on the **X** in the popup.

    ![Dismiss Help](./images/click-x.png  " ")

7.  By default, only the ADMIN user can use the SQL Developer Web. To enable ML\_USER to use it, enter the following and click the **Run Statement** button to grant SQL developer web access to ML\_USER.

    ````
    <copy>
    BEGIN
      ORDS_ADMIN.ENABLE_SCHEMA(
        p_enabled => TRUE,
        p_schema => 'ML_USER',
        p_url_mapping_type => 'BASE_PATH',
        p_url_mapping_pattern => 'ml_user',
        p_auto_rest_auth => TRUE
      );
      COMMIT;
    END;

    </copy>
    ````

    ![Grant access to the user](./images/grant-mluser-access.png " ")
    ![access granted](./images/mluser-access-granted.png " ")

8.  To grant storage privileges to ML\_USER, enter the following code and click **Run Statement**

    ````
    <copy>
    alter user ml_user quota 100m on data;
    </copy>
    ````

    ![Grant storage orivileges](./images/storage-privileges.png " ")

9. Now, on the right, click the ADMIN profile dropdown and click **Sign Out** of the ADMIN account.

## Task 5: Create Machine Learning User in ATP

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, select **Autonomous Transaction Processing**.

    ![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")

2. Navigate to your ATP instance.
    ![Navigate to your instance](./images/atp-instance.png " ")

3.  Select **Tools** on the Autonomous Database Details page.

    ![Tools](./images/atp-tools.png " ")

4.  Select **Open Oracle ML User Administration** under the tools.

    ![Click ML admin](./images/atp-open-ml-admin.png " ")

5. Sign in as **Username - ADMIN** and with the **Password - WELcome__1234** created for your ATP instance.

    ![Login](./images/atp-ml-admin-login.png  " ")

6.  Click **Create** to create a new ML user.
    ![Create a user](./images/atp-ml-user.png  " ")

7. On the Create User form, enter **Username - ML\_USER**, an e-mail address (you can use admin@oracle.com), un-check **Generate password**, and enter a password you will remember. You can use the same password you used for the ADMIN account. Then click **Create**.
    ![Login details](./images/create-ml-user.png  " ")

8. Notice that the **ML\_USER** is created.

    ![User created](./images/atp-ml-user-created.png " ")

## Task 6: Grant Privileges to ATP ML_USER to Access Database Actions

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, select **Autonomous Transaction Processing**.

    ![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")

2. Navigate to your ATP instance.
     ![](./images/atp-instance.png " ")

3.  Click **Database Actions**.The Launch DB Actions initialization screen appears.

    ![Click Database Actions](./images/atp-tools.png " ")

4. The Database Actions login page appears. If prompted, log in with your ATP credentials. Provide the **Username - ADMIN** and click **Next**. Then provide the **Password - WELcome__1234** created for the ATP instance and click **Sign in**.

    ![Login](./images/ml-admin.png " ")

    ![Password](./images/ml-admin-password.png " ")

5. From the Database Actions menu, select **SQL**.

    ![Click SQL](./images/sql.png " ")

6. Dismiss the Help by clicking on the **X** in the popup.

    ![Dismiss Help](./images/click-x.png  " ")

7.  By default, only the ADMIN user can use the SQL Developer Web. To enable ML\_USER to use it, you need to enter the following and run the procedure to grant SQL developer web access to ML\_USER.

    ````
    <copy>
    BEGIN
      ORDS_ADMIN.ENABLE_SCHEMA(
        p_enabled => TRUE,
        p_schema => 'ML_USER',
        p_url_mapping_type => 'BASE_PATH',
        p_url_mapping_pattern => 'ml_user',
        p_auto_rest_auth => TRUE
      );
      COMMIT;
    END;

    </copy>
    ````

    ![Grant access to the user](./images/grant-mluser-access.png " ")

    ![Access granted](./images/mluser-access-granted.png " ")

8.  Grant storage privileges to ML\_USER.

    ````
    <copy>
    alter user ml_user quota 100m on data;
    </copy>
    ````

    ![Grant storage privileges](./images/storage-privileges.png " ")

## Task 7: Download Custom Lab Files
Download the compressed install file that contains data files and custom OML Notebooks that are used further in this workshop.

1.  Click the link below to download the install file.

    [install.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/n/idrudhdwamji/b/adbml/o/install.zip)

2.  Save the install.zip to a download directory and then unzip the file.

    ![Save and unzip the file](./images/save-file.png  " ")

## Task 8: Upload the Data Files to ADW ML_USER

1. On the tab with your ADW instance, click **Database Actions**.

    ![Click Database Actions](./images/open-database-actions.png  " ")

2. This time sign in as **ML\_USER**. Provide the **Username - ML\_USER** and click **Next**. Then provide the password for your ML\_USER and click **Sign in**.

    ![Login](images/ml-user-next.png)

    ![Password](images/ml-user-sign-in.png)

3. Select **Data Load** from Database Actions menu.

    ![Click Data Load](images/data-load.png)

4. Leave the default selections - **LOAD DATA** and **LOCAL FILE** and click **Next**.

    ![Click Next](images/to-load-data.png)

5. Drag and drop the **credit\_scoring\_100k.csv** from the directory where you downloaded and unzipped the install file onto the Data Load Drag and drop target or click on **Select Files** to browse the _credit\_scoring\_100k.csv_ file and upload it.

    ![Load the file](images/select-files.png)

6. When the upload is complete, click **Start** and click **Run** in the Run Data Load Job confirmation dialog box.

    ![Run](images/run.png)

    ![Run in progress](images/run2.png)

7. Notice the *Status: Running(0/1)* while loading the data. The status will be updated to *Status: Completed(1/1)* once the data loading job is completed.

    ![Data loading status](./images/loading.png " ")

    ![Data loading completed](images/load-complete.png)

8. Click the cloud menu and select **SQL** under  **Development**.

  ![Click SQL](images/development-sql.png)

9. Click on the **X** in the popup to dismiss the Help.

    ![Dismiss Help](./images/close.png  " ")

10. The Database Actions screen shows the table has been successfully created (and associated with ML\_USER).

    ![Table created](images/tables-loaded.png)

[Please proceed to the next lab](#next).

## Acknowledgements

- **Author** - Derrick Cameron, Leah Bracken (v2)
- **Contributors** - Mark Hornick, Sr. Director, Data Science and Oracle Machine Learning Product Management; Sherry LaMonica, Consulting Member of Technical Staff, Machine Learning; Marcos Arancibia, Senior Principal Product Manager, Machine Learning
- **Last Updated By/Date** - Sarika Surampudi, Principal User Assistance Developer, Oracle Database User Assistance Development, October 2022
