# Create and Deploy an ONNX Format model Using OML Services

## Introduction

 In this lab, you will .

Estimated Time: 40 minutes

### About Oracle Machine Learning Services

OML Services extends OML functionality to support model deployment and model lifecycle management for both in-database OML models and third-party Open Neural Networks Exchange (ONNX) machine learning models via REST APIs. These third-party classification, regression or clustering models can be built using tools that support the ONNX format, which includes packages like Scikit-learn and TensorFlow, among several others.

Oracle Machine Learning Services provides REST endpoints through the Oracle Autonomous Database environment. These endpoints enable the storage of machine learning models along with their metadata, the creation of scoring endpoints for the model, and producing scores using these endpoints.

### Objectives

In this lab, you will:
* Deploy and score an ONNX format models


### Prerequisites 

* OML server name
* oml-cloud-service-location-url
* A valid authentication token
* Create the ONNX model zip file containing the `modelName.onnx` file, `metadata.json` file, and `label.txt` (optional) file. 
* Ensure that the `metadata.json` file contains the following information:
    * `function`
    * `regressionOutput` 
    * `classificationLabelOutput`
    * `classificationProbOutput` 
    * `inputTensorDimNames` 
    * `height` 
    * `width` 
    * `channel` 
    * `mean`
    * `scale` 
    * `inputChannel` 
    * `clusteringDistanceOutput` 
    * `clusteringProbOutput`


To know more about the the `metadata.json` file, see:  [Specifications for ONNX Format Models](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/onnx_spec.html)



## Task 1: Deploy an ONNX Format Model

Open Neural Network Exchange or ONNX is an open standard format of machine learning models. By using the Oracle Machine Learning Services REST API, you can deploy and score with your ONNX format models (both image and non-image).

The REST API is exposed as part of the Oracle Machine Learning Services on Oracle Autonomous Database cloud service. The Oracle Machine Learning Services REST API supports ONNX format model deployment through REST endpoints for:

   * Classification models (both non-image models and image models)
   * Clustering models (non-image models)
   * Feature Extraction models (image models)
   * Regression models (non-image models)


To deploy and score an ONNX format regression model: 
1. Create the ONNX.zip file by using the following command: 

      ```
      <copy>
      zip -r modelName.zip modelName.onnx, metadata.json, label.txt
      </copy>
      ```
    The zip file must contain the following files:
    * `modelName.onnx` file (Mandatory). This is the ONNX model file. 
    * `metadata.json` file (Mandatory). 

  Example of a metadata.json file: 

      ```
      {
      "function": "classification",
      "classificationProbOutput": "dense/Sigmoid:0",
      "inputTensorDimNames": ["batch", "height", "width", "channel"],
      "height": 224,
      "width": 224,
      "channel": 3,
      "mean": [123.68, 116.779, 103.939],
      "scale": [1.0,1.0,1.0],
      "inputChannel": "BGR"
      }
      ```
    * `label.txt` file (Optional). This file stores labels for Classification or Clustering models. The labels should be listed in the same order as the model was trained. This file is not required if the scoring outputs contain label information. It those cases, it will be ignored.

    Example of a label.txt file: 

      ```
      scorpion-winter
      cinturato-winter
      ice-zero-fr
      winter-sottozero
      ```

  
2. Obtain an authentication token by using your Oracle Machine Learning (OML) account credentials to send requests to OML Services. To authenticate and obtain a token, use `cURL` with the `-d` option to pass the credentials for your Oracle Machine Learning account against the Oracle Machine Learning user management cloud service REST endpoint `/oauth2/v1/token`. Run the following command to obtain the access token: 

    ```
    <copy>
    $ curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{"grant_type":"password", "username":"'<yourusername>'", 
    "password":"' <yourpassword>'"}'"<oml-cloud-service-location-url>/omlusers/api/oauth2/v1/token"
    </copy>
    ```  

3. Store the ONNX model in the model repository in the database by sending a POST request to the Model Repository Service. 

  Here is an example of a `POST` request to store an ONNX format regression model: 

    ```
    curl -X POST --header "Authorization: Bearer $token" 
    <oml-cloud-service-location-url>/omlmod/v1/models \
    -H 'content-type: multipart/form-data; boundary=Boundary' \
    -F modelData=@sk_rg_onnx.zip \
    -F modelName=onnxRegressionModel \
    -F modelType=ONNX \
    -F version=1.0 \
    -F 'description=onnx sk regression model version 1.' \
    -F shared=true
    ```

    > **Note:** When you store the model in the repository, a unique ID is generated. This is the `modelId` that you use when creating the model endpoint.
  
    In this example:
      * `$token` - Represents an environmental variable that is assigned to the token obtained through the Authorization API.
      * `sk_rg_onnx.zip` - This is the ONNX zip file.
      * `onnxRegressionModel` - This is the model name.

## Task 2: Deploy the ONNX Model

1. Now, send a `POST` request to the `/omlmod/v1/deployment` endpoint to deploy the ONNX model. The inputs for this request are the `modelId` and `URI`.

    > **Note:** Only the model owner can deploy the model. The model owner is the user who stores the model. A new endpoint is created for the deployed model. 

    Example of POST request to deploy an ONNX model: 

    ```
    curl -X POST --header "Authorization: Bearer $token" 
    <oml-cloud-service-location-url> /omlmod/v1/deployment \
    -H 'Content-Type: application/json' \
    -d '{\
      "modelId": "29e99b4b-cfc2-4189-952a-bd8e50fb8046",\
      "uri": "onnxrg"\
        }' 
    ```

    In this example: 
      * `$token` - Represents an environmental variable that is assigned to the token obtained through the Authorization API.
      * `29e99b4b-cfc2-4189-952a-bd8e50fb8046` - This is the `modelId`. The `modelId` is generated when you store the model in the repository.
      * `onnxrg` - This is the URI.


5. Get the Open API document for this model endpoint by sending a `GET` request to `/deployment/{uri}/api`. 

    Example of getting the Open API document of a deployed ONNX regression model:

    ```
    <copy>
    curl -X GET --header "Authorization: Bearer $token" 
    <oml-cloud-service-location-url>/omlmod/v1/deployment/onnxrg/api
    </copy>
    ```

    In this example: 
    * `$token` - Represents an environmental variable that is assigned to the token obtained through the Authorization API.
    * `onnxrg` - This is the URI.

## Task 3: Score the ONNX Model

1. Score the model by sending a POST request to the `deployment/{uri}/score` endpoint. The `GET` response to `{uri}/api` provides detailed information about the model.

    >**Note:** Prediction details are not supported for ONNX model scoring. 

    ```
    <copy>
    curl -X POST --header "Authorization: Bearer $token" 
    <oml-cloud-service-location-url> /omlmod/v1/deployment/onnxrg/score \
    -H 'Content-Type: application/json' \
    -d '{\
      "inputRecords": [\
        {\
          "input": [\
            [\
              -0.07816532,\
              0.05068012,\
              0.07786339,\
              0.05285819,\
              0.07823631,\
              0.0644473,\
              0.02655027,\
              -0.00259226,\
              0.04067226,\
              -0.00936191\
            ]\
          ]\
        },\
        {\
          "input": [\
            [\
              0.0090156,\
              0.05068012,\
              -0.03961813,\
              0.0287581,\
              0.03833367,\
              0.0735286,\
              -0.07285395,\
              0.1081111,\
              0.01556684,\
              -0.04664087\
            ]\
          ]\
        }\
      ]\
    }
    </copy>
    ```
    This is a deployed ONNX regression model with URI `onnxrg`. In this example: 
    * `$token` - Represents an environmental variable that is assigned to the token obtained through the Authorization API.
    * `POST` request is sent to `/deployment/onnxrg/score`.  

  This completes the task of scoring an ONNX regression model.





## Learn More

* [REST API for Oracle Machine Learning Services](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/index.html)
* [Work with Oracle Machine Learning ONNX Format Models](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/omls-example-onnx-ml.html)
* [Work with Oracle Machine Learning ONNX Image Models](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/omls-example-onnx-image.html)
* [Create the proper ONNX files that work with OML Services](https://github.com/oracle/oracle-db-examples/blob/main/machine-learning/oml-services/SKLearn%20kMeans%20and%20GMM%20export%20to%20ONNX.ipynb)


## Acknowledgements

* **Author** - Moitreyee Hazarika, Principal UAD, Database User Assistance Development
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Oracle Machine Learning Product Management; Sherry LaMonica, Consulting Member of Technical Staff, Oracle Machine Learning; Marcos Arancibia Coddou, Senior Principal Product Manager, Machine Learning
* **Last Updated By/Date** - Moitreyee Hazarika, February 2025
