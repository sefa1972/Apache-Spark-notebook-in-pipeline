# Apache Spark Notebook in an Azure Synapse Pipeline

This project demonstrates how to use an Apache Spark notebook inside an Azure Synapse Analytics pipeline to transform sales order data. It covers the steps to provision Synapse resources, interactively run and test the notebook, and finally automate the process through a pipeline.

---

## Lab Overview

In this lab, you'll:
- Provision an Azure Synapse Analytics workspace
- Run a Spark notebook interactively
- Perform transformations on CSV data and store it in Parquet format
- Automate the transformation with a pipeline using parameters

---

## Prerequisites

- Azure Subscription
- Synapse workspace with a Spark pool (e.g., `sparkxxxxxxx`)
- Synapse Studio access
- PowerShell & ARM template (for provisioning)
- Downloaded notebook: `Spark Transform.ipynb`

---

## Step-by-Step Guide

### 1. Provision Synapse Analytics Workspace

Provision a Synapse workspace using the provided PowerShell script and ARM template.

---

### 2. Run the Spark Notebook Interactively

- Open **Synapse Studio**
- Navigate to `Allfiles/labs/11/notebooks`
- Upload and open `Spark Transform.ipynb`
- Attach it to your `sparkxxxxxxx` Spark pool
- Click **Run All**

⚠️ The first cell may take a few minutes due to Spark pool warm-up.

---

### 3. Review the Notebook

The notebook performs the following tasks:
- Sets a unique folder name using a variable
- Loads CSV sales order data from `/data`
- Splits customer name into multiple columns
- Saves transformed data in **Parquet** format under a unique folder

---

### 4. Validate Output

- In the **Data** tab, refresh to see the newly created folder
- Open it to confirm presence of `.parquet` files
- Right-click the folder > **New SQL Script** > **Select TOP 100 rows**
- Choose file type as **Parquet**, run the query

---

### 5. Add Parameters for Pipeline Use

- In the notebook, select the cell defining `folderName`
- Use the **...** menu > **[@] Toggle parameter cell**
- Click **Publish** to save changes

---

### 6. Create a Pipeline

- Go to **Integrate** > `+` > **Pipeline**
- Rename to `Transform Sales Data`
- Add a **Notebook** activity:
  - Name: `Run Spark Transform`
  - Notebook: `Spark Transform.ipynb`
  - Parameters:
    - `folderName`: `@pipeline().RunId`
  - Spark Pool: `sparkxxxxxxx`
  - Executor Size: `Small`

---

### 7. Publish & Trigger

- Click **Publish All**
- From **Add Trigger** > **Trigger Now** > **OK**
- Go to **Monitor** > **Pipeline Runs** to observe status

Once completed:
- Confirm creation of a folder with the **Pipeline Run ID**
- Check Parquet files within for the transformed data

---

## Cleanup

To avoid unnecessary charges, delete the Synapse workspace and other Azure resources created during this exercise.

---

## Project Structure
📁 project-root/
├── 📄 README.md                   # Project documentation
├── 📓 Spark Transform.ipynb       # Apache Spark notebook used for data transformation
└── 📁 deployment/                 # Scripts and templates for provisioning resources
    ├── 📄 synapse-arm-template.json   # ARM template for deploying Synapse Analytics workspace
    └── 📄 provision-workspace.ps1     # PowerShell script to provision the Synapse workspace

---

## Notes

- Apache Spark notebooks are a powerful tool for big data transformations.
- Integrating them into Synapse pipelines enables automation of batch jobs.
- Parameterization via `@pipeline().RunId` allows for dynamic folder creation per run.

---

## Author

**Sefa Öztürk**  
Junior Developer | Data Enthusiast | Lifelong Learner
🔗 www.linkedin.com/in/sefa-ozturk1972

---

## License

This project is for educational purposes only.


