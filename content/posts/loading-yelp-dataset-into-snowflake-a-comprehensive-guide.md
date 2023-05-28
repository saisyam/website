---
title: "Loading Yelp Dataset into Snowflake: A Comprehensive Guide"
categories:
  - Data Analytics
tags:
  - Snowflake
  - Yelp Dataset
date: 2023-05-28
image: /yelp_dataset.jpg
featured: 1
---
In this blog post, we will explore how to load the Yelp dataset, which comprises business, review, and user information, into Snowflake - a cloud-based data warehousing platform. The Yelp dataset is a popular dataset used by data enthusiasts and professionals worldwide to perform analysis and build models around review systems. 

{{< table_of_contents >}}

{{< adsense >}}

## Prerequisites
To follow this guide, you'll need:

* **Snowflake Account:** You can create one on the [Snowflake website](https://snowflake.com). The basic tier should suffice for learning purposes.
* **Yelp Dataset:** You can download the Yelp dataset [here](https://www.yelp.com/dataset).
* **SnowSQL:** This is Snowflake's command line interface. Download it from [here](https://docs.snowflake.com/en/user-guide/snowsql-install-config.html).

## Preparing Yelp Dataset
Yelp dataset contains data related to business, checkins, reviews, users and tips. But we are interested in the following files:
* **yelp_academic_dataset_business.json** - List of around 150k business with location, working hours, categories etc.
* **yelp_academic_dataset_review.json** - Around 6M reviews related to the businesses mentioned in the above file

We follow below steps to load these JSON files into Snowflake.
1. Create Database and tables in Snowflake for business, review and user
2. Upload these json file to Snowflake stage
3. Load these JSON files from stage into the tables

Let's go step by step in getting Yelp data to Snowflake.

{{< adsense >}}

## Create database and tables in Snowflake for business and review
Once you analyze the JSON files you will understand what type of data they are capturing and create table structure based on that. Below are the Snowflake SQL commands to create tables for business, review and user.

Create a database in Snowflake:

```SQL
CREATE DATABASE yelp;
USE DATABASE yelp;
CREATE SCHEMA dataset;
USE SCHEMA dataset;
```
**Note:** I use `public` schema. Make sure you grant necessary permissions to read/write. 

SQL for business:

```SQL
CREATE TABLE business_info (
    business_id STRING,
    name STRING,
    address STRING,
    city STRING,
    state STRING,
    postal_code STRING,
    latitude FLOAT,
    longitude FLOAT,
    stars FLOAT,
    review_count INTEGER,
    is_open BOOLEAN,
    attributes VARIANT,
    categories STRING,
    hours VARIANT
);
```
SQL for reviews:

```SQL
CREATE TABLE reviews (
    review_id STRING,
    user_id STRING,
    business_id STRING,
    stars FLOAT,
    useful INTEGER,
    funny INTEGER,
    cool INTEGER,
    text STRING,
    date TIMESTAMP_NTZ
);
```

**Note:** Reviews JSON is more than 1 GB which is difficult to open in an editor. If you are using Linux/Mac then you can use the `more` terminal command to check the sample data in those large files.

```shell
$ more yelp_academic_dataset_review.json
{"review_id":"KU_O5udG6zpxOg-VcAEodg","user_id":"mh_-eMZ6K5RLWhZyISBhwA","business_id":"XQfwVwDr-v0ZS3_CbbE5Xw","stars":3.0,"useful":0,"funny":0,"cool":0,"text":"If you decide to eat here, just be aware it is going to take about 2 hours from beginning to end. We have tried it multiple times, because I want to like it! I have been to it's other locations in NJ and never had a bad experience. \n\nThe food is good, but it takes a very long time to come out. The waitstaff is very young, but usually pleasant. We have just had too many experiences where we spent way too long waiting. We usually opt for another diner or restaurant on the weekends, in order to be done quicker.","date":"2018-07-07 22:09:11"}
...
```

{{< adsense >}}

## Upload Yelp files to Snowflake stage
You can use Snowflake web UI as well to create stage.

Let's login to `SnowSQL`:

```shell
snowsql -a <account url> -u <usernmae> -d <database name> -s <schema name> -r <role>
```
It will prompt for the password. On successful login, you will be taken to the SnowSQL prompt, like this:

```shell
sdampuri#DEMO_WH@YELP_DATASET.PUBLIC>
```
We will use `SnowSQL` to upload JSON files to Snowflake stage. Let's create a stage first:

```SQL
CREATE STAGE yelp_stage
FILE_FORMAT = (TYPE = 'JSON');
```
Once we have the stage created we can upload the files. Make sure you have necessary permissions to upload to this stage. 
Now let's upload the files to the above stage using `PUT` command:

```shell
put file:///Users/saisyam/work/yelp_dataset/yelp_academic_dataset_business.json @YELP_STAGE AUTO_COMPRESS=FALSE;
```
Yes, I want to keep my JSON uncompressed even the size is large. The upload will depend on your Internet speed.

## Load these JSON files from stage into the tables
We can load the JSON files from stage to table using `COPY` command:

To load businesses into table:

```SQL
COPY INTO business_info
FROM (
    SELECT 
        $1:business_id::string,
        $1:name::string,
        $1:address::string,
        $1:city::string,
        $1:state::string,
        $1:postal_code::string,
        $1:latitude::float,
        $1:longitude::float,
        $1: stars::float,
        $1:review_count::integer,
        IFF($1:is_open::integer = 1, TRUE, FALSE),
        $1:attributes,
        $1:categories::string,
        $1:hours
    FROM @yelp_stage/yelp_academic_dataset_business.json
);
```
To load reviews into table:
```SQL
COPY INTO reviews
FROM (
    SELECT 
        $1:review_id::string,
        $1:user_id::string,
        $1:business_id::string,
        $1: stars::float,
        $1:useful::integer,
        $1:funny::integer,
        $1: cool::integer,
        $1:text::string,
        TO_TIMESTAMP($1: date::string, 'YYYY-MM-DD HH24:MI:SS')
    FROM @yelp_stage/yelp_academic_dataset_review.json
);
```
It took 3.37s to load 6,990,280 rows into the reviews table using `X-SMALL` warehouse. Now we have the data ready to perform analytics.

{{< adsense >}}

## Conclusion
In this blog post, we discussed the steps to load the Yelp dataset, which contains business, review, and user information, into Snowflake. Following this guide, you should be able to load this data into Snowflake for your own projects. The process involves data preprocessing, setting up staging area in Snowflake, and then loading the data into Snowflake using the COPY INTO command. Happy data loading!