---
title: "Data Testing with Great Expectations: An Introduction to In-Memory Context"
categories:
  - Data Analysis
tags:
  - Python
  - Great Expectations
date: 2023-05-20
image: /ge_in_memory.jpg
featured: 1
---
In the world of data science and data engineering, ensuring the quality and integrity of your data is crucial. One tool that can help with this is Great Expectations, a Python library that allows you to test your data against a set of "expectations". In this blog post, we'll explore a particular feature of Great Expectations: the in-memory context.

{{< table_of_contents >}}

{{< adsense >}}

## What is Great Expectations?
Great Expectations is a Python library that helps data teams eliminate pipeline debt. It does this by enabling automated testing of data quality and documentation of data. With Great Expectations, you can express what you "expect" from your data as simple, human-readable assertions. [Here](https://saisyam.com/unlock-quality-insights-with-great-expectations-python-library/) is my blog about Great Expectations in detail.

```python
import great_expectations as ge

# Load some data
df = ge.read_csv("data.csv")

# Set an expectation
df.expect_column_values_to_be_unique("id")
```
## The In-Memory Context
One of the features of Great Expectations is the DataContext. A DataContext represents a Great Expectations project and includes many features for configuring and managing your data expectations.

In version 0.13.8, Great Expectations introduced the InMemoryDataContext. This allows you to create a DataContext that doesn't require a filesystem. This can be particularly useful for testing or for situations where you don't want to or can't write to disk.
You can create a in memory data context using the build-in method from Great Expectations.

```python
from great_expectations.util import *
context = build_in_memory_runtime_context()
```
The full working code, with `datasource config`, `expectation suite`, `batch request` and `checkpoints`:

```python
import great_expectations as ge
from great_expectations.util import *
from great_expectations.core.batch import RuntimeBatchRequest
from great_expectations.core.expectation_configuration import ExpectationConfiguration

# Load some data
df = ge.read_csv("data.csv")
context = build_in_memory_runtime_context()

datasource_config = {
    "name": "orders_datasource",
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

# Create expectations suite and add expectations
suite = context.create_expectation_suite(expectation_suite_name="my_suite", overwrite_existing=True)

expectation_config = ExpectationConfiguration(
    expectation_type="expect_column_values_to_be_unique",
    kwargs={
        "column": "ORDERNUMBER"
    }
) 
suite.add_expectation(expectation_configuration=expectation_config)
context.save_expectation_suite(suite, "my_suite")

batch_request = RuntimeBatchRequest(
    datasource_name="orders_datasource",
    data_connector_name="default_runtime_data_connector_name",
    data_asset_name="orders",
    runtime_parameters={"batch_data":df},
    batch_identifiers={"default_identifier_name":"default_identifier"}
)

checkpoint_config = {
    "name": "orders_checkpoint",
    "config_version": 1,
    "class_name":"SimpleCheckpoint",
    "expectation_suite_name": "my_suite"
}
context.add_checkpoint(**checkpoint_config)
results = context.run_checkpoint(
    checkpoint_name="orders_checkpoint",
    validations=[
        {"batch_request": batch_request}
    ]
)
print(results)
```
{{< adsense >}}

## Conclusion
Great Expectations is a powerful tool for ensuring data quality, and the in-memory context feature provides even more flexibility for data testing. Whether you're working in a robust data engineering environment or just doing some quick data quality checks, Great Expectations and its in-memory context can be a valuable addition to your data toolkit.