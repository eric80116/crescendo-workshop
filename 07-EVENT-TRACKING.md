Personalize can build an Event Tracker to record the viewers' interactions into Personalize and it would make reference the recent activities to adjust the recommendations in time.
In this chapter, you will build a event tracker first then custom a pipeline to send the view activity to Personalize with following architecture.

![09-event-tracking-1](/static/image/09-event-tracking-1.png)

1. Right click [Personalize](https://us-east-1.console.aws.amazon.com/personalize/home?region=us-east-1) to open in new tab and select **Manage dataset groups** on the left silde menu and click your dataset group. 

2. You will nevigate to the Overview page then click the **Create event tracker** in the Create datasets block.

![09-event-tracking-2](/static/image/09-event-tracking-2.png)

3. Type the **Tracker name** as `personalize-workshop-event-tracker` and click **Next**

![09-event-tracking-3](/static/image/09-event-tracking-3.png)

4. Copy the **Tracking ID** to the clipboard for later use then click **Finish**

![09-event-tracking-4](/static/image/09-event-tracking-4.png)

5. Check the tracker status is **Active**

![09-event-tracking-5](/static/image/09-event-tracking-5.png)

6. Right click [AWS Lambda](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#functions) to open functions page in new tab.

7. Click **Create function** on the top-right then configure with following settings:
* Select **Author from scratch**
* Type `PutEvent-Function` as function name
* Select **Python 3.9** as Runtime
* Select **x86_64** as Architecture
* Expand **Change default execution role** and select **Use an existing role** and select the role you created at Workshop setup step, should be like AmazonPersonalizeWorkshop-PersonalizeIamRole-XXXXXXXXXXXX.
* Click **Create function**

![09-event-tracking-6](/static/image/09-event-tracking-6.png)

Now, you will be switch to Code source page automatically.

8. Navigate to **Configuration** tab and select **Environment variables** and click **Edit**

![09-event-tracking-7](/static/image/09-event-tracking-7.png)

9. Click **Add environment variable**

![09-event-tracking-8](/static/image/09-event-tracking-8.png)

10. Type `TRACKING_ID` as Key and the tracking ID you just recorded on the clipboard as Value
11. Click **Save**

![09-event-tracking-9](/static/image/09-event-tracking-9.png)

12. Switch back to the **Code** tab.
13. Replace the defualt code with the following code.
```
import json
import boto3
import base64
import time
import os

def lambda_handler(event, context):
    personalize_events = boto3.client(service_name="personalize-events")
    for i in event['Records']:
        new_records = json.loads(base64.b64decode(i['kinesis']['data']).decode())
        response = personalize_events.put_events(
        trackingId = os.environ['TRACKING_ID'],
        userId = str(new_records['userid']),
        sessionId = 'session1',
        eventList = [{
            'eventId':'event_'+str(round(time.time())),
            'sentAt':str(new_records['timestamp']),
            'eventType': str(new_records['event_type']),
            'properties': json.dumps({
                'ITEM_ID': str(new_records['itemid']),
            })
        }]
        )
        print(new_records)
        print(response)
    
    return {
        "response": response,
    }
```

![09-event-tracking-10](/static/image/09-event-tracking-10.png)

12. Select **File** and **Save** on the top menu.
13. Click the **Deploy** button to deploy your code

![09-event-tracking-11](/static/image/09-event-tracking-11.png)

14. Right click [Kinesis Data Streams](https://us-east-1.console.aws.amazon.com/kinesis/home?region=us-east-1#/streams/list) to open in new tab and click the **Create data stream** on the top-right.

![09-event-tracking-12](/static/image/09-event-tracking-12.png)

15. Create data stream with the following setting:

* Type the Data stream name as `PutEvent-datastream`
* Select **Provisioned** as Data stream capacity, leave the Provisioned shards as default

16. Click **Create data stream** and check the Status is Active

![09-event-tracking-13](/static/image/09-event-tracking-13.png)

![09-event-tracking-14](/static/image/09-event-tracking-14.png)

17. Back to the [AWS Lambda](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#functions) functions page.

18. Click **PutEvent-Function** you previously created

19. At the function overview, click **Add trigger** button

![09-event-tracking-15](/static/image/09-event-tracking-15.png)

20. In trigger configuration, drop down menu and select **Kinesis**

![09-event-tracking-16](/static/image/09-event-tracking-16.png)

21. In Kinesis stream, select **PutEvent-datastream** you previously created

22. Leave the rest as default and click **Add**. Then you would be navigated back to the function page.

![09-event-tracking-17](/static/image/09-event-tracking-17.png)

![09-event-tracking-18](/static/image/09-event-tracking-18.png)

23. Right click [Amazon API Gateway](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1) to open in new tab

24. In the **REST API** section, click **Build**

![08-front-end-integration-19](/static/image/08-front-end-integration-19.png)

25. Configure with the following settings

* Select **REST** as protocol
* Select **New API** to create a new API
* Type `PutEventAPI` as API name
* Select **Edge optimized** as Endpoint Type

26. Click **Create API**

![08-front-end-integration-20](/static/image/08-front-end-integration-20.png)

27. Drop down the **Actions** and select **Create Resource**

![08-front-end-integration-21](/static/image/08-front-end-integration-21.png)

28. Type `streams` as **Resouce Name** and **Resource Path**

29. Tick **Enable API Gateway CORS**

30. Click **Create Resource**

![08-front-end-integration-22](/static/image/08-front-end-integration-22.png)

31. Select **streams** and drop down the **Actions** again then select **Create Method**

![08-front-end-integration-23](/static/image/08-front-end-integration-23.png)

32. Select **GET** and click the check mark

![08-front-end-integration-24](/static/image/08-front-end-integration-24.png)

33. Configure the GET Method with the following settings:

* Select **AWS Service** as Integration type
* Select **us-east-1** as Lambda Region
* Select **Kinesis** as AWS Service
* Select **POST** as HTTP method
* Type `ListStreams` as **Action**
* Type the role ARN you previous record in the clipboard as **Execution role**
* Click **Save**

![08-front-end-integration-25](/static/image/08-front-end-integration-25.png)

34. Click the **Integration Request** pane, expand the **HTTP Headers** section:

* Choose Add header.
* In the Name column, type `Content-Type`.
* In the Mapped from column, type `'application/x-amz-json-1.1'`.
* Choose the check mark icon to save the setting.

![08-front-end-integration-26](/static/image/08-front-end-integration-26.png)
![08-front-end-integration-27](/static/image/08-front-end-integration-27.png)


35. Expand the Mapping Templates section:

* Select **When there are no templates defined (recommended)**
* Choose Add mapping template.
* For Content-Type, type `application/json`.
* Click the check mark icon to save the Content-Type setting.
* Type `{}` in the template editor.
* Choose the **Save** button to save the mapping template.

![08-front-end-integration-28](/static/image/08-front-end-integration-28.png)

36. Click the **Method Execution** on the top-left and go back Method Execution page.

![08-front-end-integration-29](/static/image/08-front-end-integration-29.png)

37. Click **Test** then leave the setting as default and click the **Test** button.

![08-front-end-integration-30](/static/image/08-front-end-integration-30.png)

![08-front-end-integration-31](/static/image/08-front-end-integration-31.png)

38. On the right side, you can get the information of your **PutEvent-datastream**

![08-front-end-integration-32](/static/image/08-front-end-integration-32.png)

39. Let's move on. Select the **/stream** on the left hierarchy and drop down the **Actions** and select **Create Resource**.

40. Type `stream-name` as **Resouce Name**, type `{stream-name}` **Resource Path** and tick **Enable API Gateway CORS** then click **Create Resource**

![08-front-end-integration-33](/static/image/08-front-end-integration-33.png)

41. Select **/{stream-name}** and drop down the **Actions** again then select **Create Method**

42. Select **GET** and click the check mark

43. Configure the GET Method with the following settings:

* Select **AWS Service** as Integration type
* Select **us-east-1** as Lambda Region
* Select **Kinesis** as AWS Service
* Select **POST** as HTTP method
* Type `DescribeStream` as **Action**
* Type the role ARN you previous record in the clipboard as **Execution role**
* Click **Save**

![08-front-end-integration-34](/static/image/08-front-end-integration-34.png)

44. Click the **Integration Request** pane, expand the **HTTP Headers** section:

* Choose Add header.
* In the Name column, type `Content-Type`.
* In the Mapped from column, type `'application/x-amz-json-1.1'`.
* Choose the check mark icon to save the setting.

![08-front-end-integration-35](/static/image/08-front-end-integration-35.png)

45. Expand the Mapping Templates section:

* Select **When there are no templates defined (recommended)**
* Choose Add mapping template.
* For Content-Type, type `application/json`.
* Click the check mark icon to save the Content-Type setting.
* Type the following code in the template editor.
```
{
    "StreamName": "$input.params('stream-name')"
}
```
* Choose the **Save** button to save the mapping template.

![08-front-end-integration-36](/static/image/08-front-end-integration-36.png)

46. Click the **Method Execution** on the top-left and go back Method Execution page.
47. Click **Test** then type `PutEvent-datastream` as {stream-name} and click the **Test** button. You would see the stream information on the right.

![08-front-end-integration-37](/static/image/08-front-end-integration-37.png)

48. Let's move on. Select the **/{stream-name}** on the left hierarchy and drop down the **Actions** and select **Create Resource**.

49. Type `records` as **Resouce Name** and **Resource Path** then tick **Enable API Gateway CORS** and click **Create Resource**.

![08-front-end-integration-38](/static/image/08-front-end-integration-38.png)

50. Select **/records** and drop down the **Actions** again then select **Create Method**

51. Select **PUT** and click the check mark

52. Configure the PUT Method with the following settings:

* Select **AWS Service** as Integration type
* Select **us-east-1** as Lambda Region
* Select **Kinesis** as AWS Service
* Select **POST** as HTTP method
* Type `PutRecords` as **Action**
* Type the role ARN you previous record in the clipboard as **Execution role**
* Click **Save**

![08-front-end-integration-39](/static/image/08-front-end-integration-39.png)

53. Click the **Integration Request** pane, expand the **HTTP Headers** section:

* Choose Add header.
* In the Name column, type `Content-Type`.
* In the Mapped from column, type `'application/x-amz-json-1.1'`.
* Choose the check mark icon to save the setting.

54. Expand the Mapping Templates section:

* Select **When there are no templates defined (recommended)**
* Choose Add mapping template.
* For Content-Type, type `application/json`.
* Click the check mark icon to save the Content-Type setting.
* Type the following code in the template editor.
```
{
    "StreamName": "$input.params('stream-name')",
    "Records": [
        #foreach($record in $input.path('$.Records'))
        {
            "Data": "$util.base64Encode($record.Data)",
            "PartitionKey": "$record.PartitionKey"
        }
        #end
    ]
}
```
* Choose the **Save** button to save the mapping template.

![08-front-end-integration-40](/static/image/08-front-end-integration-40.png)

55. Click the **Method Execution** on the top-left and go back Method Execution page.

56. Click **Test** then type `PutEvent-datastream` as {stream-name} and type the following input in the **Request Body** to simulate the interaction data injection.

```
{
    "Records": [
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"zGRmEOdTo648OJoqhmazwKPuWpA=\",\"timestamp\":\"1676975804\",\"event_type\":\"click\"}",
            "PartitionKey": "1"
        },
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"RZ4KlaqSK4zAVf/FdprF522ip14=\",\"timestamp\":\"1676975924\",\"event_type\":\"click\"}",
            "PartitionKey": "1"
        },
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"TmnhhnrtAe/OuxEEDAEhHFpXTTQ=\",\"timestamp\":\"1676975984\",\"event_type\":\"click\"}",
            "PartitionKey": "1"
        },
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"VcODDeZtFt9rFcj6rW0yb4iKh50=\",\"timestamp\":\"1676976044\",\"event_type\":\"click\"}",
            "PartitionKey": "1"
        },
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"zGRmEOdTo648OJoqhmazwKPuWpA=\",\"timestamp\":\"1676976644\",\"event_type\":\"adds_to_cart\"}",
            "PartitionKey": "1"
        },
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"VcODDeZtFt9rFcj6rW0yb4iKh50=\",\"timestamp\":\"1676977244\",\"event_type\":\"adds_to_cart\"}",
            "PartitionKey": "1"
        },
        {
            "Data": "{\"userid\":\"146456223\",\"itemid\":\"zGRmEOdTo648OJoqhmazwKPuWpA=\",\"timestamp\":\"1676977844\",\"event_type\":\"checkouts\"}",
            "PartitionKey": "1"
        }
    ]
}
```


57. Click the **Test** button. You would see the stream information on the right.


![08-front-end-integration-41](/static/image/08-front-end-integration-41.png)


58. Let's go back [AWS Lambda](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#functions) to check our Lambda function be triggered or not

59. Click the **PutEvent-Function** you previously created.

60. Click **Monitor** Tab and **View CloudWatch logs** button

![08-front-end-integration-42](/static/image/08-front-end-integration-42.png)

61. You would be navigated to the CloudWatch page. You will see a log stream be created. Click it.

![08-front-end-integration-43](/static/image/08-front-end-integration-43.png)

62. You can see the interaction events be injected to Personalize event tracker successfully.

![08-front-end-integration-44](/static/image/08-front-end-integration-44.png)

63. Now, you can try get recommendation for user 146456223 by the console or Python SDK again. The recommendation list should be changed.

**Congratulation! You finished the workshop!**

You can get more detail about Personalize from the [documentation](https://docs.aws.amazon.com/personalize/latest/dg/what-is-personalize.html). 
If you are interested in the detail about API Gateway and Kinesis integration, please reference the [documentation](https://docs.aws.amazon.com/zh_tw/apigateway/latest/developerguide/integrating-api-with-aws-services-kinesis.html)












