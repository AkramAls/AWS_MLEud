# Operationalizing Machine Learning on SageMaker by Akram Al-Selwadi

## Initial Setup

I'm using the default SageMaker instance ml.t3.medium when Notebook Instances are created for my notebook as I will be only running the notebook only when working on the project, and I do not need a CPU or RAM more than that judging from the file size that is provided and the goals for the project. I was having issues getting my code to store the downloaded data to the S3 bucket, because I was doing it though Jupyter and not Jupyter labs.
![Screenshot 2024-10-18 at 14-09-26 Notebook instances Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/8ba902f7-e32b-45a3-8fd0-f1059941d7c6)

S3 Bucket with the dataset
![Screenshot 2024-10-19 at 13-33-48 opmlimagebucket - S3 bucket S3 us-east-1](https://github.com/user-attachments/assets/5a37ff41-6f7d-4933-b6e7-8cbca4d3ce75)
