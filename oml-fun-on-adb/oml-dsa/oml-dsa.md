# Introduction to Data Science Agent

## Introduction

This lab walks you through the steps to use Data Science Agent in Oracle Machine Learning UI.

Estimated Time: X

### About Data Science Agent
Data Science Agent is an intelligent built-in conversational chatbot integrated with Oracle Machine Learning UI included in your Oracle Autonomous AI Database subscription. You must provide the LLM, specified through your AI profile, whether from a third-party AI provider, OCI GenAI Service, or one you privately host. You can run complete data science workflows using natural language in the Data Science Agent chat.

Data Science Agent helps you explore data, create views, build and deploy machine learning models through conversation, while giving you control over the objects, conversation context, and the AI profile it uses.

* **Choose the data for your conversation:** Associate relevant database objects to give the agent context and help it work more precisely and efficiently. Before creating a view, the objects it uses must be associated with the conversation or approved by you. Other supported operations can use objects available under your database privileges.
* **Manage your conversations:** Each conversation has its own context. Create conversations for your work, manage them as needed, and delete them when you’re done.
* **Select your AI profile:** Choose the AI profile, and therefore, the LLM that powers the agent. You can also switch profiles mid-conversation.
* **Approve objects for a view:** If a view requires tables or views that aren’t yet associated with the conversation, the agent will identify them and ask you to approve adding them. If you approve, the association is recorded and the agent continues. If you decline, the operation is canceled.

### Objectives

In this lab, you will learn how to:

* Access and use the Oracle Machine Learning Data Science Agent
* Create a Data Science Conversation
* Use the Data Science Agent chat interface
* Add objects to your conversation


### Prerequisites

This lab assumes you have:
* An Oracle Machine Learning account
* OML administrator role (ADMIN)
* OML developer role (OML_DEVELOPER)
* Access to Oracle Machine Learning USER account

## Task 1: Access Oracle Data Science Agent

[](include:oml-dsa-access.md)

## Task 2: Review the recommended models for use with Data Science Agent

[](include:oml-dsa-recommended-models.md)

## Task 3: Create a Data Science Agent conversation

[](include:oml-dsa-create-conversation.md)

## Task 4: Add Objects to your Conversation

[](include:oml-dsa-add-objects.md)

## Task 5: Manage AI Profile and Service Level of Data Science Conversation

[](include:oml-dsa-manage-profile-settings.md)

## Task 6: Start Chatting with Data Science Agent

[](include:oml-dsa-start-chat.md)


## Task 7: Explore Data Science Agent

* [Get Started with Oracle Data Science Agent](https://livelabs.oracle.com/ords/dbpm/r/livelabs/view-workshop?wid=4468)
* [Recommended Models](https://docs.oracle.com/en/database/oracle/machine-learning/data-science-agent/dsaug/dsa-create-ai-credential-and-profile.html#GUID-FD8053A2-5988-43B9-B024-19EEBA5CD9D4)
* [Create AI Credential and AI Profile](https://docs.oracle.com/en/database/oracle/machine-learning/data-science-agent/dsaug/dsa-create-ai-credential-and-profile.html#GUID-BEF5D9CD-2CDB-468B-A9D6-76A20D3310E1)
* [Issue Prompts to Data Science Agent](https://docs.oracle.com/en/database/oracle/machine-learning/data-science-agent/dsaug/dsa-example-model-deploy.html#GUID-BEF5D9CD-2CDB-468B-A9D6-76A20D3310E1)


## Learn More

* [Data Science Agent](https://docs.oracle.com/en/database/oracle/machine-learning/data-science-agent/index.html)
* [Oracle Machine Learning UI](https://docs.oracle.com/en/database/oracle/machine-learning/oml-notebooks/)


## Acknowledgements

* **Author** : Moitreyee Hazarika, Consulting User Assistance Developer, Database User Assistance Development
* **Contributors**: Mark Hornick, Senior Director, Data Science and Machine Learning; Marcos Arancibia Coddou, Product Manager, Oracle Data Science; Sherry LaMonica, Consulting Member of Tech Staff, Machine Learning
* **Last Updated By/Date**: Moitreyee Hazarika, November 2026
