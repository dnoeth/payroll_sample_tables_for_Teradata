# payroll_sample_tables_for_Teradata

Real world payroll data based on two Public Domain datasets published by the city of [Baton Rouge, Louisiana](https://www.brla.gov/).

[City-Parish Employees](https://data.brla.gov/Government/City-Parish-Employees/gyhq-w3h3): 
"City-Parish employees, both active and inactive, that exist in the City-Parish Payroll System."

[City-Parish Employee Annual Salaries](https://data.brla.gov/Government/City-Parish-Employee-Annual-Salaries/g5c2-myyj): 
"City-Parish employees' annual salaries and other payroll related information. Information is calculated after the last payroll is run for the year specified."

## Description

The data was downloaded, cleaned and split into multiple normalized tables in January 2019.  All files contain tab-delimited data:

    br_departments.tsv, 53 rows: Departments within City-Parish government (PK: department_number)
    br_divisions.tsv, 271 rows: Divisions within City-Parish government (PK: division_number)
    br_jobs.tsv, 1226 rows: Jobs within City-Parish government, both active and inactive (PK: job_code)
    br_pay_plan.tsv, 2479 rows: A pay plan defines the salary ranges for each job class (PK: pay_grade, pay_step)
    br_payroll.tsv, 24740 rows: City-Parish employees, both active and inactive, that exist in the City-Parish Payroll System (PK: employee_number)
    br_salary_hist.tsv, 54338 rows: City-Parish employees' annual salaries and other payroll related information (years 2008 to 2017)
 
## Prerequisites

 1. `BTEQ` must be installed on your client.

 2. The load user needs the `CREATE TABLE` right in the target database.

### Installing using a Windows client

 1. Download [payroll_db_Teradata.zip](https://github.com/dnoeth/payroll_sample_tables_for_Teradata/blob/master/payroll_db_Teradata.zip) and extract the zip file to a folder, e.g. `C:\Samples\payroll_db_Teradata`.

 2. Modify the file `br_install.btq` to match your target system. Optionally modify the database name.
 
 3. Open a *cmd* (or *PowerShell*) window, change to the folder and run the file using BTEQ:

```
cd C:\Samples\payroll_db_Teradata
bteq < br_install.btq > br_install.log
```

### Installing using a Linux client

 1. Download [payroll_db_Teradata.zip](https://github.com/dnoeth/payroll_sample_tables_for_Teradata/blob/master/payroll_db_Teradata.zip) and extract the zip file to a folder, e.g. `~/Samples/payroll_db_Teradata`.

 2. Modify the file `br_install.btq` to match your target system. Optionally modify the database name.
 
 3. Open a *terminal* window, change to the folder and run the file using BTEQ:
 
```
cd ~/Samples/payroll_db_Teradata
bteq < br_install.btq > br_install.log
```

#### Checking for successful installation

The script should finish in a few minutes.

If there's an error message returned check the `br_install.log` for the failing step.

### Reinstalling

Before re-running the install script the tables must be dropped in a specific order due to the FK constraints

```
    ALTER TABLE br_departments
    DROP FOREIGN KEY (manager_employee_number) REFERENCES WITH NO CHECK OPTION br_payroll (employee_number); 
    DROP TABLE br_payroll;
    DROP TABLE br_salary_hist;
    DROP TABLE br_pay_plan;
    DROP TABLE br_divisions;
    DROP TABLE br_departments;
    DROP TABLE br_jobs;
    DROP VIEW br_active_employees;
    DROP VIEW br_active_jobs;

```
