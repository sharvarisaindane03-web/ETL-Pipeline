# MySQL ETL Pipeline with Python

A beginner-friendly ETL workflow that uses Python and MySQL to extract employee data, transform it with SQL, and load the results into separate analysis and backup tables.

## Overview

The pipeline connects to a MySQL database with `mysql-connector-python`, creates an `Employee` table, and runs SQL queries to create 12 derived tables. The examples demonstrate common ways to filter, select, sort, group, and categorize data.

### ETL workflow

1. **Extract** employee records from the MySQL `Employee` table.
2. **Transform** the data using SQL operations such as `WHERE`, `DISTINCT`, `ORDER BY`, `GROUP BY`, aggregates, `HAVING`, `BETWEEN`, `IN`, `LIKE`, and `CASE`.
3. **Load** each query result into its own MySQL table.

## Tables created

| Table | Example operation |
|---|---|
| `Backup_All` | Copy all employee rows |
| `Backup_High_Salary` | Filter employees with salary above 50,000 |
| `Backup_Selected_Columns` | Keep selected employee columns |
| `Backup_Departments` | Select distinct departments |
| `Backup_Salary_Sorted` | Sort employees by salary descending |
| `Backup_Department_Count` | Count employees by department |
| `Backup_Average_Salary` | Calculate average salary by department |
| `Backup_High_Average_Salary` | Keep departments with average salary above 50,000 |
| `Backup_Salary_Range` | Filter salaries between 40,000 and 70,000 |
| `Backup_Selected_Departments` | Select employees in IT or HR |
| `Backup_Name_Search` | Find names matching a pattern (for example, starting with A) |
| `Backup_Salary_Category` | Assign salary categories with `CASE` |

> The salary thresholds and example department/name filters can be changed to match your data.

## Screenshots

### ETL workflow

![Python and MySQL ETL workflow](assets/etl-workflow.png)

### MySQL Workbench results

![MySQL Workbench showing the database tables and results](assets/mysql-workbench.png)

Add the screenshots to the repository at `assets/etl-workflow.png` and `assets/mysql-workbench.png` (or update the links above to match your filenames). The workflow image should show the Python/SQL process; the Workbench image should show the created database tables or query results.

## Requirements

- Python 3
- MySQL Server
- MySQL Workbench (optional, for browsing tables and query results)
- Python package: `mysql-connector-python`

Install the connector:

```bash
python -m pip install mysql-connector-python
```

## Configure the database connection

Create a database for the project in MySQL, then configure the connection in the Python script. Avoid committing credentials to Git. One option is to read them from environment variables:

```python
import os
import mysql.connector

connection = mysql.connector.connect(
    host=os.getenv("MYSQL_HOST", "localhost"),
    user=os.getenv("MYSQL_USER"),
    password=os.getenv("MYSQL_PASSWORD"),
    database=os.getenv("MYSQL_DATABASE", "etl"),
)
```

Set `MYSQL_USER` and `MYSQL_PASSWORD` in your local environment before running the script. Make sure the configured database exists and that the account can create and write tables.

## Run

1. Start MySQL Server and create the target database.
2. Install the Python dependency.
3. Set the database connection environment variables.
4. Run the ETL script from the repository root:

   ```bash
   python etl_pipeline.py
   ```

5. Refresh the schema in MySQL Workbench and inspect the generated tables.

If your script has a different filename, replace `etl_pipeline.py` with its actual name.

## Notes

- The workflow uses `CREATE TABLE IF NOT EXISTS ... AS SELECT ...` to create output tables. If a table already exists, rerunning the query will not refresh its contents. Drop or truncate the output tables, or change the SQL, when you need to regenerate them.
- Commit database changes after successful table creation and close the cursor and connection when finished.
- Use a dedicated MySQL account with only the permissions the project requires.

## License

Add a license here if you intend to share or reuse this project publicly.
