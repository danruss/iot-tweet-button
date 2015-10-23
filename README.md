# iot-tweet-button

The iot-tweet-button gives you the ability to press your AWS IoT Button and send a tweet to your Twitter account with some button metadata (battery life, click type, and time stamp).  You can see my IoT button in action at [@iotweetbutton](https://twitter.com/IoTweetButton).

##### What exactly is the AWS IoT button?
The AWS IoT button is a developer button (modeled after the Amazon Dash buttons) that allows you to connect to your home or office WiFi network and send messages that can trigger actions in the AWS IoT platform.

##### What is the AWS IoT Platform?
The AWS IoT platform provides developers with a way to communicate with 'things' such as the AWS IoT button.

##### What is AWS Lambda?
AWS Lambda gives you the ability to execute Java, NodeJS or Python code in a short-lived container triggered by a number of events such as an HTTP request, SMS message (via SNS), IoT action, etc. without having to fuss with servers.  Lambda paves the way for serverless architecture in the AWS ecosystem.

##### To get started, you'll need:
  - A Twitter account with API access (note: this requires verifying your account with a cell phone)
  - An AWS IoT Button (These were given out at AWS Re:Invent 2015)
  - An AWS account (The free tier will cover the IoT platform and Lambda requests)

### Prep
Before entering the AWS console, you'll need to package the Node application and dependencies so it can be uploaded to AWS Lambda.
1. Modify the .env file with the appropriate values for your Twitter API account
```
TWITTER_CONSUMER_KEY=[YOUR API KEY HERE]
TWITTER_CONSUMER_SECRET=[YOUR API SECRET HERE]
TWITTER_ACCESS_TOKEN_KEY=[YOUR ACCESS TOKEN HERE]
TWITTER_ACCESS_TOKEN_SECRET=[YOUR TOKEN SECRET HERE]
```
2. Install the node dependencies using npm in the root of the folder you downloaded these files to
```
npm install
```
3. Zip the files recursively to bring along the node_modules folder (OS X syntax)
```
zip -r iotweet.zip .
```

### Setup
1. Setup your AWS Free Tier account (if you don't already have one).
2. Activate your AWS IoT Button on your AWS account.  
  * This will create a 'thing', policy and certificate on the IoT platform.
3. In the AWS IoT platform, create a rule.  I named mine 'Button Press'.
  * For the rule, you want SELECT * FROM 'iotbutton/+'
  * In other words: Attribute = *, Topic Filter = iobutton/+
  * This rule will fire every time a 'thing' with a name that begins with 'iotbutton' sends a message
  * Click add an action and choose 'Insert this message into a code function and execute it'.
  * Click 'Create a New Resource'  (this will take you to the AWS Lambda console.)
4. In AWS Lambda, 
  * Click 'Skip' on the select blueprint page
  * Enter a name and description for the function
  * Choose a Runtime of Node.js
  * Code entry type: Upload a ZIP file and select the iotweet.zip file created above
  * Handler = index.handler
  * Role = basic execution role
  * Memory = 128 MB
  * Timeout = 30 sec
5. Test the AWS Lambda function using the following message:
```javascript
{"serialNumber": "NOTREALLYASERIALNUMBER", "batteryVoltage": "1778mV", "clickType": "SINGLE"}
```
6. If everything worked, you should see a success message in the Lambda console and a new tweet on your timeline!

### Version
0.0.1

### Resources
* [AWS IoT Button](https://aws.amazon.com/iot/button/)
* [AWS Lambda Documentation](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
* [AWS Free Tier/Signup](https://aws.amazon.com/free/)

### Tech
The iot-tweet-button uses two open source projects to work properly:

* [Node Twitter](https://www.npmjs.com/package/twitter) - For tweeting
* [Dotenv](https://www.npmjs.com/package/dotenv) - Allows the use of environment variables in AWS Lambda

### Special Thanks
Special thanks to Jinesh Varia for providing me with the AWS IoT button after I missed the initial handout at AWS Re:Invent.




