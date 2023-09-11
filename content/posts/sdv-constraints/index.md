+++
title = 'The Significance of Constraints: Synthetic Data Vault'
date = 2023-09-07T17:03:22+05:30
draft = false
+++

Synthetic Data Vault (SDV) is an open-source Python library that allows users to easily model and sample synthetic data from multiple sources of real data. SDV can handle different types of data, such as tabular, relational, time series, and multi-modal. SDV uses deep learning models to learn the structure and dependencies of the real data and generate realistic synthetic data that can be used for various applications.

**_Please note that this article was written with the assistance of GPT. While I did provide all of the points that I intended to convey, the article is not as tight knit and polished as I'd like. I do believe, however, that this is a somewhat appropriate usecase for GPT and that the major ideas have been imparted. I would essentially like to employ this article as a placeholder until I get the time to do said polishing._**

However, SDV may not be able to learn everything from the real data, especially when there are deterministic rules that must be followed by every row of the data. These are patterns and laws that are inherent to the dataset or the domain, and they are unchangeable, no matter what data you input. For example, in a hotel booking dataset, the searched check-in date must always be before the searched check-out date. If SDV does not learn these rules, it may generate synthetic data that violates them, which can reduce the quality and validity of the synthetic data.

To overcome this problem, SDV allows users to input constraints that specify the deterministic rules that the synthetic data must satisfy. Constraints are business logic that can improve the machine learning model and ensure that the synthetic data is consistent and realistic.

## Types of Constraints

SDV provides different types of constraints for different types of data and rules. Some of the available constraints are:

- **ColumnFormula**: This constraint applies a formula to one column based on the values of other columns. For example, this constraint can be used to calculate the total price of a hotel booking based on the number of nights and the price per night.
- **GreaterThan**: This constraint ensures that one column is always greater than another column. For example, this constraint can be used to ensure that the searched check-in date is always before the searched check-out date.
- **Unique**: This constraint ensures that one or more columns are unique within the table. For example, this constraint can be used to ensure that each hotel booking has a unique ID.
- **UniqueCombinations**: This constraint ensures that the combinations of values from two or more columns are unique within the table. For example, this constraint can be used to ensure that each hotel room has a unique combination of hotel ID and room number.
- **OneHotEncoding**: This constraint ensures that one or more columns follow a one-hot encoding scheme. For example, this constraint can be used to ensure that each hotel booking has only one value for the site name column.

SDV also allows users to define their own custom constraints by subclassing the `sdv.constraints.base.Constraint` class and implementing the required methods.

## How to Apply Constraints

To apply constraints using SDV, the following steps are required:

- **Import SDV**: Import the SDV library and the desired synthesizer class from the corresponding module. For example, to use the GaussianCopula synthesizer, the following code can be used:

```python
import sdv
from sdv.tabular import GaussianCopula
```

- **Load real data**: Load the real data that will be used to train the synthesizer. The real data can be loaded from various sources, such as CSV files, pandas dataframes, SQL databases, etc. For example, to load a CSV file containing tabular data, the following code can be used:

```python
import pandas as pd
real_data = pd.read_csv("real_data.csv")

```

- **Define constraints**: Define the constraints that specify the deterministic rules that the synthetic data must satisfy. The constraints can be defined using either a dictionary format or a custom class. For example, to define a GreaterThan constraint that ensures that  `srch_ci`  is always less than  `srch_co`, the following code can be used:

```python
from sdv.constraints import GreaterThan

constraint = GreaterThan(
    low='srch_ci',
    high='srch_co'
)

```

- **Create synthesizer instance**: Create an instance of the synthesizer class and pass any optional parameters that are needed to customize the synthetic data generation process. Use the  `constraints`  parameter to pass in a list of constraint objects. For example, to create a GaussianCopula instance with a specified random seed and a list of constraints, the following code can be used:

```python
synthesizer = GaussianCopula(
    primary_key='log_id',
    random_state=42,
    constraints=[constraint]
)

```

- **Fit synthesizer**: Fit the synthesizer to the real data using the  `fit`  method. This will train the synthesizer to learn the structure and dependencies of the real data and respect the constraints. For example, to fit the GaussianCopula instance to the real data, the following code can be used:

```python
synthesizer.fit(real_data)

```

- **Sample synthetic data**: Sample synthetic data from the synthesizer using the  `sample`  method. This will generate synthetic data that mimics the characteristics and patterns of the real data and satisfies the constraints. The  `sample`  method can take optional parameters that can control the size and conditions of the synthetic data. For example, to sample 100 rows of synthetic data from the GaussianCopula instance, the following code can be used:

```python
synthetic_data = synthesizer.sample(100)

```

- **Save or use synthetic data**: Save or use the synthetic data for the desired purpose. The synthetic data can be saved to various formats, such as CSV files, pandas dataframes, SQL databases, etc. The synthetic data can also be used for various applications, such as testing, training, privacy protection, data augmentation, etc. For example, to save the synthetic data to a CSV file, the following code can be used:

```python
synthetic_data.to_csv("synthetic_data.csv")

```

## Performance Implications of Applying Constraints

Applying constraints may have some impact on the performance of the synthetic data generation process. Depending on the type and number of constraints, the synthesizer may need more time and resources to learn and sample the synthetic data. For example, some constraints may require additional preprocessing or postprocessing steps, such as transforming or rejecting the data. Some constraints may also introduce additional hyperparameters that need to be tuned or optimized.

To measure the performance impact of applying constraints, users can use various metrics and tools, such as execution time, memory usage, CPU/GPU utilization, etc. Users can also compare the performance of different synthesizers and constraints to find the optimal combination for their specific use case.

## Quality and Size Ramifications of Constraints on Model

Applying constraints may also have some ramifications on the quality and size of the synthetic data and the model. Depending on the type and number of constraints, the synthesizer may generate synthetic data that is more or less realistic and diverse than the real data. For example, some constraints may reduce the variability or increase the bias of the synthetic data. Some constraints may also limit the size or scope of the synthetic data.

To evaluate the quality and size ramifications of applying constraints, users can use various metrics and tools, such as statistical tests, visualizations, similarity scores, validation methods, etc. Users can also compare the quality and size of different synthesizers and constraints to find the optimal combination for their specific use case.

## Conclusion

Constraints are a powerful and useful feature of SDV that allow users to input deterministic rules that the synthetic data must satisfy. Constraints can improve the machine learning model and ensure that the synthetic data is consistent and realistic. SDV provides different types of constraints for different types of data and rules. SDV also allows users to define their own custom constraints by subclassing the  `sdv.constraints.base.Constraint`  class. To apply constraints using SDV, users need to define the constraints, create a synthesizer instance with a list of constraint objects, fit the synthesizer to the real data, sample synthetic data from the synthesizer, and save or use the synthetic data for their desired purpose. Applying constraints may have some impact on the performance, quality, and size of the synthetic data generation process. Users can measure and evaluate these ramifications using various metrics and tools. Users can also compare different synthesizers and constraints to find the optimal combination for their specific use case. To learn more about SDV and how to use it with constraints, you can visit [the official documentation](https://sdv.dev/) or [the GitHub repository](https://github.com/sdv-dev/SDV).
