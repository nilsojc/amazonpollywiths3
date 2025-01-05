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
  - Node.js

  
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

Next, we will write the code necessary for lambda to execute its function in the `index.js` file.

We start by connecting AWS SDK with Amazon Polly (Converting Text-to-speech) and S3 (for storing files)

`const AWS = require('aws-sdk');
const polly = new AWS.Polly();
const s3 = new AWS.S3();`

Then, We will be writing a function that AWS will run whenever an action happens. 

`exports.handler = async (event) => {`

We will also assign a function to the text so that it converts into speech along with the voice recipient. We will decide how the speech will result in along with the format.

`const text = event.text;`

`const params = {`

   `Text: text,
    OutputFormat: 'mp3',
    VoiceId: 'Joanna' // You can change this to the desired voice
};`

We send the text to Polly and ask it to turn it into speech. Polly does its magic and gives us back the speech as data.

`const data = await polly.synthesizeSpeech(params).promise();`

`const key = audio-${Date.now()}.mp3;`

`const s3Params = {`
    `Bucket: 'your-bucket-name', // Replace with your S3 bucket name
    Key: key,
    Body: data.AudioStream,
    ContentType: 'audio/mpeg'`
`};`

`await s3.upload(s3Params).promise();`

Finally, we create an output message saying that the speech has been saved successfully with it's name in our S3 bucket. If something is not right, it will display an error message. 

`const outputMessage = The audio file has been successfully stored in the S3 bucket by the name ${key}`;

`return {
    statusCode: 200,
    body: JSON.stringify({ message: outputMessage })`
`};`

`} catch (error) {
    console.error('Error:', error);
    return {
        statusCode: 500,
        body: JSON.stringify({ message: 'Internal server error' })
    };`
`}`

The complete function code will look like this:

![image](/assets/image10.png)


3. **Test the Result**


Once finished with the code configuration for the lambda function, it's time to test the function by creating a test event.

![image](/assets/image11.png)

We will click on 'test' and configure an event for the function, as well as provide a name and text of your choosing in the event JSON.

![image](/assets/image12.png)

After saving the JSON file and testing the function, the code we created will run.

![image](/assets/image13.png)

Finally, the audio file will be output to our S3 bucket!

![image](/assets/image14.png)

Here is the sample file that was created from the project:

https://github.com/user-attachments/assets/d267821a-9fb4-4ad6-9e7b-0c29577439de


 ---

<h2>Conclusion</h2>
This project demonstrates the power of AWS Polly in creating lifelike text-to-speech experiences. By leveraging Amazon S3 for storage and AWS Lambda for automation, it ensures a seamless and scalable solution. Whether you're converting books, articles, or newsletters, this tool can bring your text to life.

