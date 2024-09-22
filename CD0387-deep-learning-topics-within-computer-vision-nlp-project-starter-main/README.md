# Image Classification using AWS SageMaker

In this project, we use AWS Sagemaker to train a pretrained model that can perform image classification by using the Sagemaker profiling, debugger, hyperparameter tuning and other good ML engineering practices. This can be done on either the provided dog breed classication data set or one of your choice.

## Project Set Up and Installation
Enter AWS through the gateway in the course and open SageMaker Studio. 
Download the starter files.
Download/Make the dataset available. 

## Dataset
The provided dataset is the dogbreed classification dataset which can be found in the classroom.
The project is designed to be dataset independent so if there is a dataset that is more interesting or relevant to your work, you are welcome to use it to complete the project.

### Access
The data was uploaded to an S3 bucket through AWS S3 so that SageMaker has access to the data. 

## Hyperparameter Tuning
I chose to use ResNet18 for since it seems light weight compared to the other ResNet models while having plenty of computational power to do the job. Give an overview of the types of parameters and their ranges used for the hyperparameter search}

- Include a screenshot of completed training jobs
![Screenshot 2024-09-21 at 15-40-57 Training jobs Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/de7a71bf-688a-4f9c-b95e-2b342fe8a640)


![Screenshot 2024-09-21 at 15-42-21 pytorch-training-240921-1713 Hyperparameter tuning jobs Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/936e0e8a-d58f-4e43-8b5c-b09ee148a765)

- Logs metrics during the training process
![Screenshot 2024-09-21 at 22-14-25 pytorch-training-2024-09-21-18-30-14-961 Training jobs Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/c56a1111-ef4a-4365-b3f5-3384c1f77280)

- Tune at least two hyperparameters and Retrieved the best best hyperparameters from all your training jobs
![Screenshot 2024-09-21 at 15-54-57 pytorch-training-240921-1713 Hyperparameter tuning jobs Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/46b0c205-8cfb-4901-90f1-c5b7f3ff07e7)
![Screenshot 2024-09-21 at 16-07-56 CloudWatch us-east-1](https://github.com/user-attachments/assets/3b8e0e52-3ff7-4f46-ba82-e10009c5599d)


## Debugging and Profiling
**TODO**: Give an overview of how you performed model debugging and profiling in Sagemaker

### Results
**TODO**: What are the results/insights did you get by profiling/debugging your model?
![Screenshot 2024-09-21 at 15-58-17 sagemaker-us-east-1-476045913561 - S3 bucket S3 us-east-1](https://github.com/user-attachments/assets/14685240-321a-4aea-b2ed-b21d97f848b2)

![Screenshot 2024-09-21 at 22-34-59 AWS_MLEud_CD0387-deep-learning-topics-within-computer-vision-nlp-project-starter-main_train_and_deploy ipynb at main · AkramAls_AWS_MLEud](https://github.com/user-attachments/assets/0813fd06-697c-440f-a11b-7dac5f551ede)


**TODO** Remember to provide the profiler html/pdf file in your submission.

[Uploading profiler-report.html…]()

## Model Deployment
**TODO**: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.

**TODO** Remember to provide a screenshot of the deployed active endpoint in Sagemaker.
![Screenshot 2024-09-21 at 16-08-53 Endpoint configuration Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/5ed2e4cf-0920-4a27-a470-2787a2fd9a45)

## Standout Suggestions
**TODO (Optional):** This is where you can provide information about any standout suggestions that you have attempted.
