Users can get recommendations in real-time or batch recommendations. 
* For real-time recommendations, if you choose to use the solution to do real-time recommendation, you must create a campaign to get recommendations, but if you choose to use the recommender, it is not required. 
* For batch recommendations, you don't need to create a campaign.

## Getting real-time recommendations from the recommender

1. In the left-side navigation pane, choose **Recommenders**.

![07-real-time-and-batch-inference-1](/static/image/07-real-time-and-batch-inference-1.png)

2. Click the recommender you just created.

![07-real-time-and-batch-inference-2](/static/image/07-real-time-and-batch-inference-2.png)

3. Click the **Test recommender** on the top-right.

![07-real-time-and-batch-inference-3](/static/image/07-real-time-and-batch-inference-3.png)

4. Type the User ID, for example, `146456223` and click the **Get recommendations**.

![07-real-time-and-batch-inference-4](/static/image/07-real-time-and-batch-inference-4.png)

5. You would get the recommendations as following.

![07-real-time-and-batch-inference-5](/static/image/07-real-time-and-batch-inference-5.png)


## Getting batch inference

Personalize can support batch inference to do user segmentations. In this workshop, we already prepared the batch input in S3 bucket, let's see how it works.

1. In the left-side navigation pane, choose **Batch segment jobs**.

![07-real-time-and-batch-inference-10](/static/image/07-real-time-and-batch-inference-10.png)

2. Click **Create batch segment job** on the top-right.

![07-real-time-and-batch-inference-11](/static/image/07-real-time-and-batch-inference-11.png)

3. In the **Job details**, do the following actions:
* Type your job name
* Select the **Solution** and **Solution version ID**
* Type the Number of users as `1000`


![07-real-time-and-batch-inference-12](/static/image/07-real-time-and-batch-inference-12.png)

4. In the **Input source**, type the S3 path which included our prepared testing input. It should be like `s3://bucket-personalize-XXXXXXXXXXXX/input/test/`.

5. In the **Output destination**, type the S3 path. It should be like `s3://bucket-personalize-XXXXXXXXXXXX/output/`.

6. In the **IAM Role**, select **Use an existing service role** and type the Role ARN you recorded in the evironment setting section.

7. Click **Create batch segment job**.

![07-real-time-and-batch-inference-13](/static/image/07-real-time-and-batch-inference-13.png)

> **Note**

> It would take some time, you can keep moving on next chapter first and check the result later

7. Check the batch inference job execute successfully.

![07-real-time-and-batch-inference-14](/static/image/07-real-time-and-batch-inference-14.png)

8. Navigate to the S3 output and download the file to check the results as below.

![07-real-time-and-batch-inference-15](/static/image/07-real-time-and-batch-inference-15.png)

![07-real-time-and-batch-inference-16](/static/image/07-real-time-and-batch-inference-16.png)





