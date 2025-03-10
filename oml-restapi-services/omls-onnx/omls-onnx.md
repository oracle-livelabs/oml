# Create and Deploy an ONNX Format model Using Oracle Machine Learning Services

## Introduction

 In this lab, you will learn how to use Oracle Machine Learning Services REST API to deploy and score with your ONNX format models.

Estimated Time: 40 minutes

### About ONNX Format Model Deployment and Scoring in Oracle Machine Learning Services

The Oracle Machine Learning Services REST API supports ONNX format model deployment through REST endpoints for:
* Classification models (both non-image models and image models)
* Clustering models
* Features Extraction models (image models)
* Regression models

Open Neural Network Exchange or ONNX is an open standard format of machine learning models. By using the Oracle Machine Learning Services REST API, you can deploy and score with your ONNX format models.

### Objectives

In this lab, you will learn how to:
* Train an open source xgboost model
* Convert the open source xgboost model to ONNX format
* Deploy the model to OML Services on Autonomous Database


### Prerequisites 

This lab assumes you have:
* OCI Cloud Shell, which has cURL installed by default. If you are using the Workshops tenancy, you get OCI Cloud Shell as part of the reservation. However, if you are in your own OCI tenancy or using a free trial account, ensure you have OCI Cloud Shell or install cURL for your operating system to run the OML Services commands.
* An Autonomous Database instance created in your account/tenancy if you are using your own tenancy or a free trial account. You should have handy the following information for your instance:
    * Your OML user name and password
    * `oml-cloud-service-location-url`
* A valid authentication token
<<<<<<< HEAD
* onnxruntime and onnxmltools
* A Conda environment with xgboost installed

## Task 1: Create and set up a Conda Environment

In this lab, you will create a Conda environment by the name xgbenv and install the libraries xgboost, onnxruntime and onnxmltools.

1. Sign into your Oracle Machine Learning UI as `ADMIN` and run the following commands:

    ```
    <copy>

    %conda
    create -n xgbenv -c conda-forge --strict-channel-priority python=3.12.6 xgboost onnxruntime onnxmltools
    upload xgbenv --overwrite -t application "OML4PY" 

    </copy>
    ```

  This command created the conda environment `xgbenv`, installs Python 3.12.6, xgboost, onnxruntime, and onnxmltools. The command also uploads the environment to Object Storage. 

2. Now, sign in as `OMLUSER` and download and activate the Conda environment `xgbenv`.

    ```
    <copy>
    %conda 

    download xgbenv
    activate xgbenv
    </copy>
    ```

3. In a Python paragraph, import the following Python packages:

    ```
    <copy>
    import xgboost as xgb
    import onnxruntime
    import onnxmltools
    </copy>
    ```

    * xgboost - A python package.
    * onnxruntime - A runtime engine to allows you to run ONNX format models.
    * onnxmltools - A Python package that allows you to convert the traditional machine learning models, for example, xgboost into the ONNX format.

This sets up the environment to create and train a xgboost model, and convert it to ONNX format.

## Task 2: Train an open source xgboost model
=======

## Task 1: Train an open source xgboost model
>>>>>>> cbc3b63332f44394cec5fa499863739bfe8bb5b9
In this lab, we will create a machine learning regression model that can predict average house price. We will use the Boston Housing price dataset. It contains the details of 506 houses in Boston.

To create and train the model:

1. Import the Diabetes dataset and store it in a variable called `diabetes`. 

  Run the following command to import the dataset:

    ```
    <copy>
<<<<<<< HEAD
    %python
=======
>>>>>>> cbc3b63332f44394cec5fa499863739bfe8bb5b9
    from sklearn.datasets import load_diabetes

    diabetes = load_diabetes()
    X = diabetes.data  # Features
    y = diabetes.target  # Target variable (progression)
    </copy>
    ```

2. Run the following command to view the description of the dataset. can be viewed by printing the contents of the .DESCR attribute.

    ```
    <copy>
<<<<<<< HEAD
    %python
    print(diabetes.DESCR)
    </copy>
=======
    print(diabetes.DESCR)
    </copy>

>>>>>>> cbc3b63332f44394cec5fa499863739bfe8bb5b9
    ```
3. Now separate the data into target and predictor variables. Then split the data into train and test sets. Run the following command. 


    ```
    <copy>
    %python
    from sklearn.model_selection import train_test_split

    x, y = diabetes.data, diabetes.target
    xtrain, xtest, ytrain, ytest=train_test_split(x, y, test_size=0.30, random_state=99)
    </copy>
    ```

  In this example, we are using the `train_test_split` function from sklearn's `model_selection` module. The test size equal to 30% of the data. A `random_state` is assigned for reproducibility.

4. Run the following command to: 
    * Build the regression model. Use the `XGBRegressor` class of the `xgboost` package with the hyper-parameter values passed as arguments. 
    * Initialize the regressor object
    * Fit the regressor to the training set 
    * Print all of the model parameters

    ```
    <copy>
    import xgboost as xgb

    model = xgb.XGBRegressor(objective ='reg:squarederror', colsample_bytree = 0.3, learning_rate = 0.1,
                max_depth = 5, alpha = 10, n_estimators = 10)
    print(model)
    </copy>
    ```

5. Run the following command to train the model using the `fit` method and make predictions using the `predict` method on the model:

    ```
    <copy>
    model.fit(xtrain, ytrain)

    pred = model.predict(xtest)
    </copy>
    ```


6. Next, compute the Root Mean Square error (RMSE) by using the `mean_squared_error` function. This function is available in the `metrics` module of sklearn. 

    ```
    <copy>
    import numpy as np
    from sklearn.metrics import mean_squared_error 

    rmse = np.sqrt(mean_squared_error(ytest, pred))

    print("RMSE: %f" % (rmse))

    </copy>
    ```

  The RMSE for the price prediction is approximately 10.4 per $1000.

  This completes the task of creating and training an open source xgboost model. 


## Task 2: Convert the open source xgboost model to ONNX format

To convert the xgboost model to ONNX, we need the model in .onnx format, zipped together with a metadata.json file. 

Before deploying an ONNX format model, you must create the ONNX model zip file. The zip file contains the following files: 

* `modelName.onnx` file 
* `metadata.json` file and 
* `label.txt` (optional) file. 




1. Run the following command to import the required libraries - onnxmltools, ZipFile, json, and FloatTensorType:

    ```
    <copy>
    import onnxmltools
    import json
    from zipfile import ZipFile
    from skl2onnx.common.data_types import FloatTensorType
    </copy>
    ```

2. Run the following command to set up the directories on the file system where the ONNX model will be created:
    ```
    <copy>
    import os
    home = os.path.expanduser('~')
    target_folder = os.path.join(home, 'onnx_test' )
    try:
      os.makedirs(target_folder)
    except:
      pass
    os.chdir(target_folder)
    </copy>
    ```

3. Now run the following command to define the model inputs: 

    ```
    <copy>
    initial_types = [('float_input', FloatTensorType([None, xtrain.shape[1]]))]
    </copy>
    ```

  In this example:

  * `initial_types` - This is an argument present in the `convert_xgboost` function. Since, `scikit-learn` does not store information about the training data, so it is not possible to retrieve the number of features or their types.
  * `float_input` - This is the name or identifier of the input tensor. Numpy array is called a tensor in ONNX. This is passed on to the model.
  * `FloatTensorType([None, xtrain.shape[1]` - Defines the type and shape of the input tensor.
    * `[None, xtrain.shape[1]]` - This is a list that defines the shape of the tensor - the number of rows and number of features (or input dimensions) for each data point.
    * `None` - This is the first dimension. It represents the number of rows. The number of rows is undefined as the the number of requested predictions is unknown at the time the model is converted.
    * `xtrain.shape[1]` - This is the second dimension. It represents the number of features (or input dimensions) for each data point.
   

4. Now that we have defined the model inputs, run the following command to convert the xgboost model to ONNX format: 
    ```
    <copy>
    onnx_model = onnxmltools.convert_xgboost(model, initial_types=initial_types)
    onnxmltools.utils.save_model(onnx_model, './xgboost_boston.onnx')
    </copy>
    ```

  In this example, we are using the `convert_xgboost` function from onnxmltools and save the model to file `xgboost.onnx`.

4. Now, run the following command to create the `metadata.json` file and compress and create the zip file by the name `onnx_xgboost.model.zip`:

    ```
    <copy>
    metadata = {
    "function": "regression",
    }

    with open('./metadata.json', mode='w') as f:
      json.dump(metadata, f)

    with ZipFile('./onnx_xgboost.model.zip', mode='w') as zf:
      zf.write('./metadata.json')
      zf.write('./xgboost_boston.onnx')
    </copy>
    ```



> **Note:** Ensure that the `metadata.json` file contains the following information:


  * `function` (mandatory for all models)
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

5. Run the followng command to view and examine the string representation of the ONNX model. It contains the version of OnnxMLTools used to create the ONNX model, and a text representation of the graph structure, including the input types that you defined in step 3.
    ```
    <copy>
    print(str(onnx_model))
    </copy>
    ```


This completes the task of creating and training an ONNX model. 

## Task 3: Validate the ONNX model by score with the data in the ONNX Runtime environment 


1. Run the following command to import the ONNX runtime environment:
    ```
    <copy>
    import onnxruntime as rt
    </copy>
    ```


2. Run the following command to set up the runtime:

    ```
    <copy>
    sess = rt.InferenceSession("xgboost_boston.onnx")
    </copy>
    ```
3. Run the following command to get model metadata to enable mapping of new input to the runtime model:

    ```
    <copy>
    input_name = sess.get_inputs()[0].name
    label_name = sess.get_outputs()[0].name
    </copy>
    ```

4. Run the following command to create predictions. The inputs are the xtest values.

    ```
    <copy>
    pred_onnx = sess.run([label_name], {input_name: xtest.astype(np.float32)})[0]
    </copy>
    ```
5. Run the following command to print first 10 predictions:
    ```
    <copy>
    print("Prediction:\n", pred_onnx[0:10])
    </copy>
    ```

This completes the task of validating the ONNX model. 



## Task 4: Deploy the model to OML Services on Autonomous Database
Before deploying the model, you must store the ONNX model in the model repository in the database. 

1. Obtain an authentication token by using your Oracle Machine Learning (OML) account credentials to send requests to OML Services. 
  
  See **Lab 1 - Authenticate your OML Account with your Autonomous Database instance to use OML Services** in this workshop for details. 


2. To store the ONNX model, send a POST request to the model repository Service. Here is an example of a `POST` request to store an ONNX format regression model: 


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

## Task 5: Deploy the ONNX Format Model
To deploy and score an ONNX format regression model: 

1. Send a `POST` request to the `/omlmod/v1/deployment` endpoint to deploy the ONNX model. The inputs for this request are the `modelId` and `URI`.

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

## Task 6: Score the ONNX Model

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

  This completes the task of deploying and scoring an ONNX regression model.


## Learn More

* [REST API for Oracle Machine Learning Services](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/index.html)
* [Work with Oracle Machine Learning ONNX Format Models](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/omls-example-onnx-ml.html)
* [Work with Oracle Machine Learning ONNX Image Models](https://docs.oracle.com/en/database/oracle/machine-learning/omlss/omlss/omls-example-onnx-image.html)
* [Create the proper ONNX files that work with OML Services](https://github.com/oracle/oracle-db-examples/blob/main/machine-learning/oml-services/SKLearn%20kMeans%20and%20GMM%20export%20to%20ONNX.ipynb)


## Acknowledgements

* **Author** - Moitreyee Hazarika, Principal UAD, Database User Assistance Development
* **Contributors** -  Mark Hornick, Senior Director, Data Science and Oracle Machine Learning Product Management; Sherry LaMonica, Consulting Member of Technical Staff, Oracle Machine Learning; Marcos Arancibia Coddou, Senior Principal Product Manager, Machine Learning
* **Last Updated By/Date** - Moitreyee Hazarika, February 2025
