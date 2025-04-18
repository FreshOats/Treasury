# Treasury
Analysis on a synthetic dataset for Treasury Analytics. 

In order to perform an analysis from multiple datasets, including validation across datasets, cleaning, and creating of dashboards, we need to have some data. Finding data is often challenging, and sometimes takes longer to find something reasonable to use than it's worth. This repo looks at the programmatic creation of synthetic data, using Faker to create real-sounding businesses, names, and dates along with the programmatic creation and destruction of high fidelity datasets. 

Why destroy such masterpieces? Because when designing an ETL or ELT pipeline that extracts from the less-than-ideal sources, it's important to expect errors, duplicates, and inconsistencies. Without them, simple copy/paste queries can provide all of the answers. But that is unrealistic. 

In an ideal world, all data would be served on a digital platter like a delta table, or as I like to refer to those as Parquet on ACID. But we are rarely that lucky. No, we get a motley of unstructured csv garbage and Excel files. It is what it is. So that's what the first part of this project is: Creating usable garbage data. 

To do this, we basically need to work backwards through the data cleaning process, to ensure we don't break the tables before we can save them. I used different sampling methods to interpolate different types of errors, inconsistencies, and outliers to the datasets, and since this was done programmatically, I am somewhat naive to what I am going to find in each table, since the processes were not the same in each. 

The ultimate intention of the creation and analysis of these data is to create a new tutorial on producing crappy financial data that would be assessed by a data analyst at a state treasury. It's going to be a blast! 

While Static files are wonderful, realistically, we should expect new files to be coming in on a regular basis. Phase 2 of this project, after establishing the original synthetic data and the ELT and export to dashboards will be creating an automated system that continues to produce new data so that an analytics pipeline can be established such that it is flexible in the processing of new, unknown data. 


---
I am planning on using Snowflake for the querying and analysis. 