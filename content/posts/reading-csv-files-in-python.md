+++
title = "Reading CSV File in Python"
description = "When it comes to data processing, Python comes first. The complex task in data processing is creating structured data called data lake from various sources like files, databases, logs, audio, video etc. Python comes with common file processing packages as part of its distribution. In this article we will use `csv` and `pandas` packages to read CSV files."
author = "Saisyam"
publishdate = "2020-04-04"
lastmodified = "2020-06-09"
categories = ["data analytics"]
type = "post"
tags = [
    "csv",
    "python",
    "pandas"
]
+++

Today data is available in various formats, like, CSV, JSON, TXT etc. Python provides all the necessary tools to parse these files. In this article we will learn how to parse a large CSV file using Python.

We will use Python `csv` package and `pandas` to parse the CSV file. I have taken a CSV file of baby names from [here](https://raw.githubusercontent.com/hadley/data-baby-names/master/baby-names.csv). This file contains 250K records of baby names. I am using this data to generate user database to test the login API I have built.

## Reading with Python `csv` package
The goal is read the name from the existing CSV file and create an `email` which is `name` + `4 digit random number` + ` @gmail.com` and `password`, a ten digit random string. I will be using Python `string` and `random` packages to achieve this. Let's look at random string function.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv.py/l/5-10" >}}

The above method generates a random string of default 10 characters. Following code will iterate over the record in CSV and create above mentioned fields using the `random_string` function.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv.py/l/10-22" >}}

Finally we will use the above two functions to generate the user data we need.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv.py/l/22-33" >}}

The above code will read the input CSV file and write back the `name`, `email` and `password` to the output CSV file. The complete source code is available at [Github](https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv.py).

{{< adsense type="article" >}}

## Reading with `pandas` package

`pandas` provides methods to read and manipulate CSV files in few lines of code. The following snippet will show how to load a CSV file and display the top five records:

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv_pd.py/l/9-15" >}}

The input CSV file contains four columns, `year`, `name`, `percent` and `sex`. We need only the `name` field, so we drop other fields.

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv_pd.py/l/15-21" >}}

The `drop` method will drop the fields from the dataframe, `axis=1` specifies we want to remove the column and `inplace=True` will change the current data frame instead of creating a new one with the deleted columns. `head` will display the top 5 records in the dataframe.

Now we add two new fields to dataframe, `email` and `password` created in the same way as mentioned above:

{{< gistify url="https://gistify.saisyam.com/https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv_pd.py/l/21-32" >}}

The complete source code is available at [Github](https://github.com/saisyam/python-tools/blob/master/readcsv/readcsv_pd.py)


