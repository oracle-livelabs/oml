# Introduction

## About this Workshop

In this workshop, you will explore how to pick a good wine for under $30 using Oracle Autonomous Database, Oracle Machine Learning, and Oracle Analytics Cloud with a simple use case.

Estimated Lab Time: 90 minutes

### About Product/Technology

Oracle Machine Learning (OML) extends Oracle Database and Autonomous Database and enables users to build machine learning-enhanced applications and dashboards. OML delivers powerful in-database machine learning algorithms via SQL, Python, and R APIs, and broader integration with open-source Python and R. In this workshop, you will build and apply an in-database ML model inside your Oracle Autonomous Database instance and visualize the data using Oracle Analytics Cloud.

![](./images/oml-circle.png =50%x*)

Use Oracle Machine Learning Notebooks to:
- Apply Machine Learning models for predictions
- Understand the business problem
- Explore data
- Identify key attributes
- Build and test predictive machine learning models
- Apply machine learning models to make predictions
Further investigate insights and predictions using Oracle Analytics Cloud, Interactive analytics, charts, and more.

![](./images/crisp-dm-process-diagram-dark.png)

### Workshop Scenario

Let's take a scenario where we are invited to a party and want to bring something to contribute. A bottle of wine is always a good option. But which one? There are so many! And we don't want to break the bank for our party gift. Let's try to use the Autonomous Database, Oracle Machine Learning, and Oracle Analytics Cloud to find a likely tasty and likely to be rated greater than 90 Points (GT\_90\_Points) adult beverage that is also affordable (Under $30).

We build, evaluate and apply Oracle Machine Learning models using random forests and support vector machine classification techniques that identify likely good (GT\_90\_Points) bottles of wine. This demo highlights OML's text mining capabilities and its tight integration with Oracle Analytics Cloud for interactively exploring insights and predictions.

### Objectives

In this lab, you will:
* Provision an Oracle Autonomous Database Instance
* Run a notebook in OML Notebooks
* Provision an Oracle Analytics Cloud Instance
* Visualize the Data

### Prerequisites

* An Oracle Cloud Account - Please view this workshop's LiveLabs landing page to see which environments are supported

*Note: If you have a **Free Trial** account, when your Free Trial expires your account will be converted to an **Always Free** account. You will not be able to conduct Free Tier workshops unless the Always Free environment is available. **[Click here for the Free Tier FAQ page.](https://www.oracle.com/cloud/free/faq.html)***

<if type="livelabs">

## Task 1: Launch the workshop

*Note: It takes approximately 20 minutes to create your workshop environment.*

1. After you receive the e-mail stating that your environment is ready, go back to the My Reservations page and click **Launch Workshop**.

    ![](images/my-reservations.png)

2. Make a mental note of the compartment name assigned to you. Click **Copy Password** to copy your initial password. Note that this is also the admin password for your Autonomous Data Warehouse instance, so save the password for later use. Then click **Launch Console**.

    ![](images/launch-page.png)

3. On the login page, use the Oracle Cloud Infrastructure direct sign-in, paste the password and click **Sign In**.

    ![](images/login-console.png)

4. Change your password by entering your existing password and entering a new password. Note: you can use your existing password as the new password also.

    ![](images/change-password.png)

5. Click on the menu icon and then select Autonomous Data Warehouse from the menu.

	![](https://oracle-livelabs.github.io/common/images/console/database-adw.png " ")

6. Select the compartment assigned to you from the **List Scope menu** and then click the ADW instance.

    ![](images/select-compartment.png)

    ![](images/adw-instance.png)

You may now [proceed to the next lab](#next).

</if>

## Learn More

* [Picking a good inexpensive wine <$30 using Oracle Machine Learning blog](https://blogs.oracle.com/machinelearning/picking-a-good-inexpensive-wine-%3c30-using-oracle-machine-learning)
* [Preview this video about picking a good inexpensive wine using Oracle Machine Learning.](https://www.youtube.com/watch?v=nKDWODevoKQ&t=5s)

## Acknowledgements

* **Author** - Charlie Berger, Senior Director of Product Management, Machine Learning, AI and Cognitive Analytics
* **Contributors** -  Anoosha Pilli & Didi Han, Database Product Management
* **Last Updated By/Date** - Didi Han, Database Product Management,  April 2021