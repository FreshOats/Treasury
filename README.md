# Treasury
Analysis on a synthetic dataset for Treasury Analytics. 

In order to perform an analysis from multiple datasets, including validation across datasets, cleaning, and creating of dashboards, we need to have some data. Finding data is often challenging, and sometimes takes longer to find something reasonable to use than it's worth. This repo looks at the programmatic creation of synthetic data, using Faker to create real-sounding businesses, names, and dates along with the programmatic creation and destruction of high fidelity datasets. 

Why destroy such masterpieces? Because when designing an ETL or ELT pipeline that extracts from the less-than-ideal sources, it's important to expect errors, duplicates, and inconsistencies. Without them, simple copy/paste queries can provide all of the answers. But that is unrealistic. 

In an ideal world, all data would be served on a digital platter like a delta table, or as I like to refer to those as Parquet on ACID. But we are rarely that lucky. No, we get a motley of unstructured csv garbage and Excel files. It is what it is. So that's what the first part of this project is: Creating usable garbage data. 

To do this, we basically need to work backwards through the data cleaning process, to ensure we don't break the tables before we can save them. I used different sampling methods to interpolate different types of errors, inconsistencies, and outliers to the datasets, and since this was done programmatically, I am somewhat naive to what I am going to find in each table, since the processes were not the same in each. 

The ultimate intention of the creation and analysis of these data is to create a new tutorial on producing crappy financial data that would be assessed by a data analyst at a state treasury. It's going to be a blast! 

While Static files are wonderful, realistically, we should expect new files to be coming in on a regular basis. Phase 2 of this project, after establishing the original synthetic data and the ELT and export to dashboards will be creating an automated system that continues to produce new data so that an analytics pipeline can be established such that it is flexible in the processing of new, unknown data. 


---
## Generating Synthetic Data
The most challenging part in synthesizing data is the number of dependencies different tables have on one another, and addressing such programmatically to ensure that future querying can actually yield meaningful results. For example, it's very easy to used Faker() to generate client names and transaction dates, but we can't just use the same programming logic in multiple tables if the clients in one table represent a subset from a different table. Additionally, when we're considering account dates, if an account is closed, there cannot be transactions after the that date. 

The intended data for this analytics pipeline is to investigate tables expected from a state treasury. The tables created were as follows: 
1. Transactions - Financial transactions, including an account id, dates, types, vendors, and expense or revenue descriptions. 
2. Investment portfolio holdings - id, quantity held, market price per unit, market values, security name, security type, and acquisition date
3. External vendor information - id, vendor name, service type, contract start and end dates, and amount paid YTD
4. Unclaimed property records - id, owner types and names, property type, reported date, property value, and claim status
5. Program performance metrics - id, program name, reporting period (Date format by quarter), participant, successful outcomes, and program cost 

The fact tables are the transactions, unclaimed property records, and investment portfolio holdings. As fact tables, most of the data within these are unique with respect to each other, but each of these fact tables do overlap within the dimension tables that hold the client/account information and the ranges of dates.

Additional considerations - every transaction should not be with a distinct clients. There should be a limited set of clients, some of whom are making multiple transactions. This same subset of clients will contain portfolio holders. With the unclaimed properties, there will be both individuals and companies that have unclaimed property, and these will often be clients or former clients. The performance metrics should include cost aggregation from the transactions, so this table can't be created until we have the data from transactions. 

**This all being said, the result of this is 'perfect' data, which is unlikely in reality.**

The next step in each of the above tables was the programmatic destrcution of the high-fidelity data, by interpolating common errors and issues that arise in data analysis. Some of the most common issues data analysts encounter are duplication, nulls, incorrect data types, logical inconsistenties, outliers, inconsistent naming conventions, and other fun stuff that create gray hairs. 

1. Duplicates: This was done using random sampling set to a percentage, usually around 2% to pull a sample from the original data and then concatenate to the table. 
2. Type Errors: Using random sampling, I created a list of indices and changed the data type in Float columns to String. 
3. Accounting Inconsistencies: Here I sampled expenses and multiplied that sample by -1, an occasional accounting error where the expense is now a double negative. 
4. Naming Issues: Adding "InvalidDepartment" as an actual department name to a sample of the set.
5. Introducing Outliers: This was done a few different ways. Some of them used a lognormal distribution to create the values for numerical datasets. In others I used a binary multiplier that either increased the value or decreased the value by a factor of 100.  
6. Introducing Future Date Errors: This code added random dates after the today-date when the dataset was created. 
7. Naming Inconsistencies: Some of the fake companies will have both Inc and Incorporated, even though it is the same company. 
8. Adding Null Values
9. Misclassifying values: assigning a percentage of the data to the wrong option in a limited set. 
10. Errors where property is claimed by a null owner
11. Logical errors: there are more successful outcomes than total participants
12. Logical numerical errors:  budget utilization is greater than 100% or below 0%
13. Completion errors: On time completion is yes, but there is no user
14. Inconsistency: Capitalization inconsistencies

Finally, after each dataset was mangled, the data were shuffled so that all of the bad data weren't just appended at the end of the tables. 

---
## ETL -> ELT
In order to in

