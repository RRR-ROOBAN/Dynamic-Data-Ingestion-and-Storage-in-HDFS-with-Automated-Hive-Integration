# Dynamic-Data-Ingestion-and-Storage-in-HDFS-with-Automated-Hive-Integration

## Overview
This project implements both **manual** and **automated** data ingestion processes into **Hive**, utilizing **AWS EMR and EC2** for scalable data processing.

## Technologies Used
- **AWS EMR**: For running Hadoop, HDFS, and Hive
- **AWS EC2**: To host and manage the EMR cluster
- **HDFS**: For distributed data storage
- **Hive**: For structured data processing

## Manual Process
1. **Download Data**: Manually download the dataset from a given URL.
2. **Move Data to HDFS**: Upload the file to HDFS using `hadoop fs -put`.
3. **Create Hive Database and Table**: Manually create the Hive table.
4. **Load Data into Hive**: Use the `LOAD DATA INPATH` command to load the file into Hive.
5. **Verify Data**: Run a `SELECT COUNT(*)` query to check data integrity.

## Automated Process
1. **Download Data**: Fetches the dataset using `wget`.
2. **Move Data to HDFS**: Transfers the file to HDFS automatically.
3. **Create Hive Database and Table**: Ensures the database and table exist before loading data.
4. **Load Data into Hive**: Loads the data into a Hive table.
5. **Verify Data**: Runs a query to count the number of records in the table.

## Prerequisites
Ensure the following are installed and configured:
- AWS EMR with Hadoop, Hive, and HDFS
- AWS EC2 instance with access to EMR
- Bash Shell
- wget (for downloading data)

## Folder Structure
```
project-folder/
│-- automate.sh
│-- create_hive_table.hql
│-- manual_steps.txt
│-- README.md
```

## Manual Steps (`manual_steps.txt`)
```sh
# Step 1: Download Data Manually
wget -O project_pe-03.csv "http://your-data-source-url"

# Step 2: Move file to HDFS
hadoop fs -put project_pe-03.csv /user/

# Step 3: Create Hive Database and Table
hive -f create_hive_table.hql

# Step 4: Load Data into Hive
hive -e "
    LOAD DATA INPATH '/user/project_pe-03.csv'
    INTO TABLE project.population_estimates;
"

# Step 5: Verify Data
hive -e "SELECT COUNT(*) FROM project.population_estimates;"
```

## Hive Table Schema (`create_hive_table.hql`)
```sql
CREATE DATABASE IF NOT EXISTS project;

USE project;

CREATE TABLE IF NOT EXISTS population_estimates (
    year_of_estimate INT,
    fips_code STRING,
    race_sex STRING,
    under_5 INT,
    age_5_9 INT,
    age_10_14 INT,
    age_15_19 INT,
    age_20_24 INT,
    age_25_29 INT,
    age_30_34 INT,
    age_35_39 INT,
    age_40_44 INT,
    age_45_49 INT,
    age_50_54 INT,
    age_55_59 INT,
    age_60_64 INT,
    age_65_69 INT,
    age_70_74 INT,
    age_75_79 INT,
    age_80_84 INT,
    age_85_above INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','  
LINES TERMINATED BY '\n'  
STORED AS TEXTFILE;
```

## Automated Script (`automate.sh`)
```sh
#!/bin/bash

# Step 1: Download Data
echo "Downloading data..."
wget -O project_pe-03.csv "http://your-data-source-url"

# Step 2: Move the file to HDFS
echo "Moving file to HDFS..."
hadoop fs -put -f project_pe-03.csv /user/

# Step 3: Create Hive Database and Table
echo "Creating Hive table if not exists..."
hive -f create_hive_table.hql

# Step 4: Load data into Hive table
echo "Loading data into Hive..."
hive -e "
    LOAD DATA INPATH '/user/project_pe-03.csv'
    INTO TABLE project.population_estimates;
"

# Step 5: Verify data count
echo "Verifying data in Hive..."
hive -e "
    SELECT COUNT(*) FROM project.population_estimates;
"

echo "Process completed successfully!"
```

## Installation & Execution

### Manual Execution
1. Follow the steps in `manual_steps.txt`.
2. Run each command manually in the terminal.

### Automated Execution
1. Ensure EMR and EC2 are running.
2. Execute `bash automate.sh`.

## Expected Output
After execution, the script should display messages confirming:
- Successful data download
- Successful file transfer to HDFS
- Hive table creation (if not exists)
- Successful data loading into Hive
- Row count from Hive table




