# Operationalizing Machine Learning on SageMaker by Akram Al-Selwadi

## Setting up Notebook Instance in AWS SageMaker

I'm using the default SageMaker instance ml.t3.medium when Notebook Instances are created for my notebook as I will be only running the notebook only when working on the project, and I do not need a CPU or RAM more than that judging from the file size that is provided and the goals for the project. I was having issues getting my code to store the downloaded data to the S3 bucket, because I was doing it though Jupyter and not Jupyter labs.

![Screenshot 2024-10-18 at 14-09-26 Notebook instances Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/8ba902f7-e32b-45a3-8fd0-f1059941d7c6)

S3 Bucket with the dataset
![Screenshot 2024-10-19 at 13-33-48 opmlimagebucket - S3 bucket S3 us-east-1](https://github.com/user-attachments/assets/5a37ff41-6f7d-4933-b6e7-8cbca4d3ce75)

For tuning I used ml.m5.2xlarge for the greater processing power so that the tuning job and training jobs could be completed more quickly and to avoid memory issues I had experienced when working with this dataset in a previous project. For training instances, I set ``instance_count=4``and kept most parameters the same.

I had some problems with AWS, which required me to start over. This time I was able to load the data to S3 and launch the endpoints. The single and multi endpoints can be found below.

  Single instance trained endpoint: ``pytorch-inference-2024-10-19-19-00-50-543``
  
  Multi instance trained endpoint: ``pytorch-inference-2024-10-19-19-22-45-799``
![Screenshot 2024-10-19 at 15-28-40 Endpoints Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/d11aa842-0e8d-4891-861a-cd95bce2094f)

## EC2 Instance Training

I used Deep Learning OSS Nvidia Driver AMI GPU PyTorch 2.3.1 (Amazon Linux 2), since we are using Pytorch 2 and need Deep Learning with a ```t3.xlarge``` since we do not need too much computing power but enough to train our model. 
![Screenshot 2024-10-19 at 19-17-02 Instances EC2 us-east-1](https://github.com/user-attachments/assets/4f8d1bf7-3a50-4b67-b549-12aff9b985a2)
Now that we have our EC2 instance we connect to it. I tried to use my own IP address, but it was not connecting the EC2 instance so I after using a VPC and adjusting the port value it worked and was used for the project. 

We now begin by downloading the dataset to the EC2 by running the following command in the terminal:
```ruby
wget https://s3-us-west-1.amazonaws.com/udacity-aind/dog-project/dogImages.zipunzip dogImages.zip
```
Once we get the data, we are going to create a directory to save the trained models
```ruby
mkdir TrainedModels
```
Now, we need to create a Python file and paste the training code into it using ```vim``` to create an empty file to paste our ```ec2train1.py``` code:
```ruby
vim solution.py
```
Use the following command so that we paste our code into solution.py
```ruby
:set paste + Press Enter
```
Copy the code located in https://github.com/AWS_MLEud/OperationalMLOps/ec2train1.py and paste into solution.py
```ruby
:wq! + Press Enter
```
Now we can run our script to train the model
```ruby
python solution.py
```

I had issues with the CUDA version which refused to run my ```ec2train.py``` code. I tried to ```pip``` some frameworks, but after spending a few hours the soultion was to reinstall PyTorch with the correct CUDA Version by using the two following scripts.

To uninstall current version of PyTorch:

```pip uninstall torch torchvision torchaudio -y```

To reinstall PyTorch with the appropriate CUDA version:

```pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118```

The result of our instance has worked and the path identified:
![Screenshot 2024-10-19 at 22-53-08 EC2 Instance Connect](https://github.com/user-attachments/assets/ddfbd45c-2c50-4169-9e46-7811eca9befa)

The metrics of our EC2 Instance:
![Screenshot 2024-10-19 at 23-07-44 Instances EC2 us-east-1](https://github.com/user-attachments/assets/69a9c91c-8440-40dc-b87b-581f6cd02bf9)

## Advantages of EC2 vs Notebook Instances:

EC2 can be customized to meet specific requirements and computing resource needs such as, the number of CPUs used, memory size, GPU support, frameworks needed while optimized for high-performance computing. This can greatly reduce the time to train large machine learning models such a LLM's or BERT.

Notebook instances are easy to set up, as they come pre-configured with popular machine learning frameworks and libraries. They also enable seamless collaboration and integrate well with other AWS services like SageMaker, Lambda, and S3, while offering essential tools for machine learning engineering and operations.

## Lambda Functions

Since there was a single instance endpoint and multi-instance endpoint, two lambda functions were created ```oneinstance``` and ```multiinstance``` repectively. After we create the Lambda Function we can replace the default code with the code located in https://github.com/AWS_MLEud/OperationalMLOps/lamdafunction.py Since a Lambda function will invoke a SageMaker endpoint, we need to grant permission to the Lambda function to access SageMaker. We need to attach a new policy to our Lambda function so that it can access SageMaker by going through AWS IAM.

AWS IAM Homepage with roles:
![Screenshot 2024-10-20 at 00-42-37 Dashboard IAM Global](https://github.com/user-attachments/assets/790d7054-d97c-4601-9cbf-ac604ea10407)
![Screenshot 2024-10-20 at 00-31-03 Roles IAM Global](https://github.com/user-attachments/assets/83f16221-82fa-4dd4-8124-08e252dc2c80)

The SageMakerFullAccess policy was the simplest choice, but can pose security risks. It is advisable to carefully consider the level of access required for every specific use case, but it will be enough for this project.

Policy added for ```oneinstance``` Lambda Function:
![Screenshot 2024-10-20 at 00-30-48 oneinstance-role-tahuisce IAM Global](https://github.com/user-attachments/assets/fb0bc612-bf31-4336-b661-183f4fe50708)

Policy added for ```multiinstance``` Lambda Function:
![Screenshot 2024-10-20 at 00-31-13 multiinstance-role-gwjexovn IAM Global](https://github.com/user-attachments/assets/ecf52b2c-7bba-42f6-9e5b-f435972234bb)

In the concole window of the lambda function, the default code is replace with the code from ```https://github.com/AkramAls/AWS_MLEud/edit/main/OperationalMLOps/lambdafunction.py``` and deploy to save the code before proceeding with testing the lambda function.

Test code for the Lambda functions:
```
{
  "url": "https://s3.amazonaws.com/cdn-origin-etr.akc.org/wp-content/uploads/2017/11/20113314/Carolina-Dog-standing-outdoors.jpg"
}
```
The result of our succsessful testing from both Lambda functions:
```
{
  "statusCode": 200,
  "headers": {
    "Content-Type": "text/plain",
    "Access-Control-Allow-Origin": "*"
  },
  "type-result": "<class 'str'>",
  "COntent-Type-In": "<__main__.LambdaContext object at 0x7fdb48dee070>",
  "body": "[[-9.096986770629883, -6.280696392059326, -3.6407206058502197, -1.623978614807129, -4.52439022064209, -5.167107105255127, -2.9249727725982666, -2.8705129623413086, -7.8250532150268555, -1.5073524713516235, -1.95694100856781, -4.514562606811523, -0.32904502749443054, 0.6669425368309021, -5.567157745361328, -3.9589951038360596, -8.504964828491211, -2.159571886062622, -3.857482433319092, 1.6533637046813965, -6.385259628295898, -3.1520373821258545, -8.131010055541992, -7.660277843475342, -6.443761348724365, -10.485486030578613, -5.250019073486328, -6.405547618865967, -8.258563995361328, -0.5176408290863037, -6.320932388305664, -4.718957901000977, -8.405498504638672, -7.335566520690918, -10.348310470581055, -6.9180426597595215, -7.017837047576904, -0.6434236764907837, -2.468348264694214, -8.042801856994629, -3.815700054168701, -2.242116689682007, 2.4018771648406982, -6.806573867797852, -0.5093210339546204, -9.101749420166016, -5.03752326965332, 1.5559486150741577, -5.102183818817139, -2.6244912147521973, -3.957767963409424, -9.652307510375977, -9.861614227294922, -3.170854330062866, -7.720099449157715, -2.309325695037842, -6.728293418884277, -6.596529006958008, -2.1687183380126953, -2.528806447982788, -7.449897289276123, -9.91438102722168, -11.869610786437988, -10.103141784667969, -3.306910276412964, -9.012389183044434, 1.470926284790039, -6.440833568572998, -2.6904876232147217, -1.164985179901123, -0.08895723521709442, -8.30551528930664, -5.315221786499023, -4.130455493927002, -6.954455375671387, -2.6175105571746826, -12.292277336120605, -2.981276512145996, -7.856142520904541, -4.847909450531006, -0.07831322401762009, -8.176919937133789, -1.3720309734344482, -2.4301841259002686, -10.431918144226074, -7.048996448516846, -1.7571501731872559, -10.699981689453125, -4.15992546081543, -2.192228078842163, -11.052444458007812, -7.7356672286987305, -6.310140132904053, -8.986392974853516, -7.390171051025391, -2.5930705070495605, -4.329840183258057, -4.918014049530029, -9.226435661315918, -4.837480545043945, -8.597445487976074, -2.8956727981567383, -6.596631050109863, -3.817946434020996, -6.6799492835998535, -10.64243221282959, -1.2400000095367432, 0.7482686638832092, -3.1552622318267822, 1.8327558040618896, -0.9535248279571533, -2.2177162170410156, -11.003973007202148, -7.895530700683594, -8.497802734375, -2.8575565814971924, -8.964386940002441, -0.7265918850898743, -8.964128494262695, -0.8830686211585999, -4.121635437011719, -5.214657783508301, -4.111880779266357, -7.8513360023498535, -8.24428653717041, -7.469294548034668, -3.6901681423187256, -1.780672311782837, -5.731601715087891, -9.206201553344727, -6.329228401184082, -2.2607877254486084, -4.914355754852295]]"
}

```
This image shows a successful test using the Lambda function ```multiinstance```:
![Screenshot 2024-10-20 at 00-53-36 Configure provisioned concurrency Version 1 multiinstance Functions Lambda](https://github.com/user-attachments/assets/5f8dac0b-17bd-4df2-92a6-9760f6ef58f2)

This image shows a successful test using the Lambda function ```oneinstance```:
![Screenshot 2024-10-20 at 00-54-25 Edit concurrency oneinstance Functions Lambda](https://github.com/user-attachments/assets/0bad26dd-5a3f-4f20-8ab5-ee901d726883)


## Scaling Lambda Function and Endpoint

By default, a Lambda function can handle only one request at a time.  Using concurrency to enable the process for multiple requests simultaneously. Before setting up concurrency, it is nessacry to create a version in the Configuration tab. Since this project is not sure of how many user will use the model, using provisioned concurrency will be a good choice. Provisioned concurrency initializes a specified number of execution environments, enabling them to respond immediately. Therefore, a higher concurrency level results in reduced latency. Since there was an opportunity, the concurrency was set to 10 so the Lambda function can handle 10 requests at once.

![Screenshot 2024-10-20 at 00-53-36 Configure provisioned concurrency Version 1 multiinstance Functions Lambda](https://github.com/user-attachments/assets/19a9cc13-b08b-4611-bc64-a6c7dfa403c4)

Then go to the deployed endpoint in SageMaker to select the AllTraffic under the ``Endpoint runtime settings`` tab and click ``Configure auto scaling``
![Screenshot 2024-10-20 at 00-57-44 pytorch-inference-2024-10-19-19-22-45-799 Endpoints Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/151daeae-0019-4398-892b-33e1758be9c9)

The value of 10 was entered in the ``Target value`` which means when our endpoint receives 10 requests simultaneously, auto-scaling will be triggered. The 'Scale In' and 'Scale Out' parameters were both set to 180 seconds, which is the amount of time auto-scaling will wait before increasing or decreasing the number of instances.
![Screenshot 2024-10-20 at 01-03-16 AllTraffic pytorch-inference-2024-10-19-19-22-45-799 Endpoints Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/77986581-4965-4322-90ac-72e72358b382)

This image shows the successful enabling of auto-scaling to the endpoint in SageMaker:
![Screenshot 2024-10-20 at 01-04-11 undefined Endpoints Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/5c0e96fe-ac84-4b04-b4a9-a410899d01f2)

This image shows the details of the auto scaling showing the endpoint scale out and in between 1 and 3 instances when auto-scaling is triggered.
![Screenshot 2024-10-20 at 01-05-05 undefined Endpoints Amazon SageMaker us-east-1](https://github.com/user-attachments/assets/29583e22-7d5e-4b95-82b0-cb7512b35000)


