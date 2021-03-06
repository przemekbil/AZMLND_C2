# Operationalizing Machine Learning with Azure ML using Bank Marketing sample dataset

In this project, I will use Azure ML Studio to create a cloud-based machine learning production model. I will deploy the best performing model as a service on a Azure Container Instance (ACI) and I will show how to consume it using its API endpoint. I will also create and publish an Azure pipeline, which will allow me to trigger an experiment run through the pipelines HTTP Endpoint.

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


Pipeline has been created:

![image](https://user-images.githubusercontent.com/77756713/130364509-4bffa76b-6fd5-4547-9567-c2745c1d9163.png)

Pipeline endpoints:

![image](https://user-images.githubusercontent.com/77756713/130364847-49560ec6-5853-4d49-8ffe-372ea7c774d9.png)


Dataset:

![image](https://user-images.githubusercontent.com/77756713/130364919-5a6df2ab-f119-44a1-aab0-22d6f461c396.png)



REST endpoint of the pipeline is Active:

![image](https://user-images.githubusercontent.com/77756713/130364803-b2dab5c3-5286-40a4-9571-56f62311bf3c.png)


Steps run in the RunDetails Widget:

![image](https://user-images.githubusercontent.com/77756713/130364966-50f8e18b-7512-4533-b1b9-e7571021ea2f.png)

Pipeline Endpoint status is Active:

![image](https://user-images.githubusercontent.com/77756713/132136295-a99e4010-4ae8-48b6-9576-e412d3e75b4e.png)



## Screen Recording

Screencast for this project:
https://youtu.be/aW1HVwou3vk


## Future Improvement Suggestions

* As in the first Project, dataset used is heavily imbalanced with nearly 90% of the values being "No" (29,258 vs 3,692 "Yes" answers). This could lead to false high model accuracy as even dummy model that always predicts "No" would be correct in 88% of the cases. As a future improvement, we should rebalance the dataset first before training the model

