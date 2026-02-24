# Score a Mini Batch Using Cognitive Image Functionality in Oracle Machine Learning Services

## Introduction

 In this lab, you will 

Estimated Time: 40 minutes

### About Cognitive Image Functionality in Oracle Machine Learning Services


### Objectives

In this lab, you will:

* Use the Cognitive Image Functionality to Score a Mini Batch
* Score a mini batch of images that are converted to base64 encoded strings


### Prerequisites

This lab assumes you have:
* OCI Cloud Shell, which has cURL installed by default. If you are using the Workshops tenancy, you get OCI Cloud Shell as part of the reservation. However, if you are in your own OCI tenancy or using a free trial account, ensure you have OCI Cloud Shell or install cURL for your operating system to run the OML Services commands.
* An Autonomous AI Database instance created in your account/tenancy if you are using your own tenancy or a free trial account. You should have handy the following information for your instance:
    * Your OML user name and password
    * `oml-cloud-service-location-url`
* Completed all previous labs successfully.




## Task 1: Use the Cognitive Image Functionality to Score a Mini Batch Containing base64 Encoded String

OML Services supports base64 encoded strings and image tensors (matrices) for image classification. Use the mini batch functionality to score multiple images, since it is not possible to upload multiple image files directly. Here, the images must be first converted to some form of text.

> **Note:** OML Services supports batch scoring for regression, classification, clustering, and feature extraction models.

The cognitive image module accepts three different kinds of image inputs - the image file, image tensors, and base64 encoded strings. You must then send a `POST` request to the `cognitive-image/classification` endpoint. 

This functionality uses the prebuilt ONNX image model for scoring.

In this example, you will learn how to:
* Score a mini batch of images that are converted to base64 encoded strings. 

**Prerequisites:**

* Image Files. This example uses the following image files `cat.jpg`, `dog.jpg`, `flowers.jpg`. 
* Bash script `test.sh`. The bash script contains the json code, and is stored locally. 
* A valid authentication token
* oml-cloud-service-location-url 

To score a mini batch:

1. First, run the following command to view the content of the bash script:
    ```
    <copy>
    $ cat test.sh
    #!/bin/bash
    </copy>

    ```

    The script returns the following:

    ```
    # get the paths to the images
    image1="./cat.jpg"
    image2="./dog.jpg"
    image3="./flowers.jpg"
    # get base64 encoded strings
    base64_1=`base64 -w 0 $image1`
    base64_2=`base64 -w 0 $image2`
    base64_3=`base64 -w 0 $image3`
    json_data=$(mktemp)
    echo "{\"inputRecords\":[
    {\"input\": \"$base64_1\"},
    {\"input\": \"$base64_2\"},
    {\"input\": \"$base64_3\"}], \"topN\":3}" > "$json_data"
    ```


    In this script:


    * The paths to the images are set here: 
         ```
        image1="./cat.jpg"
        image2="./dog.jpg"
        image3="./flowers.jpg"

        ```

    * Then, the .jpg images are converted to base64 encoded strings. 
         ```      
        base64_1=`base64 -w 0 $image1`
        base64_2=`base64 -w 0 $image2`
        base64_3=`base64 -w 0 $image3`
        ```



    * In this part of the script, a temporary file `json_data` is created to which the input records comprising the base64 strings are added. It also specifies the topN class, and uses the `@$json_data` argument to fetch the content. During image classification, for each image the possible classes that the image belongs to will be sorted based on the probability. 


        ```
        json_data=$(mktemp)
        echo "{\"inputRecords\":[
        {\"input\": \"$base64_1\"},
        {\"input\": \"$base64_2\"},
        {\"input\": \"$base64_3\"}], \"topN\":3}" > "$json_data"
        ```


2. Next, run the following command to make the `test.sh` file executable: 

    ```
    <copy>
    $ chmod +x test.sh 
    </copy>

    ```

3. Run the bash script file `test.sh`: 

    ```
    <copy>
    ./test.sh 
    </copy>

    ```


4. Obtain an authentication token by using your Oracle Machine Learning (OML) account credentials to send requests to OML Services. To authenticate and obtain a token, use `cURL` with the `-d` option to pass the credentials for your Oracle Machine Learning account against the Oracle Machine Learning user management cloud service REST endpoint `/oauth2/v1/token`. Run the following command to obtain the access token: 

    ```
    <copy>
    $ curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"grant_type":"password", "username":"'<yourusername>'", 
    "password":"' <yourpassword>'"}'"<oml-cloud-service-location-url>/omlusers/api/oauth2/v1/token"
    </copy>
    ```  

5. Run the following cURL command to score a mini batch containing base64 encoded strings in the input record: 

    ```
    <copy>
    $ curl -X POST "${omlservice}/omlmod/v1/cognitive-image/classification" \
    --header "Authorization: Bearer $token" \
    --header 'Content-Type: application/json' --data "@$json_data" | jq
    </copy>
    ```


    >**Note:** Here, you use the flag `--data` to fetch the input records from the temporary file `json_data`. Since the base64 encoded strings are very long, the application will throw the error Argument list too long. Hence, it is recommended to use the `--data` flag. 

    In this example, you use the Linux utility `jq` to parse the JSON output into a readable format. 
    
    >**Note:** The `jq` utility is included in all the major Linux distribution repositories. On Oracle Linux and Red Hat systems, you can install this utility by running this command: 

    ```
    <copy>
    $ sudo yum install jq
    </copy>
    ```

  _Sample Response:_

  The command returns the following scoring results:

    ```
        {
      "scoringResults": [
        {
          "classifications": [
            {
              "label": "Cat",
              "probability": 0.9999594688415527
            },
            {
              "label": "Felidae",
              "probability": 0.9997081756591797
            },
            {
              "label": "Whiskers",
              "probability": 0.9994940757751465
            }
          ]
        },
        {
          "classifications": [
            {
              "label": "Snout",
              "probability": 0.9996676445007324
            },
            {
              "label": "Dog",
              "probability": 0.996991753578186
            },
            {
              "label": "Vertebrate",
              "probability": 0.9954350590705872
            }
          ]
        },
        {
          "classifications": [
            {
              "label": "Pink",
              "probability": 0.9993271827697754
            },
            {
              "label": "Flowering plant",
              "probability": 0.9986602663993835
            },
            {
              "label": "Flower",
              "probability": 0.99815833568573
            }
          ]
        }
      ]
    }


    ```

The scoring result computes the probability of each label. This completes the task of scoring a mini batch containing base64 encoded strings in the input record. 




## Learn More

* [REST API for Oracle Machine Learning Services](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/index.html)
* [Work with Batch Scoring](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/omls-batch-scoring.html)



## Acknowledgements

* **Author** : Moitreyee Hazarika, Consulting User Assistance Developer, Database User Assistance Development
* **Contributors**: Mark Hornick, Senior Director, Data Science and Machine Learning; Marcos Arancibia Coddou, Product Manager, Oracle Data Science; Sherry LaMonica, Consulting Member of Tech Staff, Machine Learning
* **Last Updated By/Date**: Moitreyee Hazarika, October 2025
