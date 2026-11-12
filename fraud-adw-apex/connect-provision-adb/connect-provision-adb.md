# Provision and Setup

## Introduction

In this lab you will setup your environment. Here we will show you how to provision an autonomous database instances, create a user, load data into the database, and set up credentials and tools.

Estimated Time: 20 mins

### Objectives

* Learn how to provision an ADW instance
* Learn how to connect to ADW and use Database Actions

### Prerequisites

This lab assumes you have completed the following labs:

* Sign Up for Free Tier/Login to Oracle Cloud

*Note: You may see differences in account details (eg: Compartment Name is different in different places) as you work through the labs. This is because the workshop was developed using different accounts over time.*

In this section, you will provision an ADWC database, create an OML user account, and load the data required by the OML notebooks.

## Task 1: Create an ADW Instance

First, we are going to create an ADW Instance.

1. Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, and select **Autonomous Data Warehouse**.

    ![Database adw](https://oracle-livelabs.github.io/common/images/console/database-adw.png " ")

1. Select **Create Autonomous Data Warehouse**.

    ![Select Create Autonomous Data Warehouse](./images/006.png)

1. Select **Compartment** by clicking on the drop-down list. (Note that yours will be different - do not select **ManagedCompartmentforPaaS**) and then enter **Display Name**, **Database Name**.

    ![Prov adw 1](./images/prov-adw-1.png)

1. Under **Choose a workload type** and **Choose a deployment type**, select **Data Warehouse**, and **Shared Infrastructure** respectively.

    ![Prov adw 2](./images/prov-adw-2.png)

1. Under **Configure the database**, leave **Choose database version** and **Storage (TB)** and **OCPU Count** as they are.

    ![Under Configure the database, leave Choose database version and Storage (TB) and OCPU Count as they are](./images/009.png)

1. Add a password. Note the password down in a notepad, you will need it later in Lab 2.

    ![Add a password. Note the password down in a notepad, you will need it later in Lab 2](./images/010.png)

1. Under **Choose a license type**, select **License Included**.

    ![Prov adw 3 ft](./images/prov-adw-3-ft.png)

1. Click **Create Autonomous Database**. Once it finishes provisioning, you can click on the instance name to see details of it.

    ![Prov adw 5](./images/prov-adw-5.png)

## Task 2: Create an OML User

1. On your instance, click the **Tools** tab, and then click **Open Oracle ML User Administration**.

    ![Adw open ml user admin](./images/adw-open-ml-user-admin.png)

1. Sign in as admin with the password you used when you created your Autonomous instance.

    ![Signin db](./images/signin-db.png)

1. Click on **Create**.

    ![Oml create user 1](images/oml-create-user-1.png)

1. On the **Create User** form, enter the username **omluser**, an e-mail address (you can use <admin@oracle.com>), uncheck **Generate password**, and enter a password you will remember. You can use the same password you used for the ADMIN account. Then click **Create**.

    ![Oml create user 2](images/oml-create-user-2.png)

## Task 3: Allow OMLUSER to access Database Actions

1. On the tab with your ADW instance, and click on **Open Database Actions**.

    ![ADW tools db actions](images/adw-tools-db-actions.png)

1. Login as ADMIN, using the same password you created for ADMIN when you created the ADW instance.

    ![Actions select admin](images/actions-select-admin.png)

    ![Actions login admin](images/actions-login-admin.png)

1. From the Database Action menu, select **SQL**.

    ![Actions sql](images/actions-sql.png)

1. Dismiss the Help by clicking on the **X** in the popup.

    ![Sql dev dismiss help](images/sql-dev-dismiss-help.png)

1. Copy and paste the SQL below into the SQL pane to allow OMLUSER to use the Database Actions.

    ```
      <copy>BEGIN
      ORDS_ADMIN.ENABLE_SCHEMA(
        p_enabled => TRUE,
        p_schema => 'OMLUSER',
        p_url_mapping_type => 'BASE_PATH',
        p_url_mapping_pattern => 'omluser',
        p_auto_rest_auth => TRUE
      );
      COMMIT;
    END;
    /</copy>
    ```

1. Click **Run Script** to execute the SQL.

    ![Sql dev add omluser](images/sql-dev-add-omluser.png)

    ![Sql dev add omluser success](images/sql-dev-add-omluser-success.png)

## Task 4: Download Files to use in next lab

1. Click the link below to download the install file.

    [claims.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/4Au3jJ-t8Wtw3LWyYsDMqQk2FdlXDvXCb1MYRS0J6P4QS-GCrv2YNOolWdtFMLZq/n/c4u04/b/livelabsfiles/o/data-management-library-files/claims.zip)

1. Save `claims.zip` to a download directory and then unzip the file.

    ![Save claims zip](./images/save-claims-zip.png)

## Task 5: Upload the two data files to OMLUSER

1. On the tab with your ADW instance, and click on **Open Database Actions**.

    ![ADW tools db actions](images/adw-tools-db-actions.png)

1. This time sign in as **omluser**.

    ![Omluser signin 1](images/omluser-signin-1.png)

    ![Omluser signin 2](images/omluser-signin-2.png)

1. Select **Data Load** from Database Actions.

    ![Datatools dataload](images/datatools-dataload.png)

1. Leave the default selections (**Load data** and **Local File**) and click **Next**.

    ![Datatools dataload 2](images/datatools-dataload-2.png)

1. Drag the **CLAIMS.csv** and **CLAIMS0.csv** files from the directory where you downloaded and unzipped onto the Drag and Drop target.

    ![Datatools dataload drop target](images/datatools-dataload-drop-target.png)

1. When the upload is complete, click **Start** and click **Run** in the confirmation dialog.

    ![Datatools dataload start job](images/datatools-dataload-start-job.png)

    ![Datatools dataload run job](images/datatools-dataload-run-job.png)

1. When the job is completed, click on the hamburger menu to open the database tools menus and select **SQL**.

    ![Datatools dataload run job completed](images/datatools-dataload-run-job-completed.png)

    ![Datatools sql dev](images/datatools-sql-dev.png)

1. The SQL Web Developer shows the two tables have been successfully created (and associated with OMLUSER).

    ![Datatools sql dev tables loaded](images/datatools-sql-dev-tables-loaded.png)

    You may now [proceed to the next lab](../create-ml-model/create-ml-model.md).

## Acknowledgements

* **Author** - Mark Hornick , Sr. Director, Data Science / Machine Learning PM

* **Last Updated By/Date** - Siddesh Ujjni, Senior Cloud Engineer, October 2021.

