# Introduction

## About this Workshop

In this workshop, you will test drive Oracle Machine Learning on Oracle Autonomous Database. You will explore OML Notebooks by creating notebooks, setting the service level, and scheduling a notebook job. In the Introduction to OML4SQL lab, you will use the SQL API via notebooks to implement a short data science project that forecasts product sales using a time series model. In the Introduction to OML4Py lab, you will use OML4Py to create a proxy object, explore and prepare data, build and evaluate models, and use those models to score data. In the next lab, Introduction to OML4R, you will use OML4R to create a proxy object, explore and prepare data, build and evaluate models, and use those models to score data.  In the Introduction to OML AutoML UI lab, you will use the no-code user interface OML AutoML UI to run an experiment that builds and ranks models, and deploy a Naive-Bayes classifier. You will also explore a notebook generated from a model produced by the experiment. Finally, in the Introduction to Oracle Machine Learning Services lab, you will score singleton and mini-batch records with a Naive-Bayes classifier and use the Cognitive Text feature to analyze a text string.

Estimated Workshop Time: 02 hours 30 minutes

### Objectives

In this workshop, you will learn how to:

* Use OML Notebooks
    * Create a notebook based on an example template
    * Check and update the service levels for a notebook
    * Schedule a notebook to run at a specific time using the Jobs interface
* Build a time series model and view forecasts using OML4SQL
    * Explore data using SQL queries
    * Build a time series-based model
    * Evaluate the time series model using standard diagnostic metrics provided by OML4SQL
    * Access the forecasts from the DM$VP model view
* Build a decision tree model and classify data using OML4Py
    * Create a table and proxy to the table
    * Explore and prepare data
    * Build and evaluate a Decision Tree classifier
    * Score with the classifier
* Build a decision tree model and classify data using OML4R
    * Create a table and proxy to the table
    * Explore and prepare data
    * Build and evaluate a Decision Tree classifier
    * Score with the classifier
* Use OML AutoML UI
    * Create an experiment, adjust experiment settings and run the experiment
    * Deploy top model to OML Services
    * View the OML Models user interface with deployed metadata and endpoint JSON
    * Create a notebook for the top model
    * View generated notebook and individual paragraphs
* Use OML Services
    * Obtain an authentication token to use OML Services via REST endpoints
    * Get a model scoring endpoint
    * Perform singleton and mini-batch scoring using the scoring endpoint
    * Discover keywords and summaries for a text string using the Cognitive Text feature
* Use OML Monitoring
    * Create a data monitor to monitor your data
    * Create a model monitor to monitor your machine learning models

### Prerequisites

This workshop assumes you have:
* An Oracle Cloud Account with an Autonomous Database instance and Cloud Shell.

> **Note:** If you have a **Free Trial** account, when your Free Trial expires your account will be converted to an **Always Free** account. You will not be able to conduct Free Tier workshops unless the Always Free environment is available. [Click here for the Free Tier FAQ page](https://www.oracle.com/cloud/free/faq.html).

## Acknowledgements

* **Authors** - Moitreyee Hazarika, Principal User Assistance Developer; Sarika Surampudi, Principal User Assistance Developer; Dhanish Kumar, Member Technical Staff; Suresh Rajan, Senior Manager, Oracle Database User Assistance Development
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Oracle Machine Learning Product Management; Sherry LaMonica, Consulting Member of Technical Staff, Oracle Machine Learning; Marcos Arancibia Coddou, Senior Principal Product Manager, Machine Learning
* **Last Updated By/Date** - Moitreyee Hazarika, Dhanish Kumar, November 2024
