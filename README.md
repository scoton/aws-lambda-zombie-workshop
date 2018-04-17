# Zombie Microservices Workshop: Lab Guide

## Overview of Workshop Labs
The [Zombie Microservices Workshop](http://aws.amazon.com/events/zombie-microservices-roadshow/) introduces the basics of building serverless applications using [AWS Lambda](https://aws.amazon.com/lambda/), [Amazon API Gateway](https://aws.amazon.com/api-gateway/), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), [Amazon Cognito](https://aws.amazon.com/cognito/), [Amazon SNS](https://aws.amazon.com/sns/), and other AWS services. In this workshop, as a new member of the AWS Lambda Signal Corps, you are tasked with completing the development of a serverless survivor communications system during the Zombie Apocalypse.

This workshop has a baseline survivor chat app that is launched via [CloudFormation](https://aws.amazon.com/cloudformation/). Complete the lab exercises to extend the functionality of the communications system or add your own custom functionality!

Prior to beginning the labs, you will need to finalize the setup of User authentication for the application with [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html). This is a necessary step to finalize the readiness of the application.

### Required: Setup Authentication with Cognito User Pools
In this setup lab, you will integrate user authentication into your serverless survivor chat application using Amazon Cognito User Pools.

### Labs
Each of the labs in this workshop is an independent section and you may choose to do some or all of them, or in any order that you prefer.

* **Lab 1: Typing Indicator**  

  This exercise already has the UI and backend implemented, and focuses on how to setup the API Gateway to provide a RESTful endpoint. You will configure the survivor chat application to display which survivors are currently typing in the chat room.

* **Lab 2: SMS Integration with Twilio**  

    This exercise uses [Twilio](http://twilio.com) to integrate SMS text functionality with the survivor chat application. You will configure a free-trial Twilio phone number so that users can send text messages to the survivor chat application. You'll learn to leverage mapping templates in API Gateway to perform data transformations in an API.

* **Lab 3: Search Integration with Elasticsearch**  

    This exercise adds an Elasticsearch cluster to the application which is used to index chat messages streamed from the DynamoDB table containing chat messages.

* **Lab 4: Slack Integration**  

    This exercise integrates the popular messaging app, [Slack](http://slack.com), into the chat application so that survivors can send messages to the survivor chat from within the Slack app.

* **Lab 5: Intel Edison Zombie Motion Sensor** (IoT device required)

    This exercise integrates motion sensor detection of zombies to the chat system using an Intel Edison board and a Grove PIR Motion Sensor. You will configure a Lambda function to consume motion detection events and push them into the survivor chat!

### Workshop Cleanup

This section provides instructions to tear down your environment when you're done working on the labs.

* * *

### Let's Begin! Launch the CloudFormation Stack
*Prior to launching a stack, be aware that a few of the resources launched need to be manually deleted when the workshop is over. When finished working, please review the "Workshop Cleanup" section to learn what manual teardown is required by you.*

1\. To begin this workshop, **click one of the 'Deploy to AWS' buttons below for the region you'd like to use**. This is the AWS region where you will launch resources for the duration of this workshop. This will open the CloudFormation template in the AWS Management Console for the region you select.

Region | Launch Template
------------ | -------------
**N. Virginia** (us-east-1) | [![Launch Zombie Workshop Stack into Virginia with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=zombiestack&templateURL=https://s3.amazonaws.com/aws-zombie-workshop-us-east-1/CreateZombieWorkshop.json)
**Ohio** (us-east-2) | [![Launch Zombie Workshop Stack into Ohio with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=zombiestack&templateURL=https://s3-us-east-2.amazonaws.com/aws-zombie-workshop-us-east-2/CreateZombieWorkshop.json)
**Oregon** (us-west-2) | [![Launch Zombie Workshop Stack into Oregon with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=zombiestack&templateURL=https://s3-us-west-2.amazonaws.com/aws-zombie-workshop-us-west-2/CreateZombieWorkshop.json)
**Ireland** (eu-west-1) | [![Launch Zombie Workshop Stack into Ireland with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=zombiestack&templateURL=https://s3-eu-west-1.amazonaws.com/aws-zombie-workshop-eu-west-1/CreateZombieWorkshop.json)
**Frankfurt** (eu-central-1) | [![Launch Zombie Workshop Stack into Frankfurt with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=zombiestack&templateURL=https://s3-eu-central-1.amazonaws.com/aws-zombie-workshop-eu-central-1/CreateZombieWorkshop.json)
**Tokyo** (ap-northeast-1) | [![Launch Zombie Workshop Stack into Tokyo with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/new?stackName=zombiestack&templateURL=https://s3-ap-northeast-1.amazonaws.com/aws-zombie-workshop-ap-northeast-1/CreateZombieWorkshop.json)
**Seoul** (ap-northeast-2) | [![Launch Zombie Workshop Stack into Seoul with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-northeast-2#/stacks/new?stackName=zombiestack&templateURL=https://s3-ap-northeast-2.amazonaws.com/aws-zombie-workshop-ap-northeast-2/CreateZombieWorkshop.json)
**Singapore** (ap-southeast-1) | [![Launch Zombie Workshop Stack into Singapore with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/new?stackName=zombiestack&templateURL=https://s3-ap-southeast-1.amazonaws.com/aws-zombie-workshop-ap-southeast-1/CreateZombieWorkshop.json)
**Sydney** (ap-southeast-2) | [![Launch Zombie Workshop Stack into Sydney with CloudFormation](/Images/deploy-to-aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=zombiestack&templateURL=https://s3-ap-southeast-2.amazonaws.com/aws-zombie-workshop-ap-southeast-2/CreateZombieWorkshop.json)

*If you have CloudFormation launch FAILED issues, please try launching in us-east-1 (Virginia)*

2\. Once you have chosen a region and are inside the AWS CloudFormation Console, you should be on a screen titled "Select Template". We are providing CloudFormation with a template on your behalf, so click the blue **Next** button to proceed.

3\. On the following screen, "Specify Details", your Stack is pre-populated with the name "zombiestack". You can customize that to a name of your choice **less than 15 characters in length** or leave as is. For the parameters section, if you want to develop with a team and would like to create IAM Users in your account to grant your teammates access, then specify how many teammates/users you want to be created in the **NumberOfTeammates** text box. Otherwise, leave it defaulted to 0 and no additional users will be created. The user launching the stack (you) already have the necessary permissions. Click **Next**.

*If you create IAM users, an IAM group will also be created and those users will be added to that group. On deletion of the stack, those resources will be deleted for you.*

4\. On the "Options" page, leave the defaults and click **Next**.

5\. On the "Review" page, verify your selections, then scroll to the bottom and select the checkbox **I acknowledge that AWS CloudFormation might create IAM resources**. Then click **Create** to launch your stack.

6\. Your stack will take about 3 minutes to launch and you can track its progress in the "Events" tab. When it is done creating, the status will change to "CREATE_COMPLETE".

7\. Click the "Outputs" tab in CloudFormation and click the link for "MyChatRoomURL". This should open your chat application in a new tab. Leave this tab open as you'll come back to it later.

Please continue to the next section for the required Cognito User Pools authentication setup.

## Setup Authentication with Cognito User Pools (Required)

The survivor chat uses [Amazon Cognito](https://aws.amazon.com/cognito/) for authentication. Cognito Federated Identity enables you to authenticate users through an external identity provider and provides temporary security credentials to access your app’s backend resources in AWS or any service behind Amazon API Gateway. Amazon Cognito works with external identity providers that support SAML or OpenID Connect, social identity providers (such as Facebook, Twitter, Amazon) and you can also integrate your own identity provider.

In addition to federating 3rd party providers such as Facebook, Google, and other providers, Cognito also offers a new built-in Identity Provider called [Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html).

A Cognito Federated Identity pool has already been created for you when you launched CloudFormation.

You will now setup the Cognito User Pool as the user directory of your chat app survivors and configure it as a valid Authentication Provider with the Cognito Federated Identity Pool. API Gateway has been configured with IAM Authorization to only allow requests that are signed with valid AWS permissions. When a user signs into the Survivor Chat App (User Pool) successfully, a web call is made to the Cognito Federated Identity Pool to assume temporary AWS credentials for your authenticated user. These credentials are used to make signed [AWS SigV4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) HTTPS requests to your message API.

![Overview of Cognito Authentication](/Images/CognitoOverview.png)

**Let's get started...**

1\. Navigate to the Cognito service console.

![Navigate to the Cognito service](/Images/Cognito-Step1.png)

Cognito User Pools is not available in all AWS regions. Please review the list [here](https://docs.aws.amazon.com/general/latest/gr/rande.html#cognito_identity_region) for the regions that Cognito is available in. Therefore if you launched your CloudFormation stack in any region other than one of those listed on the above website, then please use the top navigation bar in the management console to switch AWS regions and navigate to **us-east-1 (Virginia)** to configure Cognito. Your application will stay hosted in the region you launched the CloudFormation template, but the authentication with Cognito will reside in us-east-1 (Virginia). If you launched the Cloudformation stack in one of those regions where Cognito exists, then please simply navigate to the Cognito service in the AWS Management Console as the service is available in that region already and you will configure it within that region.

When inside the Cognito service console, click the blue button **Manage your User Pools**. You will setup the user directory that your chat application users will authenticate to when they use your app.

2\. Click the blue button **Create a User Pool** in the upper right corner. You'll create a new user directory.

3\. In the "Pool Name" text box, name your user pool **[Your CloudFormation stack name]-userpool**. For example, if you named your CloudFormation stack "sample" earlier, then your user pool name would be "sample-userpool". After naming your User Pool, click **Step through Settings** to continue with manual setup.

4\. On the attributes page, select the "Required" checkbox for the following attributes: **email, name, phone number**.

* Cognito User Pools allows you to define attributes that you'd like to associate with users of your application. These represent values that your users will provide when they sign up for your app. They are available to your application as a part of the session data provided to your client apps when users authenticate with Cognito.

5\. Click the link "Add custom attribute". Leave all the defaults and type a "Name" of **slackuser** exactly as typed here. Add 2 additional custom attributes: **slackteamdomain** and **camp**.

* Within a User Pool, you can specify custom attributes which you define when you create the User Pool. For the Zombie Survivor chat application, we will include 3 custom attributes.

Your attributes configuration should match the image below:

![Cognito User Pools: Attributes Configuration](/Images/Cognito-Step5.png)

Click **Next Step**.

6\. On the Policies page, leave the Password policy settings as default and click **Next step**.

7\. On the verifications page, leave the defaults and click **Next step**.

* We will not require MFA for this application. However, for during sign up we are requiring verification via email address. This is denoted with the email checkbox selected for "Do you want to require verification of emails or phone numbers?". With this setting, when users sign up for the application, a confirmation code will be sent to their email which they'll be required to input into the application for confirmation.

8\. On the "Message Customizations" page, in the section titled **Do you want to customize your email verification message?** add a custom email subject such as "Signal Corps Survivor Confirmation". We won't modify the message body but you could add your own custom message in there. We'll let Cognito send the emails from the service email address, but in production you could configure Cognito to send these verifications from an email server you own. Leave the rest of the default settings and click **Next step**.

On the Tags page, leave the defaults and click **Next step**. Next, on the Devices page, leave the default option of "No" selected. We will not configure the User Pool to remember user's devices.

9\. On the Apps page, click **Add an app**. In the **App Name** textbox, type "Zombie Survivor Chat App" and **deselect the client secret checkbox**. Click **Set attribute read and write permissions**. You need to make sure that the app has "writable" and "readable" access to the attributes you created. Make sure that **all of the checkboxes are selected** for "Readable Attributes" and "Writable Attributes". Then click **Create app**, and then click **Next step**.

10\. In the dropdowns for the **Pre authentication** and **Post confirmation** triggers, select the Lambda function named "[Your CloudFormation Stack name]-CognitoLambdaTrigger-[Your Region]". Click **Next step**.

* Cognito User Pools allows developers to inject custom workflow logic into the signup and signin process. This custom workflow logic is represented with AWS Lambda functions known as Lambda Triggers.

* With this feature, developers can pass information to a Lambda function and specify that function to invoke at different stages of the signup/signin process, allowing for a serverless and event driven authentication process.

* In this application, we will create two (2) Lambda triggers:

    * Post-Confirmation: This trigger will invoke after a user successfully submits their verification code upon signup and becomes a confirmed user. The Lambda function associated with this trigger takes the attributes provided by the user and inserts them into a custom Users table in DynamoDB that was created with CloudFormation. This allows us to perform querying of user attributes within our application.

    * Pre-Authentication: This trigger will invoke when a user's information is submitted for authentication to Cognito each time the survivor signs into the web application. The code for with this Lambda Trigger takes the user's attributes have been passed in as parameters from the invoking User Pool and using them to perform an update on the User's record in DynamoDB Users table. This allows us to load the user's data into DynamoDB when they initially sign in and also keep it current with the values in User Pools in an on-going basis as they log in each time.

    * For this workshop we use the same backend Lambda function for both of the triggers. On invocation, the function checks what type of even has occurred, Post-Confirmation or Pre-Authentication, and executes the correct code accordingly.

11\. Review the settings for your User Pool and click **Create pool**. If your pool created successfully you should be returned to the Pool Details page and it will display a green box that says "Your user pool was created successfully".

12\. Open a text editor on your computer and copy into it the "Pool Id" displayed on the Pool details page. Then click into the **Apps** tab found on the left side navigation pane. You should see an **App client id** displayed. Copy that **App client id** into your text editor as well.

You are done configuring the User Pool. You will now setup federation into the Cognito Identity Pool that has already been created for you.

* An [Amazon Cognito Identity Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html) has been configured for you. Identity Pools allow external federated users to assume temporary credentials from AWS to make service API calls from within your apps.

* You've just created the User Pool for authentication into your app. Now your users still need access to make IAM Authorized AWS API calls.

* You'll setup federation inside of Cognito Identity and allow your User Pool as an **Authentication Provider**.

On the top navigation bar in the management console, switch to **Federated Identities** by clicking the link as shown below.

![Navigating to Federated Identities Console](/Images/Cognito-Step12.png)

13\. Click into the Identity Pool that has already been created for you. It should be named "[Your CloudFormation stack name] _ identitypool". On the Idenity pool dashboard, in the upper right, select **Edit identity pool**.

14\. Cognito Identity allows you to give access to both authenticated users and unauthenticated (guest) users. The permissions associated with these groups of users is dictated by the IAM role that you attach to these Cognito roles. Your Authenticated and Unauthenticated Cognito roles have already been configured for you in CloudFormation. The Authenticated role has been configured to give permissions to the principal (your Cognito authenticated application user) to make "execute-api:invoke" calls to the API Gateway endpoint ARNs associated with the survivor serverless app.

* When users authenticate into the application, they become an authenticated user, and the application allows them to send chat messages to the survivor chat.

15\. Click the black dropdown arrow in the section titled "Authentication providers". You will configure your Identity pool to allow federated access from your Identity Provider, your Cognito User Pool. In the "Cognito" identity provider tab, insert your **User Pool ID** and **App Client ID** into their respective text boxes from your text editor file. Do not delete them from the text file, you'll need these items again in a later step.

You should have copied these from your User Pool earlier when you set it up. If you do not have these copied, please navigate back to your Cognito User Pool you created earlier and locate your User Pool Id and App Client ID.

Scroll to the bottom of the page and click **Save Changes** to save the User Pool configuration settings. Your Cognito Federated Identiy Pool has been configured with Congito User Pool as an IdP. When users authenticate to the User Pool, they will assume temporary credentials with the permissions allowed via the Authenticated Role.

16\. You will now make an update to an application config file so that the serverless Javascript application can communicate with your User Pool to log users in.

Navigate to the Amazon S3 console **in the region where you launched your CloudFormation stack.**

* If you changed regions to configure Cognito, please return back to the region where you launched the stack and navigate to the S3 service.

![Navigate to the S3 service](/Images/Cognito-Step16.png)

17\. On the Amazon S3 buckets listing page, find and click into the bucket that was created for you by CloudFormation. It should be named with your stack name prepended to the beginning. Something like [CloudFormation Stack Name]-s3bucketforwebsitecontent"....

* In the S3 Console search bar you can type **s3bucketforwebsitecontent** and your S3 bucket will display.

18\. This bucket contains all the contents for hosting your serverless JS app as well as the source code for the workshop's Lambda functions and CloudFormation resources. Please do not delete these contents. Click into the folder (prefix) named **S3** and navigate through to the file **S3/assets/js/constants.js**

Download the **S3/assets/js.constants.js** file to your local machine and open it with a text editor.

![Download the constants.js file](/Images/Cognito-Step18.png)

19\. Open up the constants.js file and copy over the User Pool ID into the "USER_POOL_ID" variable. Then copy the App Client ID into the "CLIENT_ID" variable. These should be copied from the open text file you had open from earlier.

* Your serverless javascript zombie application requires this constants values in file communicate with the different services of the workshop.

* The Identity Pool Id was automatically filled in with several other variables when the CloudFormation template was launched.

20\. Save the constants.js file and upload it back to S3. While in the S3 console window, make sure you are in the **js** directory. Click the blue **Upload** button and upload the constants.js file from your local machine. Within the upload dialog, select the "Manage public permissions" dropdown and set the permissions on the file to read-only for the public by selecting the **Read** checkbox next to Everyone under the Objects category. You can also drag your file from your local machine into the S3 browser console to initiate an upload and then when the object is uploaded, make sure to select **Make Public**.

* Your application now has the configuration it needs to interact with Cognito.

21\. Navigate back to CloudFormation and find the Chat Room URL (MyChatRoomURL) in the Outputs tab of your CloudFormation stack. Click it to open the chat application in a new browser window.

* If you already had the application opened in your browser, please refresh the page so that the new constants.js loads with the app.

22\. You should see a sign in page for the Zombie survivor web app. You need to create an account so click **Sign Up**.

23\. Fill out the form to sign up as a survivor.

* **Select your Camp**: Specify the geography where you live! Currently this attribute is not used in the application and is available for those that want to tackle an extra credit opportunity!. When you're done with the workshop, try and tackle the Channel Challenge in the Appendix.

* **Slack Username**: Type the Slack Username you will use during the Slack lab of this workshop. This associates your Slack username with your Survivor app user account and is required if you want to do the Slack lab.

* **Slack Team Domain Name**: Slack users can be members of many teams. Type the Slack team domain name that you want to integrate with this survivor chat app. The combination of a Slack team domain and Slack Username will unique identity a user to associate with your new Survivor chat app account.

When done, click **Sign Up**.

24\. A form should appear asking you to type in your confirmation code. Please check your inbox for the email address you signed up with. You should received an email with the subject "Signal Corps Survivor Confirmation" (May be in your Spam folder!). Copy over the verification code and enter into the confirmation window.

**Troubleshooting tips:**

* If you are getting errors during the signup, please revisit the settings for your Cognito User Pool. You need to make sure that you've done the following -  

    * Configured your Cognito Lambda triggers for both the **Pre-Authentication** and **Post-Confirmation** steps as described in Step 10.

    * Properly modified the constants.js config file and re-uploaded it to the JS directory for your application in S3. After you uploaded this constants.js file, you should have refreshed your zombie chat browser application page so that it could pull down the latest JS files. The application is client-side and needs this file's properties in order to bootstrap itself.

* Users created in the application are also stored in a DynamoDB table named "Users". If you did not have your Cognito triggers set up correctly, you will need to navigate to the DynamoDB Users table and delete the entry for your user. You can then re-register in the application again.


![Confirm your signup](/Images/Cognito-Step24.png)

After confirming your account, sign in with your credentials and begin chatting! You should see a red button called **Start Chatting** - click that button to toggle your session on. You may then begin typing messages followed by the "Enter" key to submit them.

25\. Your messages should begin showing up in the central chat pane window. Feel free to share the URL with your teammates, have them signup for accounts and begin chatting as a group! If you are building this solution solo, you can create multiple user accounts with different email addresses. Then login to both user accounts in different browsers to simulate multiple users.

**The baseline chat application is now configured and working! There is still important functionality missing and the Lambda Signal Corps needs you to build it out...so get started below!**

*This workshop does not utilize API Gateway Cache. Please KEEP THIS FEATURE TURNED OFF as it is not covered under the AWS Free Tier and will incur additional charges. It is not a requirement for the workshop.*

## Lab 1 - Typing Indicator

**What you'll do in this lab...**

In this section you will create functionality in your chat application that allows survivors to see which survivors in the chat room are currently typing. To configure this functionality, you will modify your API to integrate with backend Lambda functions that are responsible for querying and returning the users currently typing in the system, as well as updating metadata in the "talkers" DynamoDB table that contains details about which survivors are typing at which times. The survivor chat app continuously polls this API endpoint with GET requests to determine who is typing. As survivors are typing, POST requests are made to this same endpoint to update the talkers DynamoDB table.

The typing indicator shows up in the web chat client in a section below the chat message panel. The UI and backend Lambda functions have already been implemented, and this lab focuses on how to configure your new integration in API Gateway.

The application uses [CORS](http://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html). This lab will both wire up the backend Lambda function as well as perform the necessary steps to enable CORS.

**Typing Indicator Architecture**
![Overview of Typing Indicator Architecture](/Images/TypingIndicatorOverview.png)

1\. Navigate to the API Gateway service. You can search for it on the main console homepage or type in the service name to quickly access the service (as shown below)
![API Gateway in Management Console](/Images/Typing-Step1.png)

2\. On the APIs listing screen in API Gateway, click into your Zombie chat API. It should be prefixed with the name of your CloudFormation stack that launched it. By default this should be "zombiestack-". Select the Zombie Workshop API Gateway.

3\. Click the **GET** method of your /zombie/talkers resource located at **/zombie/talkers/GET**. You can do this by clicking the "GET" method under the /zombie/talkers resource. The GET method is highlighted in blue in the image below. Click there.
![GET Method](/Images/Typing-Step3.png)

*This GET HTTP method is used by the survivor chat app to perform continuous queries on the DynamoDB talkers table to determine which users are typing.*

4\. Click the **Integration Request** box.

5\. Under "Integration Type", Select **Lambda Function.**

* Currently, this API method is configured to a "MOCK" integration. MOCK integrations are dummy backends that are useful when you are testing and don't yet have the backend built out but need the API to return sample dummy data. You will remove the MOCK integration and configure this GET method to connect to a Lambda function that queries DynamoDB.

6\. For the **Lambda Region** field, select the region in which you launched the CloudFormation stack. (HINT: Select the region code that corresponds with the yellow CloudFormation button you clicked to launch the CloudFormation template. You can also look in the upper right corner of the Management Console to see which region you are in). For example if you launched your stack in Virginia (us-east-1), then you will select us-east-1 as your Lambda Region.

* When you launched the CloudFormation template, the launch also created several Lambda functions for you locally in the region where you launched your CFN stack - this includes functions for retrieving data from and putting data into a DynamoDB "Talkers" table with details about which survivors are currently typing in the chat room.

7\. For the Lambda Function field, begin typing "gettalkers" in the text box. In the auto-fill dropdown, select the function that contains "GetTalkersFromDynamoDB" in the name. It should look something like this.... **[CloudformationTemplateName]-[XXXXXXX]-GetTalkersFromDynamoDB-[Your Region]**.

* This Lambda function is written in NodeJs. It performs GetItem DynamoDB requests on a Table called Talkers. This talkers table contains records that are continuously updated whenever users type in the chat room. By hooking up this Lambda function to your GET method, it will get invoked by API Gateway when the chat app polls the API with GET requests.

8\. Select the blue **Save** button and click **OK** if a pop up asks you to confirm that you want to switch to Lambda integration. Then grant access for API Gateway to invoke the Lambda function by clicking "OK" again. This 2nd popup asks you to confirm that you want to allow API Gateway to be able to invoke your Lambda function.

9\. Click the **Method Execution** back button to return to the method execution overview page. You'll now tell API Gateway what types of HTTP response types you want your API to expose. Click the **Method Response** section of the Method Execution Flow.

10\. Add a 200 HTTP Status response. Click "Add Response", type "200" in the status code text box and then click the little checkmark to save the method response, as shown below.
![Method Response](/Images/Typing-Step10.png)

* You've configured the GET method of the /zombie/talkers resource to allow responses with HTTP status of 200. We could add more response types but we'll skip that for simplicity in this workshop.

11\. Go to the /zombie/talkers/POST method by clicking the "POST" option in the resource tree on the left navigation pane.
![POST Method](/Images/Typing-Step11.png)

12\. We're now going to configure to the /zombie/talkers resource to properly integrate with AWS Lambda on POST requests.

**Perform Steps 4-10 again** just as you did for the GET method. However, this time when you are selecting the Lambda Function for the Integration Request, you'll type "writetalkers" in the auto-fill and select the function that looks something like this... **[CloudformationTemplateName]-[XXXXXXX]-WriteTalkersToDynamoDB-[Your Region]**. This way on POST requests, API Gateway will invoke your **writetalkers** Lambda function. Don't forget to return to the Method Response section for this POST method and add a "200" HTTP response status as you did for the GET method earlier, if it doesn't exist already.

* In these steps you are configuring the POST method that is used by the chat app to insert data into DynamoDB Talkers table with details about which users are typing. You're performing the same exact method configuration for the POST method as you did for your GET method. However, since this POST method is used for sending data to the database, it triggers a different backend Lambda function. This function writes data to DynamoDB while the "GetTalkersToDynamoDB" function was used to retrieve data from DynamoDB.

	* You could optionally include the logic for both the POST and GET operations inside of a single Lambda function with your own built-in logic that properly checks for and handles POSTs and GETs (or other actions).

13\. Go to the /zombie/talkers/OPTIONS method

14\. Select the Method Response.

15\. Add a 200 method response. Click "Add Response", type "200" in the status code text box and then click the little checkmark to save the method response.

16\. Go back to the OPTIONS method flow and select the Integration Response. (To go back, there should be a blue hyperlink titled "Method Execution" which will bring you back to the method execution overview screen).

17\. Select the Integration Response.

18\. Add a new Integration response with a method response status of 200. Click the "Method response status" dropdown and select "200". Leave the "Content Handling" option set to **Passthrough**. When done, click the blue **Save** button.

* In this section you configured the OPTIONS method simply to respond with HTTP 200 status code. The OPTIONS method type is simply used so that clients can retrieve details about the API resources that are available as well as the methods associated with them.

19\. Select the /zombie/talkers resource on the left navigation tree.
![talker resource](/Images/Typing-Step19.png)

20\. Click the "Actions" box and select "Enable CORS" in the dropdown.

21\. Select Enable and Yes to replace the existing values. You should see all green checkmarks for the CORS options that were enabled, as shown below.
![talker resource](/Images/Typing-Step21.png)

* If you don't see all green checkmarks, this is probably because you forgot to add the HTTP Status 200 code for the Method Response Section. Go back to the method overview section for your POST, GET, and OPTIONS method and make sure that it shows "HTTP Status: 200" in the Method Response box.

22\. Click the "Actions" box and select Deploy API  
![talker resource](/Images/Typing-Step22.png)

23\. Select the ZombieWorkshopStage deployment and hit the Deploy button.

* In this workshop we deploy the API to a stage called "ZombieWorkshopStage". In your real world scenario, you'll likely create different stages of the API to reflect different versions that you'd like to maintain.

**LAB 1 COMPLETE**

Head back to the survivor chat app and **Refresh the page** type messages. POST requests are being made to the Talkers API resource which is updating a DynamoDB table continuously with timestamps along with who is typing. Simultaneously, the application is performing a continuous polling (GET Requests) against /zombie/talkers to show which survivors are typing. This displays

![talker resource](/Images/Typing-Done.png)

* * *

## Lab 2 - SMS Integration with Twilio - We are not going to do this Lab, jump to Lab 3

* * *

## Lab 3 - Search over the chat messages with Elasticsearch Service

**What you'll do in this lab...**

In this lab you'll launch an Elasticsearch Service cluster and setup DynamoDB Streams to automatically index chat messages in Elasticsearch for future ad hoc analysis of messages.

**Elasticsearch Service Architecture**
![Overview of Elasticsearch Service Integration](/Images/ElasticsearchServiceOverview.png)

1\. Select the Amazon Elasticsearch icon from the main console page.

2\. Create a new Amazon Elasticsearch domain. Provide it a name such as "[Your CloudFormation stack name]-zombiemessages". Click **Next**.

3\. On the **Configure Cluster** page, leave the default cluster settings and click **Next**.

4\. For the access policy, select the **Allow or deny access to one or more AWS accounts or IAM users** option in the dropdown and fill in your account ID. Your AWS Account ID is actually provided to you in the examples section so just copy and paste it into the text box. Make sure **Allow** is selected for the "Effect" dropdown option. Click **OK**.

5\. Select **Next** to go to the domain review page.

6\. On the Review page, select **Confirm and create** to create your Elasticsearch cluster.

7\. The creation of the Elasticsearch cluster takes approximately 10 minutes.

* Since it takes roughly 10 minutes to launch an Elasticsearch cluster, you can either wait for this launch before proceeding, or you can move on to Lab 4 and come back to finish this lab when the cluster is ready.

8\. Take note of the Endpoint once the cluster starts, we'll need that for the Lambda function.
![API Gateway Invoke URL](/Images/Search-Step8.png)

9\. Go into the Lambda service page by clicking on Lambda in the Management Console.

10\. Select **Create a Lambda Function**.

11\. On the Blueprints screen select **Blank Function** to create a Lambda function from scratch.

12\. In Configure Triggers section, select the DynamoDB event source type and then select the **messages** DynamoDB table. It should appear as **"[Your CloudFormation stack name]-messages"**. Then set the **Batch size** to **5**, the **Starting position** to **Latest** and select the checkbox **Enable trigger**. Then click on Next button.

13\. Give your function a name, such as **"[Your CloudFormation stack name]-ESsearch"**. Keep the runtime at the default. You can set a description for the function if you'd like.

14\. Paste in the code from the ZombieWorkshopSearchIndexing.js file provided to you. This is found in the Github repo in the "ElasticsearchLambda" folder.

15\. On [line 6](/ElasticSearchLambda/ZombieWorkshopSearchIndexing.js#L6) in the code provided, replace the **region** variable with the code for the region you are working in (the region you launched your stack, created your Lambda function etc). If you're working in Oregon region, then leave the code us-west-2 as is.

Then on line 7, replace the **endpoint** variable that has a value of **ENDPOINT_HERE** with the Elasticsearch endpoint created in step 8\. **Make sure the endpoint you paste starts with https://**.

* This step requires that your cluster is finished creating and in "Active" state before you'll have access to see the endpoint of your cluster.

16\. Now you'll add an execution role to your Lambda function which gives permissions for your Lambda function to access AWS resources. For the Role, select **Choose an existing role**, and for the Existing Role, select **"[Your CloudFormation stack name]-ZombieLabLambdaRole"** which is the role that was created for you for this workshop. It has permissions to the Elasticsearch service.

17\. Expand the "Advanced settings" section and find the "Timeout" field for your Lambda function. In the timeout field, change the function timeout to **1** minute. This ensures Lambda can process the batch of messages before Lambda times out. Keep all the other defaults on the page set as is. Select **Next** and then on the Review page, select **Create function** to create your Lambda function.

18\. In the above step, we configured [DynamoDB Streams](http://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html) to capture incoming messages on the table and trigger a Lambda function to push them to our Elasticsearch cluster. Your messages posted in the chat from this point forward will be indexed to Elasticsearch. Post a few messages in the chat, at least 5 as configured in the DynamoDB Streams event source (batch size). You should be able to see that messages are being indexed in the "Indices" section for your cluster in the Elasticsearch Service console.
![API Gateway Invoke URL](/Images/Search-Done.png)

**LAB 3 COMPLETE**

If you would like to explore and search over the messages in the Kibana web UI that is provided with your cluster, you will need to navigate to the Elasticsearch domain you created and change the permissions. Currently you've configured the permissions so that only your AWS account has access. This allows your Lambda function to index messages into the cluster.

To use the web UI to build charts and search over the index, you will need to implement an IP based policy to whitelist your computer/laptop/network or for simplicity, choose to allow everyone access. For instructions on how to modify the access policy of an ES cluster, visit [this documentation](http://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-gsg-configure-access.html). If you choose to open access to anyone be aware that anyone can see your messages, so please be sure to restrict access back to your AWS account when you're done exploring Kibana, or simply delete your ES cluster.  

* * *

## Lab 4 - Slack Integration

**What you'll do in this lab...**

In this lab, you'll integrate a Slack channel with your survivor chat. There may be survivors who use different chat systems and you'll want to communicate with them! After completing this lab, survivors communicating on Slack can send messages to survivors in the Zombie Chat App by configuring a slash command prefix to be used on any messages in their Slack channel that they want to send to the survivors. When Slack users type messages with this Slash command, it will pass the message to your survivor chat API, similiar to the webhook functionality enabled in the Twilio lab!

If you aren't familiar with Slack, they offer a free chat communications service that is popular, especially among the developer community. Slack uses a concept called "Channels" to distinguish different chat rooms. Visit their website to learn more!

**Slack Integration Architecture**
![Overview of Slack Integration](/Images/SlackOverview.png)

1\. Go to [http://www.slack.com](http://www.slack.com) and create a username, as well as a team. If you want to use your existing Slack username and existing team, then proceed with that profile instead of creating a new one.

2\. Once logged into your Slack team, navigate to [https://slack.com/apps](https://slack.com/apps) which should direct you to the app directory for your team. In the search bar in the middle of the App Directory page, type **slash commands** and select it from the options. This will take you to the Slash Commands portal. 

3\. On the Slash Commands page, click **Add configuration**. 

Slash commands allow you to define a command that you can use within Slack to trigger  Slack to perform actions in an event driven manner. In this case we are going to configure a slash command to forward messages to an external source with a webhook. You'll configure your Slash Command to make a POST request to a /zombie/slack API resource you will soon be creating in API Gateway. 

4\. On the Slash Commands configuration page, define a command in the **Commands** text box. Insert **/survivors** as your Slash Command. Then select "Add Slash Command Integration" to save it.

5\. On the Integration Settings page, scroll down to the **Method** configuration andmake sure the **Method** section has "POST" selected from the dropdown options. Then scroll to the **Token** section and copy the Token (or generate a new one) to a text file as you'll need it in the following steps.

6\. Keep the Slack browser tab open and in another tab navigate to the AWS Lambda management console in the AWS Management Console.

7\. Click **Create a Lambda function**. You'll create a Lambda function to parse incoming Slack messages and send them to the Chat Service.

8\. On the Blueprints page select **Blank Function** to create a function from scratch. Also skip past the triggers page by selecting **Next**.

9\. Give your function a name such as **"[Your CloudFormation Stack name]-SlackService"**. For the Nodejs version, you can keep the default Nodejs version selected. Now navigate to the GitHub repo for this workshop, or the location where you downloaded the GitHub files to your local machine.

10\. Open the **SlackService.js** file from the GitHub repo, found in the slack folder. Copy the entire contents of this js file into the Lambda inline edit window.

* This SlackService function will serve as the backend for a new /zombie/slack API resource you will create later. This function accepts incoming messages forwarded from Slack when you use the slash command, it then reformats the parameters and proxies the Slack messages to the zombie survivor chat service (/zombi/message) . This Lambda function verifies that the incoming message has the predefined Slack Token, and it also does a DynamoDB query against the Users table to validate that the user who submitted the message in Slack is a preconfigured survivor in our backend (Remember when you signed up for the chat, you provided your Slack username and team domain as part of the sign-up process). In this workshop, we're using this is as the way to authorize requests against the /zombie/slack resource.

11\. You should have saved the Slack Token string from earlier. Copy the Token string from Slack into the "token" variable on [line 15](/Slack/SlackService.js#L15) in the Lambda function, replacing the string **INSERT YOUR TOKEN FROM SLACK HERE** with your own token.

* Slack provides a unique token associated with your integration. You are copying this token into your Lambda function as a form of validation. When incoming requests from Slack are sent to your API endpoint, and your Lambda function is invoked with the Slack payload, your Lambda function will check to verify that the incoming Token in the request matches the Token you provided in the code. If the token does not match, Lambda returns an error and doesn't process the request.

12\. There are 4 variables you need to insert in the code to communicate with the backend.

a) In the "API" variable, you will insert the fully qualified domain name (FQDN) for your API. The **API.endpoint** variable should show a value of "INSERT YOUR API GATEWAY FQDN HERE INCLUDING THE HTTPS://" on [line 9](/Slack/SlackService.js#L9). Your final FQDN inserted into the code should look something like "https://xxxxxxxx.execute-api.us-west-2.amazonaws.com". This allows the SlackService function to communicate with your API.

b) You should also fill in the region code in the variable **API.region**. This should be the region where you launched CloudFormation.

c) Finally you will also copy in the name of your DynamoDB Users table that was created for you. This should be placed in the **table** variable. You will also need to copy in the name of your "slackindex" (this is an index that was created on the DynamoDB table to assist with querying). These attributes can be found in the Outputs section in CloudFormation. You should be copying the values for **DynamoDBUsersTableName** and **DynamoDBUsersSlackIndex** from CloudFormation.

13\. After you have copied the code into the Lambda inline code console and modified the variables, scroll down to the **Lambda function handler and role** section. For the role, select **Choose an existing role** from the dropdown and then select the role that looks like **[Your stack name]-ZombieLabLambdaRole...**. For simplicity we are reusing the same Lambda role for our functions.

14\. In the Advanced Settings, set the **Timeout** to **30** seconds. Then click **Next**.

15\. On the review page, make sure that everything looks correct.

16\. Click **Create function**. Your Lambda function will be created.

17\. When the function is created, navigate to the API Gateway service in the AWS Management Console. Click into your "Zombie Workshop API Gateway" API. On the left Resources pane, click/highlight the "/zombie" resource so that it is selected. Then select the **Actions** button and choose "Create Resource". For Resource Name, insert **slack** and for Resource Path, insert **slack**. Click "Create Resource" to create your slack API resource. The final resource for your Slack API should be as shown below.
![Create Slack API Resource](/Images/Slack-Step17.png)

*  In this step, you are creating a new API resource that the Slack slash command webhook can forward requests to. In the next steps, you'll create a POST method associated with this resource that triggers your Lambda function. When you type messages in Slack with the correct slash command, Slack will send requests to this resource, which will invoke your SlackService Lambda function to pre-process the payload and make a call to your /zombie/message endpoint to insert the data into DynamoDB.

18\. For your newly created "/slack" resource, highlight it, then click **Actions** and select **Create Method** to create the **POST** method for the /zombie/slack resource. In the dropdown, select **POST**. Click the checkmark to create the POST method. On the Setup page, choose an Integration Type of **Lambda Function**, and select the region that you are working in for the region dropdown. For the Lambda Function field, type "SlackService" for the name of the Lambda Function. It should autofill your function name. Click **Save** and then **OK** to confirm.

19\. Click **Integration Request** for the /slack POST method. We'll create a Mapping Template to convert the incoming query string parameters from Slack into JSON which is the format Lambda requires for parameters. This mapping template is required so that the incoming Slack message can be converted to the right format.

20\. Expand the **Body Mapping Templates** arrow and click **Add mapping template**. In the Content-Type box, enter **application/x-www-form-urlencoded** and click the little checkmark to continue. If a popup appears asking if you would like to secure the integration, click **Yes, secure this integration**. This ensures that only requests with the defined content-types will be allowed.

As you did in the Twilio lab, we're going to copy VTL mapping logic to convert the request to JSON. A new section will appear on the right side of the screen with a dropdown for **Generate Template**. Click that dropdown and select **Method Request Passthrough**.

In the text editor, delete all of the exiting VTL code and copy the following into the editor:

```
{"body": $input.json("$")}
```

Click the grey **Save** button to continue. The result should look like the screenshot below:

![Slack Integration Response Mapping Template](/Images/Slack-Step20.png)

21\. Click the **Actions** button on the left side of the API Gateway console and select **Deploy API** to deploy your API. In the Deploy API window, select **ZombieWorkshopStage** from the dropdown and click **Deploy**.

22\. On the left pane navigation tree, expand the ZombieWorkshopStage tree. Click the **POST** method for the **/zombie/slack** resource. You should see an Invoke URL appear for that resource as shown below.
![Slack Resource Invoke URL](/Images/Slack-Step22.png)

23\. Copy the entire Invoke URL. Navigate back to the Slack.com website to the Slash Command setup page and insert the Slack API Gateway Invoke URL you just copied into the "URL" textbox. Make sure to copy the entire url including "HTTPS://". Scroll to the bottom of the Slash Command screen and click **Save Integration**.

24\. You're ready to test out the Slash Command integration. In the team chat channel for your Slack account, type the Slash Command "/survivors" followed by a message. For example, type "/survivors Please help me I am stuck and zombies are trying to get me!". After sending it, you should get a confirmation response message from Slack Bot like the one below:
![Slack Command Success](/Images/Slack-Step24.png)

**LAB 4 COMPLETE**

Navigate to your zombie survivor chat app and you should see the message from Slack appear. You have configured Slack to send messages to your chat app!
![Slack Command in Chat App](/Images/Slack-Step25.png)

**Bonus Step:**

You've configured Slack to forward messages to your zombie survivor chat app. But can you get messages sent in the chat app to appear in your Slack chat (i.e.: the reverse)? Give it a try or come back and attempt it later when you've finished the rest of the labs! HINT: You'll want to configure Slack's "Incoming Webhooks" integration feature along with a Lambda code configuration change to make POST requests to the Slack Webhook whenever users send messages in the chat app!

* * *
