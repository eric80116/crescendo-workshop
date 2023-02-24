In this chapter, users will use pre-provisioned SageMaker notebook instance to run the Python script to show how to use Python SDK to inference model.

## Environemt Setting

1. Navigate to the [SageMaker notebook instance](https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/notebook-instances) and you can see a pre-provisioned notebook instance named **AmazonPersonalizeWorkshop** then click **Open Jupyter**

![11-inference-by-sdk-1](/static/image/11-inference-by-sdk-1.png)

2. Expand the **New** on the top-right and select **Terminal** and you will be navigated to anew tab with terminal.

![11-inference-by-sdk-2](/static/image/11-inference-by-sdk-2.png)

3. Type the following command in the terminal, **please replace the bucket name to your bucket**.
```
cd SageMaker
aws s3 sync s3://bucket-personalize-XXXXXXXXXXXX/inference-by-sdk inference-by-sdk 
```

![11-inference-by-sdk-3](/static/image/11-inference-by-sdk-3.png)

4. Back to the jupyter notebook tab, you should see the script be downloaded.

![11-inference-by-sdk-4](/static/image/11-inference-by-sdk-4.png)

## Getting real-time recommendations from the recommender

1. Click the **inference-by-sdk** folder and open **crescendolab_personalize_realtime_inference.ipynb** notebook.


> **Note**  
> If you got the **'Kernel not found'** message, please select **conda_python3** from drop-down list then click **Set Kernel**


2. At the first cell, replace the \<userID\> to `146456223` and \<recommendARN\> to the recommenderARN you previous record in the clipboard.

![11-inference-by-sdk-5](/static/image/11-inference-by-sdk-5.png)

3. Import the related library and build a client with personalize-runtime

![11-inference-by-sdk-6](/static/image/11-inference-by-sdk-6.png)

4. Get recommendation for user 146456223 from the recommender

![11-inference-by-sdk-7](/static/image/11-inference-by-sdk-7.png)

5. Import the item table for look up what item be recommended

![11-inference-by-sdk-8](/static/image/11-inference-by-sdk-8.png)

## Getting user segmentation by batch job

1. Back to jupyter home page then click the **inference-by-sdk** folder and open **crescendolab_personalize_batch_inference.ipynb** notebook.


> **Note**  
> If you got the **'Kernel not found'** message, please select **conda_python3** from drop-down list then click **Set Kernel**


2. At the first cell, replace the \<solutionArn\>, \<roleArn\> and the bucket name of jobInputPath and jobOutputPath with the information you previously record in the clipboard.

![11-inference-by-sdk-9](/static/image/11-inference-by-sdk-9.png)

3. Import the related library and build a client with personalize

![11-inference-by-sdk-10](/static/image/11-inference-by-sdk-10.png)

4. Create a batch segment job

![11-inference-by-sdk-11](/static/image/11-inference-by-sdk-11.png)

5. Check the status of batch segment job, you can go to S3 bucket to check the file when the status is ACTIVE

![11-inference-by-sdk-12](/static/image/11-inference-by-sdk-12.png)

![11-inference-by-sdk-13](/static/image/11-inference-by-sdk-13.png)





