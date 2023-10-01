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
4. python app.py </br>

## Text Summarization Project run results
Training logs
![image](https://github.com/ravi0dubey/Text_Summarizer_using_Hugging_Face/assets/38419795/fc1ffe55-2f96-4593-8cdd-c3f9683ded64)

Training results
![image](https://github.com/ravi0dubey/Text_Summarizer_using_Hugging_Face/assets/38419795/f8b9a816-1531-4a7a-9a3d-fc8cec8d8389)

**Prediction**
**Input text given for summarization:**
Surrey Place is seeking a Data Scientist. The successful applicant will report to the VP, Adult Services. This position will work with both internal and external databases. Internally, this position will design data models and analyze existing data to address gaps in clinical services and forecast future needs. This position will also work with external data sets to address questions related to the broader service sector.

**Summarized text given by model :**
Surrey Place is seeking a Data Scientist .<n>The successful applicant will report to the VP, Adult Services .<n>This position will work with both internal and external databases .

![image](https://github.com/ravi0dubey/Text_Summarizer_using_Hugging_Face/assets/38419795/89163454-f442-4e56-9577-2f0a17b476aa)


## How project was designed and build
1. Write **template.p**y which create a folder structure of our project. Within each folders, it will create the filenames where we will be writing our code. </br>
2. **setup.py** file is created where we write statement so that signLanguage folder will behave as libraries </br>
3. **Logger** module is created to write log activities respectively</br>
4. **config->config.yaml** contains all the constant variables declared over here which will be used by all major components in step **6 e**.
5. **params.yaml** -> it contains the training parameters declared over here
6. **src->text_summarizer->** Steps to create the project. We will write code in following order for better structure under  </br>
  a.  **config-> configuration.py ->** It has returns all the artifacts of each components module detailed below  </br> </br>
  b. **Constants->__init__.py** it contains the  config_file_path(config->config.yaml) and params_file_path(params.yaml) </br> </br>
  c. **entity->__init__.py**  We will declare dataclass for each components in entity->config_entity.py </br>
  d. **utils>main.py** All common functionality like reading/writing of yaml files are written in   </br>          
  e. **components ->**  </br>
          i. **data_ingestion.py ->**  will fetch input data kept in github repo and store it in data.zip folder. Then it will unzip the data.zip folder into train,test and validation folder with respective data into it. </br>
            It will return url of data, root directory, data_zile_file_path and data_ingestion_path as its artifact. data_ingestion folder contains train(folder), test(folder) and **validation(folder)**   </br>
         ii. **data_validation.py ->** which will read the artifacts of data_ingestion and validate that it has 3 necessary folders in the data_ingestion/samsun_dataset, train, test and validation folders. If yes then it will write "True" in status.txt signalling all
             is good </br>.It will return root directory, status_file and all_required_files as its artifact </br>
         iii. **data_transformation.py ->** which will read the artifacts of data_validation and apply embeddings on the entire samsun dataset to convert it to numbers.  </br>.
               After applying  embedding technique we have 3 additional columns( input_ids, attention_mask and labels). Attention_mask signifies which word in the text has more significance i.e. "Ravi need help.""  in this case "Ravi" and "help" has more attention 
               compared to "need" hence attention mask will have value '1',0,1 with 1 signifiying higher or more attention and 0 = no attention </br>
              It will return root directory of artifacts and each folder(Train, test and validations) will have embedded data </br>
        iii. **model_trainer.py ->**  Data Collator is used to take data in batches instead of taking complete data at once. Sequence2sequence is used to train the models in 1 epochs</br>
         iv. **model_evaluation.py ->** Once model is trained then evaluation is done to see the accuracy and loss </br> </br>
         
   f. **pipeline ->** </br>
      i. **stage_01_data_ingestion.py** -> it will call data_ingestion.py </br></br>
     ii. **stage_02_data_validation.py** -> it will call data_validation.py </br></br>
    iii. **stage_03_data_transformation.py** -> it will call data_transformation.py </br></br>
    iv. **stage_04_model_trainer.py** -> it will call model_trainer.py </br></br>
    v. **stage_05_model_evaluation.py** -> it will call model_evaluation.py </br></br>
    vi. **prediction.py** -> it gets triggered from app.py when predict is chosen. It does the actual prediction i.e Text summarization</br></br>

7. **mainpy ->**  It calls all the pipeline for training except the prediction </br>
8. **app.py ->**  It is the main driver part of the application which calls the main.py for training and call prediction.py for prediction </br>
