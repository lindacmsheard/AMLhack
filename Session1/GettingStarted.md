# Getting Started with Azure Maching Learning

## Agenda
11:00 -12:00 – Overview / Demo of AML key features and approach to ML Ops

(Same teams meeting available but optional for afternoon session, feel free to go chat-only)

- 13:00 – Intros: Linda / Luke / Remko
- 13:10 – 13:30:  Hands-on: Focus: get set up with access in AML, orientation, tips for working with AML
- 13:30 – 13:45  optional Teams Call check-in – any issues?
- 13:45 – 15:00  Hands-on / self paced guide: Focus: AML basics with tutorials – Python SDK
- 15:00 – 15:15  optional Teams Call check-in – any issues?
- 15:15 – 16:00  Design session/open discussion: given learnings, what would a follow on hack look like, can we start sharing sample data for it.


## Introduction
AML has a visual designer mode, and a Python SDK. The availability of these modes depends on the service tier.

| |Basic | Enterprise |
|--|--|--|
| Status| generally available (GA)| Preview|
| Visual designer|No| Yes|
| python SDK | Yes | Yes

*(Status as of March 2020 - see [here](https://azure.microsoft.com/en-gb/pricing/details/machine-learning/) for full details)*

## Setup and Provisioning

When creating an instance of AML in your subscription, it will automatically create the following associated services:
- Storage Account
- Key Vault
- App Insights

These follow a naming convention of `youramlservicename`+`unique 10 digit identifier`, where any non-alpha-numeric characters are stripped from your AML service name.

<img src="img/aml_associated_services.jpg" width=250>

Follow the steps here (initial set up)
https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-1st-experiment-sdk-setup

## 1. Working from cloud-based Azure Machine Learning compute
Use Tutorial 1 to get familiar with working fully in the cloud. In this tutorial, the data prepation, training orchestration and training steps all happen on the AML compute VM. 
https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-1st-experiment-sdk-train


## 2. Working with a local development environment
For the second tutorial, set up a local development environment for data preparation and orchestration. The training step in this tutorial uses a cluster of VMs in the cloud.

*(Instructions adapated from 
https://docs.microsoft.com/en-gb/azure/machine-learning/how-to-configure-environment#local)*

Start by creating a conda environment.

> If you don't yet have conda on you system, follow the instructions here: https://gist.github.com/lindacmsheard/928b21764d0fa2c1324804de9e38953e

Then, from your conda base environment:
```
conda create -n myenv python=3.6.5
conda activate myenv
```
Install the required packages for this lab into the environment.
```
conda install matplotlib, numpy, scikit-learn=0.22.1
conda install notebook ipykernel
```
Enable jupyter notebook to connect to the conda python kernel. 
```
ipython kernel --user --name osaml --display-name "Python (myenv)"
```
Add the Azure ML SDK.
```
pip install azureml-sdk[notebooks]
pip install --user azure-opendatasets
```
Clone the sample tutorial notebooks and navigate `tutorials` subfolder.
```
git clone https://github.com/Azure/MachineLearningNotebooks.git
cd ./MachineLearningNotebooks/tutorials
```

Launch jupyter:
```
jupyter notebook
```
and locate the MNIST tutorial notebooks (`image-classification-mnist-data`) in the jupyter file tree browser. Follow the instructions within the tutotial notebooks.

- [Part 1 - Train with a cluster](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part1-training.ipynb)

> TIP: If you are working within the `image-classification-mnist-data` folder of the cloned repo, rename the provided model pkl file before continuing to part 2
```
mv sklearn_mnist_model.pkl sklearn_mnist_model-sample.pkl
```
- [Part2 - Deploy Model](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/image-classification-mnist-data/img-classification-part2-deploy.ipynb)


## 3. Learning to work with ML Pipelines
https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-pipeline-batch-scoring-classification

Update your environment with the following packages
```
conda install pandas requests
```
and 
```
pip install azureml-pipeline-core azureml-contrib-pipeline-steps
```
In the jupyter file tree browser, navigate to the 
[machine-learning-pipelines-advanced](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials/machine-learning-pipelines-advanced) folder.


# Advanced topics
In the labs above, we looked at integrating with Azure ML from a cloud VM, and from a local development environment. It is also possible to integrate from a spark-environment databricks:

- https://docs.microsoft.com/en-gb/azure/machine-learning/how-to-configure-environment#azure-databricks

Databricks-based notebook samples
- https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks

Alternatively, databricks can be configured as the compute target:
- https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/databricks-as-remote-compute-target/aml-pipelines-use-databricks-as-compute-target.ipynb





# Reference Links
1. [Service Pricing](https://azure.microsoft.com/en-gb/pricing/details/machine-learning/) 
2.  Configuring your environment
    - https://docs.microsoft.com/en-gb/azure/machine-learning/how-to-configure-environment
    - Alternative notebook-based instructions for configuring environments: https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb

2.  Tutorials
    -	https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-1st-experiment-sdk-setup
    -	https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-train-models-with-aml
    -	https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-deploy-models-with-aml
    - https://docs.microsoft.com/en-gb/azure/machine-learning/tutorial-train-deploy-model-cli 
3.  Notebook Samples 
    - https://github.com/Azure/MachineLearningNotebooks
4.  Python SDK Reference
    - https://docs.microsoft.com/en-gb/python/api/overview/azure/ml/?view=azure-ml-py
5.  Working with your own datasets
    - https://docs.microsoft.com/en-us/azure/machine-learning/how-to-create-register-datasets






<hr>

### Troubleshooting

#### Login issues

https://github.com/Azure/azure-cli/issues/6962
This will happen if your account is associated with multiple Azure AD's. It can exist as a member user in one Azure AD, and as a guest user in the other Azure AD. If the Azure AD that you are a guest user in requires MFA, this error will occur.

You can mitigate this by adding logging in via the the Azure AD tenant to the login:
```
az login --tenant [tenantid]
```
To achieve this when using the device login triggered within a jupyter notebook, clear the browser history. Then use the shell access available in the built-in notebook interface to issue to command above, and refresh the page before running the notebook again.


#### Remote VSCode
https://code.visualstudio.com/docs/remote/ssh