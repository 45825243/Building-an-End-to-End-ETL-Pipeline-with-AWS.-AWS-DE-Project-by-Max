# Building an End-to-End ETL Pipeline with AWS. AWS DE Project by Max

## [You can find the full description here](https://medium.com/@kazarmax/from-api-to-dashboard-building-an-end-to-end-etl-pipeline-with-aws-3c1f4048676d). Its clear and concise material. In my steps you can see thing where I met troubles and trying to describe it better for myself.

### Prerequisites

1. Install `VS Code` and `Python`.
2. Create [Adzuna API account](https://developer.adzuna.com/).
3. Write down your `app_id` and `app_key` (you can find it after registration in **Dashboard** panel **API Access details**).
*Example* - Application ID 80393870; Application Keys 875441c752735976f76161157548e0f0.
4. Create [AWS Account](https://aws.amazon.com/account/).
5. You need a Google Account to access [Looker Studio](https://lookerstudio.google.com/u/0/navigation/reporting).
6. Its good to have Jupiter Notebook. To run it type `jupyter notebook` in your terminal 
7. In the project we used **US East (N. Virginia) us-east-1**. You have to use it as well, otherwise you need to change this option in the code in every document.

### Step 1. Researching Adzuna API

1. In my VSCode I created new file `adzuna_research.ipynb` and put the code from the `adzuna_research.py`.
2. Make sure you preinstalled libraries like 'pandas' and 'Loguru'.
3. Don't forget to change the content of variables (line 10,11). I did it in a wrong way so got an error.
```
Original strings:
    ADZUNA_APP_ID = os.getenv('ADZUNA_APP_ID')
    ADZUNA_APP_KEY = os.getenv('ADZUNA_APP_KEY')
I put content in a wrong way:
    ADZUNA_APP_ID = os.getenv('80393870')
    ADZUNA_APP_KEY = os.getenv('875441c752735976f76161157548e0f0')
But supposed to do this:
    ADZUNA_APP_ID = '80393870'
    ADZUNA_APP_KEY = '875441c752735976f76161157548e0f0'
```
4. After that I was able to run my ipynb and used `jobs_df.head()` to check I have the data I need.

### Step 2. Creating Amazon S3 bucket and folders

1. You can't create a bucket named `adzuna-etl-project` because name already exists and must be unique within the global namespace. Use something like `vlad-adzuna-etl-project`. 
2. Don't forget to pay attention to future documents and files where you must replace `adzuna-etl-project` with your bucket name.
3. Structure of the bucket:
```
vlad-adzuna-etl-project:
   - raw_data
      - to_process
      - processed
   - transformed_data
      - to_migrate
      - migrated
```
4. It's a nice thing to add **S3**, **IAM**, and **Lambda** to favorites since we will work with them closely. To do this search in your AWS account and click on a star near the service.

![favorites](pictures\awsservices.png)

### Step 3. Developing Lambda function to extract data

1. When you're creating policy (**step 3.1 / 11**) on this step don't forget to edit the "Resource" field in a JSON file you put into.

![Policy](pictures\prpolicy.png)

2. Don't forget to change lines 15-17 (**Step 3.3 / 1**) using your ID, key and bucket name.

3. For future needs you need to edit lines below. You can find AWS account ID under "My security credentials" (click your username on the top right corner and choose Security credentials).

![Policy2](pictures\policy.png)

### Step 4. Developing Lambda function to transform data

1. Don't forget to change the line 97 (**Step 4.2 / 1**) using your ID,key and bucket name.
2. When you run the test now you'll get the syntax error, so just go on.

### Step 5. Creating and configuring Redshift Serverless storage

1. Don't forget to write down the info (**Step 5.12**) like this:
```
IAM roles
arn:aws:iam::820242936798:role/service-role/AmazonRedshift-CommandsAccessRole-20241006T132615
```
2. Set a minimal Base capacity (**Step 5.4**) it's enough for our project.

### Step 6. Developing Lambda function to load data into Redshift

1. Check the lines 12, 13, 98 when you're on **step 6.2 / 1**. It's our third Lambda function.

### Step 7. Creating and Configuring Step Function

1. Don't forget to change region (if applicable) and accountID on **Step 7.1 / 11**

2. Don't forget to change region (if applicable) and accountID on **Step 7.2 / 4** + check the Lambda Functions' names






