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

## Sign Language Project run results
