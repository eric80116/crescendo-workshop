---
title : "Workshop setup"
weight : 12
---

Deploy a cloud formation template that will perform the initial setup work for you. This CloudFormation template will complete the following:
1. Create a Role with Personalize access.
2. Create a S3 bucket with appropriate policy for workshop. 

:::alert{header="Important" type="warning"}
All AWS resources for this workshop will be deployed in **us-east-1** reagion.
:::

## CloudFormation Deployment Steps

1. Click :button[Cloudformation Template]{href=":assetUrl{path="/personalize-workshop-assets/AmazonPersonalizeWorkshop.yaml" source=s3}" action="download"}  to download CloudFormation template.


2. Click [Cloudformation console link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/template)

3. Select **Upload a template file** and choose the downloaded template `AmazonPersonalizeWorkshop.yaml` as shown in below screenshot and click next.

![02-workshop-setup-1](/static/image/02-workshop-setup-1.png)

4. For Stackname, enter `AmazonPersonalizeWorkshop`, scroll down and click next till you reach review.

![02-workshop-setup-2](/static/image/02-workshop-setup-2.png)

5. For Capabilities, check the on Acknowledge box as shown below and click on **Create Stack** button.

![02-workshop-setup-3](/static/image/02-workshop-setup-3.png)

6. Wait the status change to **CREATE_COMPLETE**

![02-workshop-setup-4](/static/image/02-workshop-setup-4.png)

7. Go to the [IAM](https://console.aws.amazon.com/iamv2/home#/roles) and check the role `AmazonPersonalizeWorkshop-PersonalizeIamRole-XXXXXXXXXXXX` be created. Click into your role and **copy the role ARN to the clipboard** since you will use it in later chapters.

![02-workshop-setup-5](/static/image/02-workshop-setup-5.png)
![02-workshop-setup-5-1](/static/image/02-workshop-setup-5-1.png)

8. Go to the [S3](https://console.aws.amazon.com/s3/buckets?region=us-east-1) and check the bucket `bucket-personalize-XXXXXXXXXXXX` be created. **Copy the bucket name to the clipboard** since you will use it in later chapters.

![02-workshop-setup-6](/static/image/02-workshop-setup-6.png)

9. Click :button[Workshop Sample Data]{href=":assetUrl{path="/personalize-workshop-assets/Workshop_Data.zip" source=s3}" action="download"} then unzip the file.

10. Unzip the file then you can either drag and drop or click Upload button to upload the folders in **Workshop_Data** to the S3 bucket.

![02-workshop-setup-7](/static/image/02-workshop-setup-7.png)

11. Navigate to the [SageMaker notebook instance](https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances) to check the instance be provisioned successfully.

![02-workshop-setup-8](/static/image/02-workshop-setup-8.png)


**Now your environment are setted!**