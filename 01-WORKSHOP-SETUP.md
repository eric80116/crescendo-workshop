## Event Engine AWS Account access

Go to the [Event Engine](https://dashboard.eventengine.run/login) and input event hash provided by instructor then click **Accept Terms & Login**.

![01-environment-setup-1](/static/image/01-environment-setup-1.png)

Click on "Email One-Time Password (OTP)".

![01-environment-setup-2](/static/image/01-environment-setup-2.png)

You are redirected to the following page:

![01-environment-setup-3](/static/image/01-environment-setup-3.png)

Enter your email address and click on **Send passcode**.

![01-environment-setup-4](/static/image/01-environment-setup-4.png)

You are redirected to the following page:

![01-environment-setup-5](/static/image/01-environment-setup-5.png)

Check your mailbox, copy-paste the one-time password, and click on **Sign in**.

![01-environment-setup-6](/static/image/01-environment-setup-6.png)

You are redirected to the Team Dashboard. Click on **AWS Console** and **Open Console**. Then you will be redirect to the **AWS Console**.

![01-environment-setup-7](/static/image/01-environment-setup-7.png)

![01-environment-setup-8](/static/image/01-environment-setup-8.png)

![01-environment-setup-9](/static/image/01-environment-setup-9.png)

> **Note**  
> You should see the account like TeamRole/MasterKey@xxxx-xxxx-xxxx on the top-right.


## CloudFormation Deployment Steps

1. Download and unzip the **Workshop_material.zip** previously sent by the instructor.

2. Right click [Cloudformation console link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template) and open link in new tab.

3. Select **Upload a template file** and choose the downloaded template `AmazonPersonalizeWorkshop.yaml` as shown in below screenshot and click next.

![02-workshop-setup-1](/static/image/02-workshop-setup-1.png)

4. For Stackname, enter `AmazonPersonalizeWorkshop`, scroll down and click next till you reach review.

![02-workshop-setup-2](/static/image/02-workshop-setup-2.png)

5. For Capabilities, check the on Acknowledge box as shown below and click on **Create Stack** button.

![02-workshop-setup-3](/static/image/02-workshop-setup-3.png)

6. Wait the status change to **CREATE_COMPLETE**

![02-workshop-setup-4](/static/image/02-workshop-setup-4.png)

7. Right click [IAM](https://console.aws.amazon.com/iamv2/home#/roles) to open in new tab and check the role `AmazonPersonalizeWorkshop-PersonalizeIamRole-XXXXXXXXXXXX` be created. Click into your role and **copy the role ARN to the clipboard** since you will use it in later chapters.

![02-workshop-setup-5](/static/image/02-workshop-setup-5.png)
![02-workshop-setup-5-1](/static/image/02-workshop-setup-5-1.png)

8. Right click [S3](https://console.aws.amazon.com/s3/buckets?region=us-east-1) to open in new tab and check the bucket `bucket-personalize-XXXXXXXXXXXX` be created. **Copy the bucket name to the clipboard** since you will use it in later chapters.

![02-workshop-setup-6](/static/image/02-workshop-setup-6.png)

9. Open **Workshop_Data** folder previously sent by the instructor.

10. You can either drag and drop or click Upload button to upload the folders in **Workshop_Data** to the S3 bucket.

![02-workshop-setup-7](/static/image/02-workshop-setup-7.png)

11. Right click [SageMaker notebook instance](https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances) to open in new tab and check the instance be provisioned successfully.

![02-workshop-setup-8](/static/image/02-workshop-setup-8.png)


**Now your environment are setted!**

Move on to [02-DATASETS-AND_SCHEMA](02-DATASETS-AND_SCHEMA.md)

