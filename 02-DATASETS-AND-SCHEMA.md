## Create dataset group

1. Go to [Personalize](https://us-east-1.console.aws.amazon.com/personalize/home?region=us-east-1){:target="_blank"} and click **Create dataset group** on the left silde menu.
2. Type your dataset group name and select **E-commerce** domain datast group and click **create dataset group and continue**.

![03-dataset-group-1](/static/image/03-dataset-group-1.png)

Next, you will start to create schema and import data.

## Create Interaction dataset
1. Type your dataset name
2. Choose **Create a new domain schema by modifying the existing default schema for your domain**
3. Type your schema name

![04-dataset-and-schema-1](/static/image/04-dataset-and-schema-1.png)

4. Copy the following schema and paste to the **Schema definition**.
```
{
	"type": "record",
	"name": "Interactions",
	"namespace": "com.amazonaws.personalize.schema",
	"fields": [
		{
			"name": "USER_ID",
			"type": "string"
		},
		{
			"name": "ITEM_ID",
			"type": "string"
		},
		{
			"name": "TIMESTAMP",
			"type": "long"
		},
		{
			"name": "EVENT_TYPE",
			"type": "string"
		}
	],
	"version": "1.0"
}
```
5. Click **Create dataset and continue**

![04-dataset-and-schema-2](/static/image/04-dataset-and-schema-2.png)

6. Choose **Import data from S3** and type **import job name**.
7. Type data location with S3 location URL of interaction folder.
e.g.  s3://bucket-personalize-xxxxxxxxxxxx/input/interaction/  

(bucket-personalize-xxxxxxxxxxxx is the bucket name you recorded in the clipboard.)

![04-dataset-and-schema-3](/static/image/04-dataset-and-schema-3.png)

8. Choose **Enter a custom IAM role ARN** and type IAM role ARN you recorded in the clipboard at Workshop setup.

9. Click **Finish**

![04-dataset-and-schema-4](/static/image/04-dataset-and-schema-4.png)

10. Wait for importing Interaction dataset successfully. After few minutes, it will show as below.

![04-dataset-and-schema-5](/static/image/04-dataset-and-schema-5.png)

**It would take some time for checking your dataset and schema, you can keep moving on next step to import user dataset**

