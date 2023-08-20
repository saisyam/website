---
title: "Unlock Quality Insights with Great Expectations Python Library"
categories:
  - Data Analysis
tags:
  - Python
  - Great Expectations
date: 2022-12-31
image: /quality_insights.jpg
featured: 1
---
Poor data quality can be a major issue for businesses, resulting in costly errors and incorrect decisions being made. To avoid these problems, it is important to take steps to ensure the quality of data being used. One way to do this is to implement a data quality solution such as [Great Expectations](https://greatexpectations.io/). 

With this tool, users can quickly and easily identify data quality issues and ensure that their data is of the highest quality. Additionally, Great Expectations has a built-in testing framework that allows users to quickly write and execute tests on their data. This makes it easier to maintain data quality over time and ensure that data is always up to date and correct. By implementing a data quality solution like Great Expectations, businesses can ensure that their data is of the highest quality and that costly errors and incorrect decisions are avoided.

In this article we will discuss how to setup Great Expectations and apply expectations or rules on a sample dataset. Data engineers can use Great Expectations in different ways. In this post we will discuss code based approach instead of command line approach.

{{< table_of_contents >}}

{{< adsense >}}

## What is Great Expectations?
The definition from Great Expectations website:

> Great Expectations is a Python-based open-source library for validating, documenting, and profiling your data. It helps you to maintain data quality and improve communication about data between teams.

Great Expectations is like unittests for your data. With Great Expectations you can assert what you expect from the data you load and transform. The main features of Great Expectations for Data engineers are:

1. Assert data from other teams or vendors and ensure its validity
2. Easily integrates into your pipelines in Databricks and Apache Spark
3. Supports different dataset types - Pandas Dataframe, Spark Dataframe and Databases with SQLAlchemy
4. Generates metrics and documentation for the results
5. Provides support to create custom expectations and custom actions to move invalid data to other locations  
6. Integrates seamlessly with DAG execution tools like [Airflow](https://airflow.apache.org), [dbt](https://www.getdbt.com), [Prefect](https://prefect.io), [Dagster](https://github.com/dagster-io/dagster), [Kendro](https://github.com/quantumblacklabs/kedro) etc.

## Getting started
The key feature of Great Expectaions is the [Expectations](https://docs.greatexpectations.io/docs/#expectations). Expectations are assertions about your data. In Great Expectations, expectations are nothing but Python methods. For example, in order to assert that you want the column "id" to be unique, you can say:

```python
expect_column_value_to_be_unique(
    column="id"
)
```
Great Expectations provides such kind of expectations out-of-the-box. If you don't find the expectation you want, you can always write your own expectation. Writing [custom expectations](https://docs.greatexpectations.io/docs/guides/expectations/creating_custom_expectations/overview) needs a separate post and hence not in the scope of this article.

{{< adsense >}}

## How Great Expectations work
Let's discuss at a high level how this framework works. Once you install the Great expectations pip package you need to setup the following:
* **Data context** - Data context defines all the configurations required for Great Expectations to work. The two important things you define in data context are data sources and stores.
    1. Great Expectations stores your expectations, checkpoints and results in a store. The store can be your file system, an AWS S3 bucket, an Azure blob or Google cloud storage. In this article we will use file system as backendstore.
    2. You have to define which data source you want to apply your expectations. Great Expectations support Pandas dataframe, Spark dataframe and database tables with SQLAlchemy support. In this article we will use Pandas dataframe.
* **Expectations suite** - An expectation suite contains a set of expectations you want to run on a dataset. You give an expectations suite a valid name so that you can identify what expectations are added to this suite.
* **Expectation** - Expectation is an assertion or rule that you want to apply on a specific column in your dataset. For example, checking whether the value in the column is in the set of values provided.
* **Checkpoints** - A checkpoint is a feature in the Great Expectations library that allows you to save the state of your data validation so that you can quickly resume at that point in the future. A checkpoint also allows you to compare data validation results over time. You can define custom checkpoints with custom actions (like storing the failed records in an S3 bucket).
* **Data docs** - The Great Expectations Python library provides a variety of data docs to help users understand their data and the data expectations they have set. These data docs include: 
    1. Data Documentation: This provides an overview of the data, including information about the data sources, data types, data quality, and more. 
    2. Data Profiling: This provides an in-depth view of the data, including information about the distribution, outliers, missing values, and more. 
    3. Data Expectations Reports: This provides an overview of the data expectations that have been set, the results of tests against the expectations, and any issues that have been identified.
    4. Data Validation Reports: This provides a detailed view into the data validation results, including information about any failed tests and any data issues that have been identified.
I prefer to store validation results (a JSON object) into a database for further analysis. The complete source code for this article is available in [GitHub](https://github.com/saisyam/great-expectations-sample).

## Installing Great Expectations
I always prefer to setup a virtual environment for my Python projects. I assume you also do the same. Inside your virutal environment, you can install Great Expectations with pip:

```shell
$ pip install great_expectations
```
{{< adsense >}}

Use Python 3.8 and above. Detailed installation steps can be found [here](https://docs.greatexpectations.io/docs/guides/setup/installation/local).

## Setting up the Data context
A typical data context config for filesystem looks like this:

```python
STORE_FOLDER = "/Users/saisyam/work/github/great-expectations-sample/ge_data"
#Setup data config
data_context_config = DataContextConfig(
    config_version = 3,
    plugins_directory = None,
    config_variables_file_path = None,
    datasources = {},
    stores = {
        "expectations_store": {
            "class_name": "ExpectationsStore",
            "store_backend": {
                "class_name": "TupleFilesystemStoreBackend",
                "base_directory": STORE_FOLDER+"/expectations"
            }
        },
        "validations_store": {
            "class_name": "ValidationsStore",
            "store_backend": {
                "class_name": "TupleFilesystemStoreBackend",
                "base_directory": STORE_FOLDER+"/validations"
            }
        },
        "checkpoint_store": {
            "class_name": "CheckpointStore",
            "store_backend": {
                "class_name": "TupleFilesystemStoreBackend",
                "base_directory": STORE_FOLDER+"/checkpoints"
            }
        },
        "evaluation_parameter_store": {"class_name":"EvaluationParameterStore"}
    },
    expectations_store_name = "expectations_store",
    validations_store_name = "validations_store",
    evaluation_parameter_store_name = "evaluation_parameter_store",
    checkpoint_store_name = "checkpoint_store",
    data_docs_sites={
        "local_site": {
            "class_name": "SiteBuilder",
            "store_backend": {
                "class_name": "TupleFilesystemStoreBackend",
                "base_directory": STORE_FOLDER+"/data_docs",
                
            },
            "site_index_builder": {
                "class_name": "DefaultSiteIndexBuilder",
                "show_cta_footer": True,
            },
        }
    },
    anonymous_usage_statistics={
      "enabled": True
    }
)
```
Once we have the config ready, we can set the context with:

```python
context = BaseDataContext(project_config = data_context_config)
```

{{< adsense >}}

## Setting up data source
We will setup a Pandas data source and add it to the context
```python
datasource_config = {
    "name": "sales_datasource",
    "class_name": "Datasource",
    "module_name": "great_expectations.datasource",
    "execution_engine": {
        "module_name": "great_expectations.execution_engine",
        "class_name": "PandasExecutionEngine",
    },
    "data_connectors": {
        "default_runtime_data_connector_name": {
            "class_name": "RuntimeDataConnector",
            "module_name": "great_expectations.datasource.data_connector",
            "batch_identifiers": ["default_identifier_name"],
        },
    },
}
context.add_datasource(**datasource_config)
```

## Creating Expectation suite and add Expectations to it
Now it's time to create an Expectation suite and add expectations to the suite:
```python
# Create expectations suite and add expectations
suite = context.create_expectation_suite(expectation_suite_name="sales_suite", overwrite_existing=True)

# Add expectations
expectation_config_1 = ExpectationConfiguration(
    expectation_type="expect_column_values_to_be_in_set",
    kwargs={
        "column": "product_group",
        "value_set": ["PG1", "PG2", "PG3", "PG4", "PG5"]
    }
) 
suite.add_expectation(expectation_configuration=expectation_config_1)

expectation_config_2 = ExpectationConfiguration(
    expectation_type="expect_column_values_to_be_unique",
    kwargs={
        "column": "id"
    }
) 
suite.add_expectation(expectation_configuration=expectation_config_2)
context.save_expectation_suite(suite, "sales_suite")
```
Now we have everything ready to run the expectations on dataset.

## Create dataset and run checkpoint
We will use a [sample sales dataset](https://github.com/saisyam/great-expectations-sample/blob/master/sales.csv). Create a batch request and run the checkpoint. We will use all the default parameters from Great Expectations.

```python
# load and validate data
df = pd.read_csv("./sales.csv")

batch_request = RuntimeBatchRequest(
    datasource_name="sales_datasource",
    data_connector_name="default_runtime_data_connector_name",
    data_asset_name="product_sales",
    runtime_parameters={"batch_data":df},
    batch_identifiers={"default_identifier_name":"default_identifier"}
)

checkpoint_config = {
    "name": "product_sales_checkpoint",
    "config_version": 1,
    "class_name":"SimpleCheckpoint",
    "expectation_suite_name": "sales_suite"
}
context.add_checkpoint(**checkpoint_config)
results = context.run_checkpoint(
    checkpoint_name="product_sales_checkpoint",
    validations=[
        {"batch_request": batch_request}
    ]
)
print(results)
```
The above code will print the validation results as a Python dict. As we have enabled data docs in our config, you will see a `data_docs` folder under `STORE_FOLDER` where the HTML output is stored. Each run will create a new HTML file under `validations` folder inside `data_docs`. The sample HTML file will look like:
![Great Expectation validation results](/great_expectations_html_output.jpg "Great Expectation validation results")
{.img-fluid}

{{< adsense >}}

## Conclusion
We have learnt how to setup and run Great Expectations on a dataset. Great Expectations provides an excellent documentation on how to use different stores (AWS S3, Azure Blob and Google storage) to store expectations and results. The sample code is available in GitHub and you can extend it with different stores. Thanks for reading.