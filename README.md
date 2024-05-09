## DataLake_Ecommerce

## Scope
This project is intended to provide experience with data engineering tasks using Fabric Spark and/or Data Pipelines to build out Delta Parquet tables and then use the Direct Lake connector in Power BI to query large volumes of real data. The medallion lakehouse architecture is followed in this sample where raw CSV files are loaded to Bronze Layer, then Silver Layer flat table built using Delta Parquet format and lastly Gold Layer tables serve up the star schema model for a Direct Lake Power BI dataset.

## Step 1: Create Lakehouse, upload data to Lakehouse and create Delta Parquet Table using Fabric Spark
In this step we will upload raw files

    1. Open up your Fabric Workspace and switch to Data Engineering persona using the menu on bottom left corner
    2. Create a new Lakehouse or use an existing one
    
  ![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/0af9f5d4-d313-4be0-a2e9-6c8dd263238d)

    3. Download Load Sale Data Spark Notebook from local to your local machine
    4. In the Files directory, create two additional directories: "Current" to store newly uploaded files, and "Archive" to store successfully loaded historical files into the Lakehouse.
![image](https://github.com/lonGDiBo/DataLake_Ecommerce_Using_Fabric/assets/115699195/436ee0f1-23c0-480a-9dcb-5e9c04cbe13d)
    
    5. Import the downloaded Notebook into Current
    6. Open the Notebook once the import is successful, you might need to update the Lakehouse association of the Notebook
    7.Then upload data files to Lakeshouse and then run Spark Notebook to create single flat Delta Parquet table with some minor modifications like data type changes, new columns based on existing columns, etc.

    
