<p align="center">
  <img src="https://i.imgur.com/E9GgcaS.png" 
</p>
  
## ☁️  Develop a text narrator using Amazon Polly ☁️

In this project, I  developed a text narrator using Amazon Polly. A piece of text (book, article, newsletter) will be uploaded in an Amazon S3 bucket and converted to speech. The voice, pitch and speed parmeters can be adjusted. We will access the Amazon Polly service and store the audio output in a S3 bucket using a Lambda function.


<h2>Environments and Technologies Used</h2>

  - Amazon Web Services (AWS)
  - AWS Console 
  - S3
  - Amazon Polly
  - AWS Lambda
  - IAM (for managing user permissions)

  
<h2>Amazon Polly Overview</h2>  
Amazon Polly transforms written text into natural-sounding speech, making it feel like you're listening to a real person rather than a robot. You can tweak the speed, pitch, and tone to match your needs. Choose from a variety of voices, accents, and languages – whether you prefer a British or American accent, male or female voice, Polly has you covered. You can change toning, timing and pacing of speech.

![image](/assets/image1.png)
![image](/assets/image2.png)

<h2>How to Build</h2>

1. **Create a S3 Bucket and upload files**  

In this step we will create an S3 bucket using the AWS CLI. In this instance, we will use the cloudshell terminal. the commnand utilized for this part is `aws s3api create-bucket --bucket bucketname`. Make sure that 'bucketname' on the command is replaced and that the bucket name is unique. 

![image](/assets/image3.png)

The audio files generated from Amazon Polly will be stored in this bucket with the help of AWS Lambda.

2. **Create Lambda Function**  

In this step, we will create the lambda function for the translator both ways: AWS CLI and AWS Console, to showcase both use cases. We will create the function with name `TTSTranslatorfunction` and `TTSTranslatorfunction` along with `Node.js 16.x` as the runtime environment, utilizing existing IAM roles. You can create roles on the AWS console easily.

![image](/assets/image4.png)

for the CLI, we will utilize the cloudshell. In the CLI example, we will create an placeholder/empty .zip file in order to create a function. Code can be modified later without issue. 

`aws s3 cp empty.zip s3://amazonpollynilso/` 

![image](/assets/image5.png)

Then, we proceed with creating the lambda function. With this command, we will define the role, name of the function and runtime, along with pointing the dummy file for later use.

`aws lambda create-function \
    --function-name TTSTranslatorfunction \
    --runtime nodejs16.x \
    --role arn:aws:iam::137068224350:role/LambdaFunction \
    --handler index.handler \
    --code S3Bucket=amazonpollynilso,S3Key=empty.zip`

![image](/assets/image6.png)

Alternatively, we will create the same type of function but this time in the console.

![image](/assets/image7.png)

Both code sources will be able to be edited without problems.

![image](/assets/image8.png)
![image](/assets/image9.png)

We will then rename the file to index.js.

---

3. **Importing Libraries from Python**
 


5. **Define Functions**



6. **Final code with results**


**(Optional) Side Project**


    
 ---

<h2>Conclusion</h2>
This project made use of an image label generator by calling boto3 API into images uploaded with a S3 bucket combined with Amazon Rekognition. With this feature, you can identify individual parts of an object along with it's closest (defined by confident percentage) label definition. Alternatively, this tool can be used for Labels on Video Streams (Amazon Kinesis) or generated frame by frame with a gif file. 
☁️
