#MachineLearningPipeline

## Projrct : Data pipline with DVC and MLFlow for machine Learning

this project demonstrates how to build an end-to-end machine learning pipline using DVC (Data version control) for data and model versioning, and MLflow for experiment tracking.
The pipeline focuses on training a randome forest classifier on pima indians Diabetes dataset, with clear stages for data preprocessing , model training and evaluation.

Key features of the project :  data version control(DVC):

DVC is used to track and version the dataset, model, and pipline stages, ensuring reproducibality across different envirnoments. the pipline is structured into stages(preprocessing,training,evaluation) that can be automatically re-executed if any dependiencies change(e.g data,script,or parameters). 
DVC also allows remote data storage(eg, DagsHub,s3) for large data sets and models.Experiment tracking with MLflow.

Mlflow is used to track experiment metrics,parameters,and artificats.It logs the hyperparameter of the model(e,g, n_estimators,max_depth)  and performance metrics like accuracy.
MLflow helps compare different runs and models to optimize the ML Pipeline.
pipelineStages:
preprocessing:

the preprocessing.py script reads the raw dataset(data/raw/data.csv), performs basic preprocessing(such as remanimng columns) and outputs the processed data to data/processed/data.csv.
this stage ensures taht data is consistently processed across runs.
Training:

the train.py script trains a Random Forest Classifier on the preprocessed data.
The mdoel is saved as models/random_forest.pkl
Hyperparamters and the model itself are logged into MLFlow for tracking and comparasion.

Evaluation:

The evaluate.py script loads the trained model anf evaluates its performance (accuracy) on the data set.
the evaluation metrics are logged to ML flow for tracking.

Goals:
Reproduciblity : by using DVC, the pipline ensures that the same data , parameters and code are reproduce the same results, making the workflow reliable and consistent.
ExperimentationL MLfloe allows users to easily track different experiments (with varying hyperparameters)  and compare the performance of models.
Collaboration: DVC and MLFlow enable smooth collabaration in a team environment , where different suers can work on the same project and track changes seamlessly.

use cases:
Data science Teams: Teams can use this project setup to track datasets, models , and experiemnets in a reproduciable and orginized manner.
Machine Learning Research:  Researches can quickly iterate over differeny experiments track performance metrics, and manage data versions effectively.

Technology Stack:
Python:  The core programming language for data processing, model training, and evaluation.
DVC: For version control of dta , models, and pipelines stages.
MLflow: For logging and tracking experiemnets , metrics, and model artifacts.
Sckit-learn: For building anf training the random Forest classifier.
This project demonsatrates how to manage the lifecycle of a machinelearning project, ensuring that data , code,models and experiements are all tracked versioned anf reproducable.


### For Adding stages

dvc stage add -n preprocess \ 
    -p preprocess.input, preprocess.output \ 
    -d src/preprocess.py -d data/raw/data.csv \ 
    -o data/processed/data.csv \
    python src/preprocess.py

dvc stage add -n tain \ 
    -p train.data,train.model,train.random_state,train.n_estimators,train.max_depth \
    -d src/train.py -d data/raw/data.csv \
    -o models/model.pkl \
    python src/train.py

dvc stage add -n evaluate \
    -d src/evalaute.py -d models/model.pkl -d data/raw/data.csv \
    python src/evalaute.py