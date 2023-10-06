# Spotify-end-to-end-data-pipeline

## GOAL OF THE PROJECT
To get the trend of most heard songs every week on spotify to understand the music trend.

<img width="1418" alt="Screenshot 2023-10-02 at 10 23 24 AM" src="https://github.com/itsmeBhavana/Spotify-end-to-end-data-pipeline/assets/37996011/c0c07f18-0872-414d-8357-99c9c4e1085b">

### Steps
1. Spotify API to extract data from Spotify.
2. Deploy our code into AWS Lambda
3. On top of that we run cloud watch since we are running our code day-day or week-week basis. So, we can schedule all those things using cloud watch(data trigger).  
   **Extraction:** Cloudwatch runs the AWS Lambda code as per the trigger daily or weekly and stores the raw data in Amazon S3 bucket.  
4. Whenever data is put in S3, we trigger the transformations to occur through Lambda functions.  
   **Transformation:** Whenever the data in put in raw S3 bucket, Lambda transformation function is triggered(Trigger put) and the transformed data is put into S3 bucket for               transformed data.  
5. Crawler(Infer Schema) crawls the data on transformed S3 bucket and builds a Glue Catalog(info about the data of the tables like number of columns and their data types).
6. Once we have our data on Glue catalog, we can use a service called Amazon Athena(Analytics) to directly run SQL queries on top of it.    
   **Load:** Crawler crawls the tables in Transformed S3 bucket and builds a Glue catalog and then we can run Athena SQL queries on top of it to Analyse the data.


### Working Steps on AWS
1. Create 2 S3 buckets raw_data and transformed_data
2. Write a lamnbda function spotify_api_data_extract
3. Add a layer called spotipy_layer since lambda does not understand pip and we need to install spotipy. So, we are doing it via lambda.
4. **import boto3:boto3 is a package that is created to programmatically communicate with AWS services.**
5. **In AWS when two services wanna talk to each other, then we create IAM Role**
6. **Timeout Error: In Configurations>General Configurations>Increase the timeout to 1min 30sec**
7. Create a lambda function spotify_transformation_load_function for transformation to place all the data from to_processed to processed folder in raw bucket.
8. Give spotify_transformation_load_function AWS_lambda_role permission so that it can talk to spotify_api_data_extract.
9. Create the triggers for both the lambda functions
10. **To skip the first row if it header, then go to appropriate Glue table and Advanced Properties>Actions>Edit Table>Add>Skip.header.line.count : 1**

   
