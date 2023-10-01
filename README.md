# Text_Summarizer_using_Hugging_Face



## Problem Statement

In this busy world we donot have time to go over long text or para, especially in corporate world where we have long emails. On an average if someone need to go over 50 emails a day he/she needs a solution where in entire content of email is summarized which not only saves time but also help to take quick decision. </br>

Constraint is that summary of email needs to be small but should incorporate the complete jist without missing any valuable information.


## Solution Proposed

To solve the problem we have implemented Text summarization project using Hugging Face Library.
**Model** used is google/**pegasus-cnn_dailymail** </br>
**Dataset** used is samsum from the huggingface library, https://huggingface.co/datasets/samsum </br>. 
Data is converted into .csv format as well as the one which is ready to be used by Hugging Face and stored in https://github.com/ravi0dubey/Dataset/raw/main/summarizer-data.zip </br>


## Tech Stack Used
Python </br>
FastAPI </br>
Yolov5 algorithms </br>
Docker </br>


## Infrastructure Required.
AWS S3 </br>
AWS EC2 </br>
AWS ECR </br>
Git Actions </br>
Terraform </br>


## How to run  
1. conda create -n textsum python=3.8 -y  </br>
2. conda activate textsum </br>
3. pip install -r requirements.txt </br>
4. pip install --upgrade accelerate </br>
5. pip uninstall -y transformers accelerate </br>
6. pip install transformers accelerate </br>
4. python main.py </br>

## Text Summarization Project run results

## How project was designed and build
1. Write **template.p**y which create a folder structure of our project. Within each folders, it will create the filenames where we will be writing our code. </br>
2. **setup.py** file is created where we write statement so that signLanguage folder will behave as libraries </br>
3. **Logger** module is created to write log activities respectively</br>
4. All common functionality like reading/writing of yaml files are written in utils>main.py  </br>
5. Steps to create the project. We will write code in following order for better structure </br>
  a. **Constants ->** We will first declare all constants variable to be used by each individual components in constant->training_pipeline->__init__.py  </br> </br>
  b. **entity ->**  </br>
              i. We will declare dataclass for each components in entity->config_entity.py </br>
              ii. We will declare artifacts which each components will be generating in  entity->artifact_entity.py </br> </br>
  c. **configurations->s3_operations.py ->** It has **upload_file** method to push the model to s3 bucket based on the s3-bucket name declared in the project  </br> </br>
  d. **components ->**  </br>
          i. **data_ingestion.py ->**  will fetch input sign language data from github repo, unzip it and divide images into train and test folder </br>
            It will return data_zile_file_path and feature_store_path as its artifact. Feature_store_path contains train(folder), test(folder) and data.yaml file  </br>
         ii. **data_validation.py ->** which will read the artifacts of data_ingestion and validate that it has 3 necessary components received from data_ingestion(train, test and data.yaml file) </br>.
            It will return validation_status as its artifact </br>
        iii. **model_trainer.py ->** If validation status from data_validation.py is True then it will download the modelweights from YOLOV5S and it will train the model on the sign language data using the number of epochs mentioned. I have trained using 300 epochs</br>
         iv. **model_pusher.py ->** If validation status from data_validation.py is True then it will call upload_file method of configurations-> s3_operations.py to push the trained model best.pt to **S3 bucket** for future usage  </br> </br>
         
   e. **pipeline ->** </br>
      i. **application.py** -> it stores the configuration of host and port number on which the project will run </br></br>
     ii. **training_pipeline.py ->** will call each components of the project(mentioned above) in sequence </br> </br>
   
   f. **app.py ->**  It is the main driver part of the application which calls the pipeline for training and prediction </br>
