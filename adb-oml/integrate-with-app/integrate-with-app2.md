# Using Prediction in an Application

## Introduction

In Lab 2, you built a model and in Lab 3, you imported that model into a new database, representing a production system. Now we just need to integrate that model with existing applications and processes. It’s worth repeating that this is a critical step. Machine learning models aren’t delivering value until they are being actively used by the company in existing applications and processes.

Estimated time: 20 - 30 minutes

Watch this short video to preview how to use prediction in an application.

[](youtube:e0TILs7IDgU)

### Before You Begin

We will start this lab with some more setup, loading both data and an APEX application, the Alpha Office Customer Service application. Once those are ready we will show you three ways that you could deploy this model.

First, we will integrate it into the Customer Service application. This is something that customer-facing employees would use when interacting with customers. In this case, the customer walks into the office, asks about making a purchase, and the rep can get an immediate answer. By integrating this model with that application workflow, we can shorten the process of applying for a given new service and improve customer experience. This is a common situation, where a model can help guide a response to an event in real time. This would include situations like any customer interaction, recommending products on a web site, processing a financial transaction, or responding to a new sensor reading on a piece of equipment.

In the second scenario, we are going to process a large number of customers in batch. Alpha Office has completed an acquisition and inherited some new customers. The marketing department would like to process all those customers at once, perhaps creating a campaign to target those who are suitable. In this lab, we do that using the Customer Service application. It would also be possible to address a use case like this by loading bulk data into the database, running scripts to process them, and making results available through a suitable analytics tool like Oracle Analytics Cloud.

Finally, while APEX applications work well with Oracle Database, many organizations use a distributed development approach. If this is the case, making a model available via a REST API endpoint is the way to go. We’ll show you how to do that as well.

And when you’ve completed this lab, you are all done. You have set up two autonomous databases, built, trained, and deployed a machine learning model. We hope that you can take your new-found skills back to your organization. Are there some business problems that you might be able to help with? Are there existing data sets that colleagues are analyzing where machine learning might help? If you set up a free trial account to do this lab, then you can continue to use the free tier or your $300 of cloud credits to experiment further. And you can find more information at Oracle.com/machine-learning

### Objectives

In this lab, you will:
- Import data to set up the lab.
- Import an APEX application.
- Review the application to see how you can make predictions on the fly.
- Expose your machine learning model as a REST endpoint so any application can use it.

### Prerequisites

This lab assumes you have completed the following labs:
- Login to Oracle Cloud/Sign Up for Free Tier Account
- Connect and Provision ADB
- Create a Machine Learning Model
- Migrate Machine Learning Model to ATP

## Task 1: Prepare Data for the Lab in ATP

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, select **Autonomous Transaction Processing**, choose your compartment and navigate to your ATP instance.

    ![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")

    ![Navigate to your instance](./images/atp-instance.png " ")

2.  Click **Database Actions**.

    ![Database Actions](./images/atp-open-database-actions.png  " ")

3.  Provide the **Username - ML\_USER** and click **Next**. Then provide the password for your ML\_USER and click **Sign in**.

    ![Login](images/atp-mluser-login.png)

    ![Password](images/atp-mluser-password.png)

4. From the Database Actions menu, click **SQL**. The SQL worksheet opens.

    ![Cick SQL](./images/atp-sql.png " ")

    ![Dismiss Help](./images/sql-worksheet.png " ")

5.  To show how an application would use machine learning predictions we will add some customer names to the original credit\_scoring\_100k data set. Click on the more options icon, select **Data Loading** and then select **Upload Data Into New Table**.

    ![Upload data to a new table](./images/upload-data.png  " ")

6.  Click **Select files**, upload **customer\_names.csv** file from the install.zip you downloaded earlier and click **Next**.

    ![Select the custom file](./images/select-files-to-upload.png  " ")

    ![Check the progress of the file upload](./images/click-next.png  " ")

7.  Change the data type of **CUSTOMER\_ID - Number**. Change the lengths of **CUSTOMER\_ID - 6**, **FIRST\_NAME - 100** and **LAST\_NAME to 100** and click **Next** .

    ![Change the parameters](./images/edit-types.png  " ")

8.  Once the DDL Code is generated, click **Finish**.

    ![Click Finish](./images/click-finish.png  " ")

9. Uploading the data may take time, click **OK** to close the popup.

    ![Click OK](./images/uploading-ok.png " ")

10.  Create a view that combines the names with the credit\_scoring\_100k data set.

    ````
    <copy>
    create or replace view ml_user.credit_scoring_100k_v as select a.first_name,
    a.last_name, b.*
    from ml_user.customer_names a, credit_scoring_100k b
    where a.customer_id(+)= b.customer_id;
    </copy>
    ````

    ![Run script](./images/create-view.png  " ")

11.  Create a new *upload_customers* table. This will be used in the application to show how newly loaded records can be scored on the fly.

    ````
    <copy>
    create table upload_customers (
    customer_id number
    , first_name varchar2(100)
    , last_name varchar2(100)
    , wealth varchar2(4000)
    , income number
    , customer_dmg_segment varchar2(26)
    , customer_value_segment varchar2(26)
    , occupation varchar2(26)
    , highest_credit_card_limit number
    , delinquency_status varchar2(26)
    , max_cc_spent_amount number
    , max_cc_spent_amount_prev number
    , residental_status varchar2(26)
    , likely_good_credit_pcnt AS (round((100*(prediction_probability(n1_class_model, 'Good Credit' USING
        wealth
    , customer_dmg_segment
    , income
    , highest_credit_card_limit
    , residental_status
    , max_cc_spent_amount_prev
    , max_cc_spent_amount
    , occupation
    , delinquency_status
    , customer_value_segment
    , residental_status))),1))
    , credit_prediction AS (prediction(n1_class_model USING   
    wealth
    , customer_dmg_segment
    , income
    , highest_credit_card_limit
    , residental_status
    , max_cc_spent_amount_prev
    , max_cc_spent_amount
    , occupation
    , delinquency_status
    , customer_value_segment
    , residental_status))
    );
    </copy>
    ````

    ![Create a new table](./images/create-a-table.png  " ")

## Task 2: Import the APEX Application

1.  Click the **Navigation Menu** in the upper left, navigate to **Oracle Database**, select **Autonomous Transaction Processing**, choose your compartment and navigate to your ATP instance.

    ![Cloud menu](https://oracle-livelabs.github.io/common/images/console/database-atp.png " ")

    ![Navigate to your instance](./images/atp-instance.png " ")

2.  Click on **Tools** and click **Open APEX**.

    ![Tools and click Open APEX](./images/apex.png " ")

3.  Enter your ADMIN **Password** and click **Sign In to Administration**.

    ![Password](./images/apex-password.png  " ")

4.  You will be prompted to create a workspace. Click **Create Workspace**.

    ![Create Workspace](./images/create-workspace.png  " ")

5.  Click **Existing Schema** on the Create Workshop screen.
     ![Click Existing Schema](./images/existing-schema.png  " ")

6.  In the next screen, click the field provided for **Database User**. Enter **ML\_USER** in the field and click on the search icon. 
    ![Search for the user](./images/database-user.png  " ")

    ![Search](./images/user-search.png  " ")

7. Select **ML\_USER** from the from the search results. The next screen displays the **Create Workspace** screen with **Database Username** and auto-populated **Workspace Name**. 
    ![Select the user](./images/select-ml-user.png  " ")

    ![Review the fields](./images/autopopulated-workspace.png  " ")

8. Enter **Workspace Name - ML\_CREDIT\_APP**, **Workspace Username - ML\_USER**, and provide a **Workspace Password**. For convenience, enter the ATP instance ADMIN password. Click **Create Workspace**.
       ![Provide the details for the fields and set a password](./images/create-new-workspace.png  " ")

9. Your APEX workspace is ready to build an application! 
    ![APEX workspace](./images/workspace-created.png  " ")

10. Click **admin** in the top right corner and then click **Sign out** to sign out of ADMIN user.
    ![Sign out of ADMIN](./images/sign-out.png  " ")

11. When the popup appears, click **Return to Sign In Page**.
    ![Sign in](./images/return-signin.png  " ")

12. Log in to the **ML\_CREDIT\_APP** as an **ML\_USER**. Enter **Workspace - ML\_CREDIT\_APP**, **Username - ML\_USER**, and **Password** you created for the ATP instance, and then click **Sign In**.
        ![Login](./images/mluser-apex-signing.png  " ")

13. Your ML\_USER workspace opens. 
    ![User workspace opens](./images/workspace-opens.png  " ")

14. Select **App Builder**.

    ![Click App Builder](./images/app-builder.png  " ")

15.  Select **Import**

    ![Import](./images/import.png  " ")

16.  To import file, click the plus icon next to Drag and Drop and select **f100.sql** file from your install zip folder that you downloaded in Lab 1. Leave all other fields with their default values. 

    ![Select the custom file](./images/import-file.png  " ")

17. Once the file is uploaded, the file name appears under the Drag and Drop field. Click **Next**.
      ![Next](./images/click-next2.png  " ")

18. Click **Next** to confirm file import.
    ![Confirm the file](./images/confirm-file-import.png  " ")

19. To install database application, accept the defaults and click **Install Application**.

    ![Install Application](./images/install-app.png  " ")

20. Click **Next** to installation the application.

    ![Click Next](./images/install-next.png  " ")

21. Click **Install** to confirm the installation.
    ![Click Install](./images/final-install.png  " ")

22. Once the installation is complete, click **Run Application** to run the application.

    ![Run the application](./images/run-app.png  " ")

23. Log in as ML\_USER, enter **Username - ML\_USER** and **Password** you created for the ATP instance and then click **Sign In**.

    ![Login](./images/mluser-app-signin.png  " ")

24. The Alpha Office application opens. 
    ![Alpha Office page opens](./images/alpha_office.png  " ")


## Task 3: Run the Application and Review on-the-fly prediction/scoring

1.  On the Alpha Office homepage, select **Customer Walk-in** from the menu.

    ![Select Customer Walk-in](./images/customer-walkin.png  " ")

2. Select **Lastname**, and then **Customer Id**. Note that the credit score prediction and the probability of that estimate calculations are done as data is queried.

    ![Select the Lastname and Customer ID](./images/select-ln-fn.png  " ")

    ![Predictions](./images/data-selection.png  " ")

3.  Next, we will upload new customers and score those as a batch. Select **Home** item from the menu at the bottom of the page. (Note: If you do not see the menu bar at the bottom of the page, switch to Oracle APEX tab which was opened earlier in the browser.)

    ![Upload new customer](./images/home.png  " ")

4.  Select **SQL Workshop**.

    ![SQL Workshop](./images/select-sql-workshop.png  " ")

5.  Select **Utilities**.

    ![Utilities](./images/utilities.png  " ")

6.  Select **Data Workshop**.

    ![Data Workshop](./images/data-workshop.png  " ")

7.  Select **Load Data** and then select **Choose File**. Select the **upload\_customers.xlsx** file from your install zip file in downloads folder.

    ![Load data](./images/apex-load-data.png  " ")

    ![Upload the file](./images/choose-file.png  " ")

8.  Select **Load To - Existing Table** and choose **Table Name - UPLOAD\_CUSTOMERS** from the drop down menu and select **Load Data**. Once the data is appended to the table, close the popup.

    ![Select Existing Table and click Table field](./images/existing-file.png  " ")

    ![Search for the table](./images/select-upload-customers.png  " ")

    ![Click Load Data](./images/load-customer-data.png  " ")

    ![Check the confirmation message](./images/close-application.png  " ")

9.  In Oracle APEX, select **App Builder** and then select **Alpha Office**.

    ![App Builder](./images/select-app-builder.png  " ")

    ![Alpha Office](./images/select-alpha-office.png  " ")

10. Click **Run Application** to run the application.

    ![Run the application](./images/run-aplha-app.png  " ")

11. In Alpha Office, click on **Review Uploaded Customers** from the menu to review the uploaded customers and note the predictions.

    ![Review the uploaded customers](./images/review-uploaded-data.png  " ")

    ![Check the predictions](./images/note-predictions.png  " ")

12. Select **Customer Upload Summary** from the menu. This provides a summary measure of the uploaded customer number of new good credit versus other credit customers. *This shows there were 40 customers with 100 percent probability of good credit, 83 customers with a 50.7 percent probability of good credit, and 277 customers with a 1.2 percent probability of good credit.* (Note: your values may differ slightly.)

    ![Add a summary](./images/cust-upload-summary.png  " ")

13. Select **Overall Credit Profile**. This provides an overall measure of the credit across the entire 100k credit database. This scoring of 100k customers with 10 variables takes less than a second. *This shows Alpha Office has 12k customers with a 100 percent probability of good credit, 22k customers with a 50.7 probability of good credit, and 66k customers with a 1.2 percent probability of good credit*. (Note: your values may differ slightly.)

    ![Provide overall credit profile](./images/overall-profile-1.png  " ")

## Task 4: Expose the Machine Learning Model as a REST End Point so any Application can Call it

1.  Select the **Home** button from the menu at the bottom of the screen. (Note: If you do not see the menu bar at the bottom of the page, switch to Oracle APEX tab which was opened earlier in the browser.)

    ![Home](./images/click-home-menu.png  " ")

2.  Select **SQL Workshop**.

    ![SQL Workshop](./images/select-workshop.png  " ")

3.  Select **RESTful Services**.

    ![Select RESTful Services](./images/restful.png  " ")

    ![Page opens](./images/restful2.png  " ")

4.  Select **Modules** on the left, and then click on **Create Module**.

    ![Create a module](./images/select-modules.png  " ")

5.  Enter the following and select **Create Module**.
    - Module Name - *Predict Credit*
    - Base Path - */credit/*

    ![Enter the details](./images/create-module2.png  " ")

6.  Select **Create Template** to create a template.

    ![Create Template](./images/select-create-template.png  " ")

7.  Enter the following and select **Create Template**.
    - URI Template: *credit_scoring_100k\_v/:wealth/:income*

    ![Enter the details](./images/create-template.png  " ")

8. Next, click on **Create Handler**.

    ![Click Create Handler](./images/click-create-handler.png  " ")

9.  Be sure to select **Method - GET** and **Source Type - Query One Row** and enter the following SQL query in the worksheet and select **Create Handler**.

    ````
    <copy>
    select prediction(n1_class_model using :wealth as wealth, :income as income) credit_prediction from dual
    </copy>
    ````

    ![Run the SQL query](./images/create-handler.png  " ")

10.  Copy **Full URL** and paste it in your browser. Replace the parameters **:wealth - Rich** and **:income - 20000** respectively and hit enter. We are passing the wealth and income variables to the prediction model. Note these are just two of the many variables we could pass to the model (just add additional ones).

    ![Copy the URL](./images/copy-url.png  " ")

    ![Inspect the URL](./images/rich.png  " ")

This concludes this lab and this workshop.

## Acknowledgements

- **Author** - Derrick Cameron
- **Contributors** - Anoosha Pilli, Peter Jeffcock, Arabella Yao, Ayden Smith, Jeffrey Malcolm Jr; Mark Hornick, Sr. Director, Data Science and Oracle Machine Learning Product Management; Sherry LaMonica, Consulting Member of Technical Staff, Machine Learning; Marcos Arancibia, Senior Principal Product Manager, Machine Learning
- **Last Updated By/Date** - Sarika Surampudi, Principal User Assistance Developer, Oracle Database User Assistance Development, October 2022
