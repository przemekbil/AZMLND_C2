# Operationalizing Machine Learning with Azure ML using Bank Marketing sample dataset

In this project, I will use Azure ML Studio to configure a cloud-based machine learning production model, deploy it and then consume it. I will also create, publish, and consume a pipeline.

## Architectural Diagram

Below image represents the key steps in the flow of operations neccesary to complete this project.

![image](https://user-images.githubusercontent.com/77756713/129484709-6c551e6c-65cb-4aa2-8aae-37ded55f7632.png)


## Key Steps

### Step 1: Authentication

This step was skipped as I didn't have the required permissions to create the Service Principal in the Udacity provided lab environment.

### Step 2: Automated ML Experiment

The Goal of this setp is to create a Model that can be deployed and consumed in Steps 3-6. 
Firstly, Bank Marketing dataset (downloaded form https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) has to be registered in Azure ML Studio:

![image](https://user-images.githubusercontent.com/77756713/129485067-602e4e18-942b-47e8-bc47-ab8fbc0e9ddf.png)

Next, a model has to be trained on this dataset either using Auto ML or provided Jupyter Notebook steps:

![image](https://user-images.githubusercontent.com/77756713/129485087-64603724-623a-4099-b7aa-15ccec4d1840.png)

After the experiment completion, the best performing Model turned out to be the VotingEnsemble:

![image](https://user-images.githubusercontent.com/77756713/129485890-efd0fc66-b1a4-4d90-816e-07a2de9fde38.png)


### Step 3: Best Model Deployment

The Model found in the Step 2 has been deployed using Azure Container Instance (ACI) with Authentication enabled as "bank-marketing-model2"

### Step 4: Logging

After successfully deploying the Best Model, the Application Insights have been enabled for the endpoint. This will enable to monitor and collect data on the following:
* Output data
* Responses
* Request rates, response times, and failure rates
* Dependency rates, response times, and failure rates
* Exceptions

The logging through Application Insights was enabled using provided script 'logs.py'. Below is the screenshot of the script running:

![image](https://user-images.githubusercontent.com/77756713/129487901-cc4ffd60-6aaa-4090-bfa0-36aa37bbaf2c.png)

Application Insights on the 'bank-marketing-model2' endpoint have been enabled and the insight url have been created:

![image](https://user-images.githubusercontent.com/77756713/129486176-4a3a1343-db6a-4fe2-b3e6-49d7b6b405b9.png)



### Step 5: Swagger Documentation

![image](https://user-images.githubusercontent.com/77756713/129488411-3381403e-0d35-44ed-9b12-74bae89a18d9.png)


### Step 6: Model Endpoint

Next, I tested the 'bank-marketing-model2' models endpoint using script 'endpoint.py' with two sample data points. It's worth noting, that the original version of this script resulted in error being returned from the endpoint and only after modyfying the order of the keys in the script to match the order in the model schema (as per [this](https://knowledge.udacity.com/questions/639437) thread on the Udacity Knowledge base), the endpoint returned correct answers: 

![image](https://user-images.githubusercontent.com/77756713/129488463-48b66190-4493-449f-bf59-938cb0277c7e.png)

Below is the result of load-testing the models's ednpoint using Apache Benchmark tool. All the requests were served within 237ms which is far less than the maximum of 60s:

![image](https://user-images.githubusercontent.com/77756713/129488470-d771519e-594c-4551-b216-528b67311d60.png)


### Step 7: Pipeline Creation



## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
