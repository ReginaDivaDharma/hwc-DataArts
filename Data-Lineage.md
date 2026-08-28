# Viewing Data Lineage in Huawei Cloud DataArts

## Introduction

DataArts Studio is a one-stop data governance platform. If you need to manage, audit, or track data assets, this is where you do it.

This guide focuses on **data lineage** — how to see where a table has been and what has modified it over its lifetime.

### What is data lineage?

Data lineage is the process of tracking, recording, and visualizing a dataset's entire journey — from origin, through transformations, to final consumption. Put simply: it shows everywhere your table has been across its whole life.

<img width="1213" height="486" alt="Data lineage overview" src="https://github.com/user-attachments/assets/0b6bdb0a-9e99-4219-a113-fae2f9e204c2" />

---

## Prerequisites

Lineage will not appear unless all three conditions are met:

| # | Requirement | Detail |
|---|---|---|
| 1 | **Metadata collection completed** | A metadata collection task must be created and run in DataArts Catalog for the table whose lineage you want to view. |
| 2 | **Job scheduled successfully** | The DataArts Factory job must either meet the automatic lineage parsing requirements or have lineage configured manually — and it must be **formally scheduled**. Test runs do not generate lineage. |
| 3 | **Wait for generation** | After a successful scheduling run, the lineage relationship appears in roughly 1 minute. |

> **Scope limitation:** DataArts can only see data held in Huawei Cloud native services. External data is invisible to it until that data lands inside a Huawei Cloud storage or compute service.

Before starting, prepare the sources you want to monitor — for example, the exact OBS path of the landing folder you intend to track.

---

## 1. Create an Incremental Metadata Collection Task

The collection task is how DataArts "sees" what is happening to your data. Without it, DataArts Catalog stays empty.

**Steps**

1. Open the **DataArts Catalog** component.
2. In the left navigation panel, go to **Collection Task**.

   <img width="1916" height="397" alt="Collection Task in the left nav" src="https://github.com/user-attachments/assets/cb6461f7-6495-4718-9f31-7e9c990c0ced" />

3. Create a new task and choose your **data connection type**. This dropdown lists the supported Huawei Cloud products — big data services are the most common choice here, but databases and buckets are also available. Pick according to your own scenario.

   > Some services require you to create a **data connection** first before they appear as a valid source.

   <img width="1853" height="846" alt="Selecting the data connection type" src="https://github.com/user-attachments/assets/f5f2ec58-3575-4213-9224-4329f28a36a4" />

4. Click **Next** and configure the **schedule**. This defines when and how often DataArts connects to the source to capture metadata. Enable the repeat option to make it recurring.

   <img width="1023" height="256" alt="Schedule configuration" src="https://github.com/user-attachments/assets/07bd6e04-c925-4130-9fcc-72d0af11d889" />

5. Wait for the task to run. Check progress under **Task Monitoring** in DataArts Catalog.

   - **Running** — healthy, nothing to do.
   - **Failed** — stop and fix the collection task before continuing.

   <img width="1707" height="633" alt="Task monitoring status" src="https://github.com/user-attachments/assets/adc928aa-42a8-4f52-b75c-31d019d55d32" />

Metadata collection is now in place.

---

## 2. Data Lineage Parsing

DataArts builds lineage from the **nodes** that run in DataArts Factory. A node defines an operation performed on data — DataArts Factory provides nodes for data integration, database operations, resource management, and compute/analysis.

There are two ways lineage gets parsed from those nodes.

### A. Automatic parsing

You do nothing. If the script is standard, the system reads the code and draws the map for you.

**How it works:** when you run `INSERT INTO` or `CREATE TABLE AS`, the system recognises that Table A is feeding Table B and draws the connection.

**Supported services only:**

- **DWS** and **MRS** (Hive / Flink) — standard SQL commands
- **CDM** — when migrating files between databases
- **OBS** — when moving files between folders or buckets

> **Rule:** a single node's SQL script must not contain semicolons (`;`) if you want automatic parsing to work.

#### Setting up automatic parsing

**1. Create the job in DataArts Factory**

Go to **DataArts Factory → Develop Job**. Choose the **pipeline** job type so that nodes with lineage can be created.

<img width="587" height="656" alt="Develop Job in DataArts Factory" src="https://github.com/user-attachments/assets/92a69c42-ef72-4880-8b6a-38e40a62afab" />

**2. Configure the node parameters**

Each service has its own node configuration — see the [DataArts Studio node reference](https://support.huaweicloud.com/intl/en-us/usermanual-dataartsstudio/dataartsstudio_01_0441.html) for details.

Drag the service you want onto the canvas. In this example we use a **DLI node**.

<img width="1912" height="865" alt="Dragging a node onto the canvas" src="https://github.com/user-attachments/assets/32768119-9a55-4b26-8460-c7426b5ae2a7" />

Fill in the form on the right. DataArts parses lineage from the SQL statement you enter here. For example, to list the tables available in a database:

```sql
SHOW TABLES IN dummy_data
```

Test the node once the form is complete.

> **Important:** testing alone does not deploy anything. You must click **Submit**, then **Execute**, to tell DataArts the job is ready to run for real.

<img width="1391" height="871" alt="Submit and execute the job" src="https://github.com/user-attachments/assets/2d7aadf3-fe9e-4c76-b21a-ecaf6a82abc9" />

### B. Manual parsing

For Spark and Python jobs the logic is often too complex for the system to read, so you label the inputs and outputs yourself.

**When to use it:** MRS Spark, Python, and REST Client nodes.

**Steps**

1. Open your job in **Data Development**.
2. Click the specific node (for example, your Spark node).
3. Open the **lineageInfo** tab.
4. Manually select the **Input** (where the data comes from) and the **Output** (where the data goes).

---

## 3. Viewing the Data Lineage

Once metadata collection has completed and the DataArts Factory job has been scheduled and run, return to **DataArts Catalog** and select the job or the output table. The lineage graph is displayed there.

<img width="1853" height="667" alt="Data lineage view in DataArts Catalog" src="https://github.com/user-attachments/assets/cb0f5c96-5b5a-4327-8777-d4b8545bec7e" />

---

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| Catalog is empty | No metadata collection task has been created or run. |
| Collection task shows **Failed** | Check the data connection and source path, then rerun. |
| Job ran but no lineage appears | The job was only **tested**, not submitted and executed. |
| Automatic parsing produced nothing | Semicolons in the node's SQL, or the service is not one of DWS / MRS / CDM / OBS — use manual parsing instead. |
| Table is not tracked at all | The data is not yet inside a Huawei Cloud native service. |
