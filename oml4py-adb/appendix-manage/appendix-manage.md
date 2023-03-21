# Appendix: OML user administration

## Introduction
This lab walks you through the steps to create and manage an Oracle Machine Learning user.

Estimated Time: 5 minutes

### Objectives

In this lab, you will learn how to create an Oracle Machine Learning user using two different interfaces
* Use the Database Users interface inside Database Actions administration section
* Use the Oracle Machine Learning Notebooks administration interface
* Get familiar with OML user settings

## Task 1: Use the Database Users interface inside Database Actions administration section

 1. An administrator can create a new user account and user credentials for Oracle Machine Learning in the Database Users administration interface.

  > **Note:** You must have the administrator role to access the Database Users interface inside the administration section of the Database Actions interface.

  To access the Autonomous Database Actions as the ADMIN user, in your Autonomous Database details page, click the Database Actions button.

  ![Click on Database Actions](images/launchdbactions.png "Click on Database Actions")

 2. To access the Database Users option in Database Actions, scroll down to the **Administration** section and click the Database Users tile.

  ![In the administration section, click on Database Users](images/dbactions-database-users.png "In the administration section, click on Database Users")

 3. You should see the list of all current users registered with this Autonomous Database instance.  Click on the **Create User** button to begin the creation of a new user.

  ![In the Database Users page, click the Create User button](images/database-users-current-users.png "In the Database Users page, click the Create User button")

 4. In the **Create User** menu, enter the following information:
  - **Username:** Enter `omluser` for username. Using this username, the user will log in to an Oracle Machine Learning instance.
  - **Password:** Enter a password for the user, twice.
  Please **do not select** the **Password Expired** option, otherwise the user will receive a `Password Expired` notice when trying to login into OML Notebooks but will be unable to change it from that interface without first logging in.
  - **Confirm Password:** Enter a password to confirm the value that you entered in the Password field.
  By doing so, you create the password for the user. The user can change the password when first logging in.
  -  Select the UNLIMITED option in the **Quota on tablesoace DATA** section
  -  Make sure to check the **OML** toggle to ON.
  Click on **Create User**.

    ![In the Create Users page, enter username and password, adjust quota to unlimited and toggle OML to ON, and click create user](images/database-users-create-user.png "In the Create Users page, enter username and password, adjust quota to unlimited and toggle OML to ON, and click create user")

  `5.` You can repeat `step 4` when you need to create additional users.  For example, if you create an additional user named `omluser2`, you will see the result below if you filter the list of users by `omluser`.

    ![List of users available that start with omluser](images/dbactions-database-list-omluser.png "List of users available that start with omluser")




## Learn More

* [Administer Oracle Machine Learning](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/omlug/administer-oracle-machine-learning.html#GUID-E74F0E2E-EEE5-4421-A0BB-96A58811C04A)
* [Oracle Machine Learning Notebooks](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/)

## Acknowledgements
* **Authors** - Marcos Arancibia, Product Manager, Machine Learning; Moitreyee Hazarika, Principal User Assistance Developer
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Machine Learning; Sherry LaMonica, Principal Member of Tech Staff, Machine Learning; Jie Liu, Data Scientist
* **Last Updated By/Date** - Moitreyee Hazarika, February 2023
