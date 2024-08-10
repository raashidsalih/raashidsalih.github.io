+++
title = 'How I Implemented a Complete Churn Analytics Solution'
date = 2023-09-07T17:03:56+05:30
draft = false
tags = ["Project"]
+++

{{< github repo="raashidsalih/churn-pipeline" >}}

An end to end data pipeline that models and visualizes customer churn. The project blends together concepts from data engineering, analytics engineering, and machine learning. Please check the README for a more detailed exposition on the project.

![Dashboard Preview](https://github.com/raashidsalih/churn-pipeline/blob/main/assets/dashboard.png)

The dashboard [can be accessed here.](http://34.134.216.50:3000/public/dashboard/085f43f1-3301-45e2-b4f5-5bd0d789d1f3)

# Overview

This project was developed to:

- Employ various elements of the modern data stack
- Be applied to a somewhat realistic business use case
- Serve as a template for others to learn from and use via extensive documentation

## Project Architecture

![Architecture Diagram](https://raw.githubusercontent.com/raashidsalih/churn-pipeline/main/assets/architecture.svg)

For this project, the Telco Customer Churn data module which is a sample dataset on IBM's Cognos Analytics platform is used. This seemed like the best representative considering the difficulty in finding a decent dataset for the use case.

The dataset is then used to train two models. The first is a Gaussian Copula Synthesizer to produce synthetic data with characteristics similar to the original. This is done since there is not much data to go around and serves as a rudimentary imitation of data entering the database, The second is an LGBModel which is a product of using FLAML's AutoML implementation on the data, and its purpose is to predict churn status for a particular user.

Both models are hosted via FastAPI and are accessed this way. Airflow is then used to orchestrate the pulling of data from the Synthesizer, obtaining churn status prediction for said data from the classification model, generating a ULID for each customer, and writing it all to a Postgres database. Airflow is also used to trigger dbt afterward to run tests and apply necessary transformations. The data is modeled after the star schema and is finally visualized as a dashboard using Metabase.

![Data Model Diagram](https://raw.githubusercontent.com/raashidsalih/churn-pipeline/main/assets/star.svg)

Almost all of the services above run in their own docker containers, as seen in the diagram. These containers are running on a GCP VM, provisioned via Terraform. Finally, GitHub Actions facilitates CD, as changes made to this repo are reflected in the VM.

## Some points to consider

 1. Keep in mind that predicting customer churn is a rather sophisticated use case and is difficult to get right. AutoML was only employed to obtain a viable baseline model, and this is how it should be applied most of the time. Check out [Fighting Churn with Data](https://www.amazon.com/Fighting-Churn-Data-Carl-Gold/dp/161729652X) for a more comprehensive outlook on modelling customer churn.
 2. The original dataset has been transformed and [can be found here](https://github.com/raashidsalih/churn-pipeline/tree/main/source_data). This is the dataset the models have been trained on.
 3. As mentioned earlier, the various elements which comprise this project have tried to be documented. Most files should have accompanying comments to facilitate understanding, and more high level system design justifications can be found in the README.
