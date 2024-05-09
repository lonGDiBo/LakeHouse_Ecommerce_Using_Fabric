## DataLake_Ecommerce using Fabric

## Scope
This project is intended to provide experience with data engineering tasks using Fabric Spark and/or Data Pipelines to build out Delta Parquet tables and then use the Direct Lake connector in Power BI to query large volumes of real data. The medallion lakehouse architecture is followed in this sample where raw excel files are loaded to Bronze Layer, then Silver Layer flat table built using Delta Parquet format and lastly Gold Layer tables serve up the star schema model for a Direct Lake Power BI dataset.
![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/5d6fa510-3acb-4456-b19d-c476a2927a36)

## Step 1: Create Lakehouse, upload data to Lakehouse and create Delta Parquet Table using Fabric Spark
#### In this step we will upload raw files
1. Open up your Fabric Workspace and switch to Data Engineering persona using the menu on bottom left corne
2. Create a new Lakehouse or use an existing one
![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/0af9f5d4-d313-4be0-a2e9-6c8dd263238d)
3. Download Load Sale Data Spark Notebook from local to your local machine
4. In the Files directory, create two additional directories: "Current" to store newly uploaded files, and "Archive" to store successfully loaded historical files into the Lakehouse.

![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/436ee0f1-23c0-480a-9dcb-5e9c04cbe13d)

6. Import the downloaded Notebook into Current
7. Open the Notebook once the import is successful, you might need to update the Lakehouse association of the Notebook
8. Then upload data files to Lakeshouse and then run Spark Notebook to create single flat Delta Parquet table  (Bronze_Sales) with some minor modifications like data type changes, new columns based on existing columns, etc. Detailed steps to create a table view in notebook: . It is partitioned by Order_Year and Order_Month which helps optimize query performance and easily manage data over time.
    
## Step 2: Create Gold Tables (Star Schema) to be used for Reporting
### Using Spark Notebook
#### Create dimension tables and truth tables stored as deltas for optimal ACID (Atomicity, Consistency, Isolation, Durability) support and more efficient queries.
1. Create Gold_DimCustomer: Contains information about the customer, including ID, customer name, segment, city, state, country, region, creation time, and last modified time.
   - Download Gold_Customer Spark Notebook from Github Repo to your local machine
   - Import the downloaded Notebook into Fabric Workspace
   - Open the Notebook once the import is successful, you might need to update the Lakehouse association of the Notebook
   - Run the Notebook to create Gold_DimCustomer
     ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/44895027-1a21-4ea3-80cb-26b210fd8204)

2. Create Gold_DimProduct: Contains information about the product, including the product ID, product category, product name, and information about when the product was created and last modified.
   - Download Gold_DimProduct Spark Notebook from Github Repo to your local machine
   - Creating Gold_DimProduct is similar to creating Gold_DimCustomer.
   - In Gold_DimProduct, additional surrogate keys are created for faster management and queries
    ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/76ff10f6-b91c-4c72-a699-5a62e82962f0)

3. Create Gold_DimDate: Contains date, month, and year information
   - Download Gold_DimDate Spark Notebook from Github Repo to your local machine
   - Creating Gold_DimDate is similar to creating Gold_DimCustomer.
   - Create a dim_date table with corresponding columns for day, month, quarter, year, week, and other attributes containing data for a 15-year period from 2015 to 2030
     ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/264f9a6c-bf28-4d66-b6a8-648397a06c1e)

4. Create Gold_DimOrderPriority: contains the priority order of the order
   - Download Gold_DimOrderPriority Spark Notebook from Github Repo to your local machine
   - Creating Gold_DimOrderPriority is similar to creating Gold_DimCustomer.
   - In Gold_DimOrderPriority, additional surrogate keys are created for faster management and queries
     ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/24c978ba-b072-47b7-8bad-6c9369e95abb)

5. Create Gold_DimOrderReturn: Contains orders that have been returned
   - Download Gold_DimOrderReturn Spark Notebook from Github Repo to your local machine
   - Creating Gold_DimOrderReturn is similar to creating Gold_DimCustomer.
   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/05c36493-42a8-40ea-8ef7-bab9590a6778)

6. Create Gold_DimShipMode: Contains information about the shipping method
   - Download Gold_DimShipMode Spark Notebook from Github Repo to your local machine
   - Creating Gold_DimShipMode is similar to creating Gold_DimCustomer.
   - In Gold_DimShipMode, additional surrogate keys are created for faster management and queries
     ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/a9176ccd-2a80-4e6c-841d-73e296ad4d91)

7. Create Gold_Fact_Sale: Contains information about sales transactions, including order ID, price, quantity, sales volume, discounts, profit margins, shipping costs, etc. It is partitioned by Order_Year and Order_Month which helps optimize query performance and easily manage data over time.

   - After successfully loading data into the dimension table, we will load data into the Fact table
   - Download Gold_Fact_Sale Spark Notebook from Github Repo to your local machine
   - Creating Gold_Fact_Sale is similar to creating Gold_DimCustomer.
     ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/66fdd4b4-ad8e-4b7b-b665-ed4b8414d1af)

8. Create Note Book Current to Archive: Push successfully loaded dataset from Current to Archive for storage
    ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/7061ff68-ad76-4582-bc4b-f957aff0e2cd)

9. Create Note Book Run_Load: This helps automate the processing process and saves time and effort compared to running each file individually.

   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/1a7d0a4e-e1d1-4878-b9a6-0c1333419213)

11. Create a pipeline to automatically run data processing processes.
    - From the Workspace select +New > Show all > Data pipeline
    - Name the pipeline and select Add > NoteBook
    - Rename the NoteBook activity
    - Change the source to the Run_Load notebook
    ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/256837f9-265c-48eb-a9b4-4116affeac07)
    - On the Pipeline ribbon, click Run and the Pipeline will populate the Fabric Lakehouse with the dimensions and staging table for the CMS data. You do not need to schedule the Pipeline since it is a one-time load.
   
## Step 3: Create Semantic Model 
 - At the LakeHouse interface, select New Semantic Model > ceate name
 - Select or deselect objects for the semantic model. Only objects that can be added to the semantic model are shown
 - Here I did not choose Bronze_Sales

![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/548cd4d4-0d63-4c11-a262-6dce3ca1ab21)

 - Create relationships between the dimension tables and the fact table gold_fact_sale: Data Model should now look like this
   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/ac0dd44d-789f-4712-9865-bf54b497b881)

## Step 4: Create Reports using Power BI
 - You can create a new Power BI report in Fabric by clicking "New Report" in the Data Model view, clicking "Create Report" from the ellipse in the Workspace view data of Dataset or by connecting to Fabric Lakehouse using Power BI Desktop.
 - The dashboard provides an overview and analysis of sales data. The dashboard displays important information such as overall sales, sales by product category, sales by geographic region, and sales by time of day. The dashboard now looks like this:
   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/0a00c1d0-8f6a-4f49-b9da-fd63fe15f074)
   
## Step 5: Additionally, build an ARIMA model to predict sales
 - Use the data in the Bronze_Sale table to create the model
 - Draw graphs to see data fluctuations

    ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/a52fd659-9952-4db0-91a6-f6afc6bd0fbb)
 - Determine the p, d, q parameters of the ARima model using the auto_arima library to automatically find the best parameters for the model

    ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/32cc7f56-8997-4e37-a77d-a02a2a662c69)
    ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/990d7c6c-3765-471a-820e-f4af151de175)
 - After determining the best parameters are (1, 0, 0) in the ARIMA model, their meaning is:
    - p = 1: Autoregressive index, indicating that the model uses the value of autoregressive lag 1 to predict the current value.
    - d = 0: Integration index, indicating that the time series has been processed and does not need to be integrated to become stationary.
    - q = 0: Moving average index, indicating that the model does not use a moving average component in prediction.
 - Then build the ARIMA model

   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/8e61a88f-a2f3-48bf-a63b-e1b6b293adb6)
 
 - Evaluate model predictions
  
   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/d49d313b-8ff8-4570-b14b-447f1f016c7f)
 
 - The result of model

   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/90653038-345a-4a2a-8c50-2795edd1b03c)
   ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/88b7fa57-8976-4d1e-a9c6-cc4d3fbbf59d)

Conclusion: Use ARIMA for time series to build an expected revenue model, but the accuracy of the model may be limited due to the small amount of data.
