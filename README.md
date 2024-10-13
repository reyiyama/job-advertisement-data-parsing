# Job Ad Data Integration and Parsing Project

This repository contains a data parsing, cleansing, and integration project focused on job advertisement datasets from multiple sources. This project demonstrates a complete data preprocessing pipeline, including parsing XML data, auditing and cleansing data, resolving conflicts, and integrating data from different sources. The main goal of this project is to provide an efficient and replicable methodology for preparing job data for downstream analytical use.

## Project Overview

The project was structured into three main tasks:

### Task 1: Parsing Data

The initial dataset, an XML file (`<student_id>_dataset1.xml`), was parsed into a structured Pandas DataFrame. This included the following steps:

1. **Examine the XML Structure**: The structure and format of the provided XML dataset were carefully examined to understand the nesting and tags used for each attribute.
2. **Parse XML to DataFrame**: Using Python's `xml.etree.ElementTree` library, the XML data was parsed into a Pandas DataFrame. Each row represented a job advertisement record, with attributes such as `Id`, `Title`, `Location`, `Company`, `ContractType`, `ContractTime`, `Category`, `Salary`, `OpenDate`, `CloseDate`, and `SourceName`.
3. **Data Type Conversion**: The parsed data was converted into appropriate data types, ensuring consistency and compatibility for subsequent processing. For example, `OpenDate` and `CloseDate` fields were converted from string format (`YYYYMMDDThhmmss`) to datetime objects.

This step provided a structured format that facilitated further data analysis and cleaning.

### Task 2: Data Cleansing

Once the data was parsed, it underwent a comprehensive data cleansing process to ensure quality and consistency. The following steps were undertaken:

1. **Data Audit**: The dataset was audited to identify various data quality issues, including:
   - **Missing Values**: Missing values were replaced with placeholders such as `'non-specified'` for categorical fields (e.g., `Title`, `Location`, `Company`).
   - **Irregularities and Typos**: Spelling mistakes and irregularities in values were corrected. For example, typos in the `Location` field like `'Loden'` were corrected to `'London'`.
   - **Data Type Issues**: Ensured all columns had the correct data types as per the requirements. For instance, `Salary` was converted to a float with two decimal places.
   - **Outliers and Duplicates**: Outliers were analyzed, and duplicate job records were identified and removed where necessary.
2. **Error Logging**: A CSV file (`<student_id>_errorlist.csv`) was generated to document all identified data issues, including:
   - The index of the affected row.
   - The `Id` of the job advertisement with the issue.
   - The column(s) affected.
   - The original and modified values.
   - A description of the error type and the action taken to resolve it.
3. **Cleaned Dataset Output**: The cleaned dataset was exported as `s3970066_dataset1_solution.csv`, ready for data integration.

This thorough cleansing process ensured the dataset was free from errors and inconsistencies, making it suitable for further analysis.

### Task 3: Data Integration

In the final task, the cleaned dataset (`s3970066_dataset1_solution.csv`) was integrated with an additional CSV dataset (`<student_id>_dataset2.csv`). The integration process involved:

1. **Schema Resolution**: The schemas of both datasets were compared to identify conflicts:
   - **Missing Columns**: The second dataset did not contain the `Id` column. A unique identifier was generated for each record in `<student_id>_dataset2.csv` to maintain consistency.
   - **Column Name Differences**: Column names were standardized to match the schema defined in Task 1 and 2. This ensured uniformity in the final dataset.
2. **Data Merging**: The datasets were merged using Pandas, adopting a consistent schema. Attributes such as `Title`, `Location`, `Company`, etc., were aligned to create a unified dataset containing job advertisements from multiple sources.
3. **Data-Level Conflict Resolution**: Data-level conflicts, such as discrepancies in job titles or salaries for the same job `Id`, were resolved. A global/unique key for the integrated dataset was chosen to ensure each job advertisement was uniquely identifiable. Duplicate records were removed, and any conflicts in the data values were handled based on business rules, such as preferring non-null values over nulls.
4. **Integrated Dataset Output**: The final integrated dataset was saved as `s3970066_dataset_integrated.csv`. This dataset provided a richer and more complete view of job advertisements, integrating data from multiple sources for enhanced insights.

## Importance for Industry

Effective parsing, cleansing, and integration of job advertisement data is crucial for companies managing job hunting platforms. Clean and well-structured data ensures:

- **Enhanced User Experience**: Better data quality helps job seekers find relevant opportunities faster.
- **Improved Analytics**: Integration of multiple data sources allows for richer insights and improved user targeting, driving platform engagement.
- **Operational Efficiency**: Automating the data cleaning and integration process reduces manual intervention and speeds up updates.

## Reproducing Results

To reproduce the results of this project, follow these steps:

1. **Clone the Repository**

   ```sh
   git clone https://github.com/reyiyama/job-advertisement-data-parsing.git
   cd job-advertisement-data-parsing
   ```

2. **Install Requirements**
   The required packages are listed in the `requirements.txt` file. Install them using pip:

   ```sh
   pip install -r requirements.txt
   ```

3. **Run the Jupyter Notebooks**

   - Open the `s3970066_task1_2.ipynb` notebook to run the data parsing and cleansing tasks.
   - Open the `s3970066_task3.ipynb` notebook to execute the data integration task.

4. **Input Data**
   Ensure the original XML (`<student_id>_dataset1.xml`) and CSV files (`<student_id>_dataset2.csv`) are in the working directory. These files are necessary for parsing and integration.

5. **Outputs**

   - **Cleaned Dataset**: `s3970066_dataset1_solution.csv`
   - **Error List**: `s3970066_errorlist.csv`
   - **Integrated Dataset**: `s3970066_dataset_integrated.csv`

## Repository Structure

```
job-advertisement-data-parsing/
|— csv_just_in_case_it_gets_lost/          # Backup of the original CSVs
|— s3970066_task1_2.ipynb                # Jupyter Notebook for Task 1 and 2
|— s3970066_task3.ipynb                  # Jupyter Notebook for Task 3
|— requirements.txt                      # List of required Python libraries
|— unified_dataset.csv                   # Final integrated dataset
|— README.md                             # Project overview and usage instructions
```

## License

This project is licensed under the MIT License. See the LICENSE file for details.
