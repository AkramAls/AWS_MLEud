# Operationalizing Machine Learning on SageMaker by Akram Al-Selwadi

## Initial Setup

I'm using the default SageMaker instance ml.t3.medium when Notebook Instances are created for my notebook as I will be only running the notebook only when working on the project, and I do not need a CPU or RAM more than that judging from the file size that is provided and the goals for the project. I was having issues getting my code to store the downloaded data to the S3 bucket, because I was doing it though Jupyter and not Jupyter labs.
![Screenshot 2024-10-18 at 14-09-26 Notebook instances Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/8ba902f7-e32b-45a3-8fd0-f1059941d7c6)

S3 Bucket with the dataset
![Screenshot 2024-10-19 at 13-33-48 opmlimagebucket - S3 bucket S3 us-east-1](https://github.com/user-attachments/assets/5a37ff41-6f7d-4933-b6e7-8cbca4d3ce75)

For tuning I used ml.m5.2xlarge for the greater processing power so that the tuning job and training jobs could be completed more quickly and to avoid memory issues I had experienced when working with this dataset in a previous project. For training instances, I set *instance_count=4* and kept most parameters the same.

I had some problems with AWS, which required me to start over. This time I was able to load the data to S3 and launch the endpoints. The single and multi endpoints can be found below.

  Single instance trained endpoint: *pytorch-inference-2024-10-19-19-00-50-543*
  Multi instance trained endpoint: *pytorch-inference-2024-10-19-19-22-45-799*
![Screenshot 2024-10-19 at 15-28-40 Endpoints Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/d11aa842-0e8d-4891-861a-cd95bce2094f)
