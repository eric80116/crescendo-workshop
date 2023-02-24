With e-commerce and video personalization use cases, users can simply select the recommenders that map to the user experience they want to create. 

## Create recommender

In this workshop, you will build an e-commerce recommender.
1. Select **Use e-commerce recommenders** tab and click **Create recommenders**

![05-recommender-1](/static/image/05-recommender-1.png)

2. Tick **Recommended for you only** and type your recommender name

3. There are some advance configurations under **Recommender configurations - optional**

* Minimum recommendation requests per second: Configure the baseline recommendation request throughput provisioned by Amazon Personalize
* Emphasis on exploring less relevent items: Configure how frequently recommendations include items with less interactions data or relevance
* Exploration item age cut off: Define the scope of item exploration in days since the latest interaction

Let's keep the default setting.

4. Click **Create recommenders**

![05-recommender-2](/static/image/05-recommender-2.png)

5. Check the recommender be created successfully.

![05-recommender-3](/static/image/05-recommender-3.png)

6. Click your recommender and copy your recommender **ARN** to the clipboard for later use.

![05-recommender-4](/static/image/05-recommender-4.png)

> **Note**

> It would take around 1 hour to build the recommender, you can keep moving on next step