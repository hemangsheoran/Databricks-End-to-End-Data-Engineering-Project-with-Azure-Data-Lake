# Repository Overview
This repository demonstrates the complete lifecycle of a modern data pipeline — from raw data ingestion to analytics — built entirely on Azure Databricks using PySpark, Delta Lake, and the Bronze-Silver-Gold architecture. It integrates real-time and batch processing with Databricks Autoloader, Delta Live Tables, and Slowly Changing Dimensions (SCD), showcasing how to design, orchestrate, and optimize a scalable Lakehouse solution.
/
<img width="1617" height="867" alt="Screenshot 2025-10-24 145738" src="https://github.com/user-attachments/assets/76ce001c-8542-4df9-8ed2-b2d02f7e3c91" />
/
# Components
Azure resources: storage account, databricks workspace, unity catalog.
Autoloader: incremental file ingestion.
Structured Streaming: real-time data ingestion and processing.
Delta Live Tables: manage pipeline.
PySpark: definition of functions, classes, transformations, SCD logic.

# Technology and Tools
Azure Databricks: workspace where Spark clusters run.
PySpark: for Spark DataFrame API, streaming, transformations.
Delta Lake: storage layer enabling ACID, time travel, schema enforcement.
Azure Blob / ADLS Gen2: storage for raw/bronze/silver/gold layers.
Unity Catalog: for data governance.
Structured Streaming / Autoloader: for ingestion.
/
<img width="1515" height="765" alt="Screenshot 2025-10-26 202845" src="https://github.com/user-attachments/assets/a76cb2a2-7071-48bc-9213-5bd54941a55a" />
/
# Prerequisites
An Azure account.
Databricks workspace with required permissions.

# Project Setup
**1. Azure Setup**
a. Azure Resource Setup- Sign in to the Azure Portal Create a new Resource Group.
/
<img width="1512" height="797" alt="Screenshot 2025-10-26 220235" src="https://github.com/user-attachments/assets/f32454c0-b450-464f-90ba-776d27860ab3" />
/
b. In the same resource group, create a Storage Account. Select Azure Data Lake Storage Gen2 (ADLS Gen2). Enable Hierarchical Namespace (important for Datalake creation on Azure). Under “Containers,” create: bronze silver gold source and metastore.
/
<img width="1515" height="795" alt="Screenshot 2025-10-26 220322" src="https://github.com/user-attachments/assets/dc9bed77-5dc6-4791-be0b-bb5474d6b7d9" />
/
<img width="1519" height="798" alt="Screenshot 2025-10-26 220350" src="https://github.com/user-attachments/assets/783f456d-42b0-4f8e-9c43-edec8d4e4520" />

c. Create an Azure Databricks Workspace In the Azure portal → Resource group → search for Azure Databricks. choose a name, select the same Resource Group and region as the storage account.create with appropriate settings , then click “Launch Workspace.”
/
<img width="1511" height="757" alt="Screenshot 2025-10-26 220433" src="https://github.com/user-attachments/assets/43b1a984-49d3-4fd4-b7fd-53f08c781bd9" />
/
d. Select your Account which has Full admin access.
/
<img width="1515" height="763" alt="Screenshot 2025-10-26 220653" src="https://github.com/user-attachments/assets/99a9921e-fbaf-4bc0-a8f2-cd4982d81832" />
/
**2. Storage attachment to Databricks Workspace**
a. In Azure in the same resource group create an Access connector for Azure Databricks.
/
<img width="1519" height="745" alt="Screenshot 2025-10-26 221108" src="https://github.com/user-attachments/assets/29a22a00-f61c-4191-934a-b2761cd23ffa" />
/
b. Go to your Storage account, select Access Control Click on Add(Add role Assignment) Search for Storage Blob Data Contributor, select it and than Next, Select Managed Identity than select your Access connector name.
/
<img width="1517" height="798" alt="Screenshot 2025-10-26 221700" src="https://github.com/user-attachments/assets/a07aee55-5586-4e48-8a6a-57c935cecc21" />
/
<img width="1519" height="794" alt="Screenshot 2025-10-26 221327" src="https://github.com/user-attachments/assets/4c3f708e-3cb2-4b79-979b-ed6c02b45481" />
/
<img width="1515" height="702" alt="Screenshot 2025-10-26 221641" src="https://github.com/user-attachments/assets/ea1a52d4-5346-4f92-9492-bd540deef0b8" />
/
c. Go to Databricks Admin Access account In catalog create a metastore with the access connector Id and assign this metastore to the Databricks workspace which was created.
/
<img width="1519" height="755" alt="Screenshot 2025-10-26 221816" src="https://github.com/user-attachments/assets/ed7facec-d483-4ec4-ac81-02ee663d049b" />
/
<img width="1518" height="773" alt="Screenshot 2025-10-26 221804" src="https://github.com/user-attachments/assets/fe8307c5-d387-432a-8620-1921ab99f23d" />
/
**3. Injestion Transformation and loading of data**
a. Create a all purpose clustur.
/
<img width="1515" height="762" alt="Screenshot 2025-10-26 222355" src="https://github.com/user-attachments/assets/8f39c7db-9a9f-4bc6-b7b8-7b195c87fe3e" />
/
b. Fetch the Notebooks from Repository 
/
<img width="1518" height="763" alt="Screenshot 2025-10-26 222548" src="https://github.com/user-attachments/assets/3e52b5c6-0d60-491b-b3cd-fcea2295b963" />
/
c. Assign clustur to the Notebooks during processing.
/
<img width="1517" height="741" alt="Screenshot 2025-10-26 222657" src="https://github.com/user-attachments/assets/56115c8e-8c08-48ad-ae2e-057e228250e9" />
/
d. All the logical implementation on given data is written under Notebooks.
e. For the processing of Gold Products Notebook instead of cluster their is a need of creation of pipeline. Go to job run select create pipeline and than attach it to the Gold Products Notebook with appropriate setting.
/
<img width="1516" height="793" alt="Screenshot 2025-10-26 223137" src="https://github.com/user-attachments/assets/e470ad0c-a7be-46a6-ac08-a7299ab51dab" />
/
<img width="1516" height="756" alt="Screenshot 2025-10-26 223011" src="https://github.com/user-attachments/assets/1cd6c185-4f8f-4692-963f-4678e48eddfc" />
/
<img width="1517" height="752" alt="Screenshot 2025-10-26 223001" src="https://github.com/user-attachments/assets/de1ba585-9733-4693-80d3-69f6cc73e418" />
/

**4. Pipeline creation**
a. In the job run section, click on create job, click on add task assign it to Parameters notebook than for each loop using different parameters add different notebook with different tasks according to the given below Diagram.
/
<img width="1517" height="767" alt="Screenshot 2025-10-24 145738" src="https://github.com/user-attachments/assets/86e64259-b564-4f21-8e47-82b4cef28051" />
/
<img width="1519" height="798" alt="Screenshot 2025-10-24 145854" src="https://github.com/user-attachments/assets/6ae0a184-9e25-480c-b9cf-f2f36526adbb" />
/
<img width="1517" height="765" alt="Screenshot 2025-10-24 145640" src="https://github.com/user-attachments/assets/1e54649e-2b48-4742-85c4-a1c32ce186a6" />
/
**When the pipeline successfully run the project has been completed. Data has been automatically started injestion processing and loaded in different layers when added to the containers created.**
# Thank You and sorry if their is any spelling mistake you found in this file.
