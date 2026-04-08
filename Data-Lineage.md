# Introduction
DataArts is basically a one stop data governance platform where you can do many things. If you want to manage, audit, usually you'd do it in this platform
In this page we will focus on how to see data lineage of your table, usually you'd want to see this if you want to track where you tables have been, and what have been modified and so on. 

But first let's try to understand what is data-lineage? 

Data lineage is the process of tracking, recording, and visualizing data’s entire journey—from origin, through transformations, to final consumption.
In an easier understanding- it's when you want to see where your tables have been the entirity of it's life.

<img width="1213" height="486" alt="image" src="https://github.com/user-attachments/assets/0b6bdb0a-9e99-4219-a113-fae2f9e204c2" />

To see your data lineage there are some things that you need to set up

a. Complete metadata collection: In the data catalog component, a metadata collection task must be created and run for the data table whose lineage you want to view.

b. Successful job scheduling: The data development job must meet the automatic lineage resolution requirements or have been manually configured with lineages, and a formal scheduling operation must be performed (test runs are invalid).

c. After successful scheduling, the lineage relationship will be generated in approximately 1 minute.

Now let's try setting up and start the process!

## Initial Setup
Before we start into the DataArts itself please do note in Huawei Cloud DataArts we can only see data from Huawei Cloud's native products. So for example you'd want to track external data from outside of this environment, the DataArts cannot track it and will only track this data when it's inside of the Huawei's storage service. 

So before we start please prepare the sources that you'd want to monitor, for example you want to monitor a specific landing folder in OBS , please prepare that path.

## 1. Creating a Incremental Metadata Collection Task
First thing to do when you want to see your data lineage is actually creating a monitoring task for dataArts, this is so they can "see" what's happening to your data. 
Once you go to the DataArts product page, please click on the DataArts catalog. At first landing in this page won't really show you anything because we havent created a metadata collection. This job will be the main point on how we can get the data itself from your huawei cloud service to DataArts. 

Now on your left side of the navigation panel in DataArts Catalog there should be something called a 'Collection Task' , now let's jump into this section
<img width="1916" height="397" alt="image" src="https://github.com/user-attachments/assets/cb6461f7-6495-4718-9f31-7e9c990c0ced" />

Once you've clicked the collection task , you will see that you can create some sort of task, here i'd like to highlight again, alot of the sources can be seen if you click on the connection data connection type, here you will see many different huawei cloud products, usually we use big data in dataArts but you will see many databases and bucket as well, please do pick it in regard to your own scenario. Please note that some services will let you need to make a data connection first. 

<img width="1853" height="846" alt="image" src="https://github.com/user-attachments/assets/f5f2ec58-3575-4213-9224-4329f28a36a4" />

Now once you click "NEXT" you will see that you can try to change the scheduling, this is basically how often DataArts will capture the metadata from your source. It basically defines when and how often the DataArts system connects to data sources. You can make this a reocurring this by clicking the repeating part.

<img width="1023" height="256" alt="image" src="https://github.com/user-attachments/assets/07bd6e04-c925-4130-9fcc-72d0af11d889" />

Once you're done setting up your metadata monitoring please wait for a while. A successful monitoring can be seen through the task monitoring section of the DataArts catalog, and if it's still "running" in this page, then it means it's still doing well. The only time you need to stop and fix your metadata collection job, is if you see a failed status, other than that you are fine. Please refer to the picture below to see what im talking about. 

<img width="1707" height="633" alt="image" src="https://github.com/user-attachments/assets/adc928aa-42a8-4f52-b75c-31d019d55d32" />

Now we have done the metadata collection! 

## 2. Data Lineage Parsing
Now after we are done with the metadata collection this is where we can do the data lineage parsing. The parsing itself can be done in two different ways.

a. Automatic Parsing

In this mode, you don't have to do anything. If you write a standard script, the system "reads" your code and draws the map for you.

How does it work ? 
If you use an INSERT INTO or CREATE TABLE AS command, the system realizes, "Aha! Table A is moving into Table B," and draws the line.

The Big Catch: It only works for specific services:
1. DWS & MRS (Hive/Flink): Standard SQL commands.
2. CDM: When you migrate files between databases.
3. OBS: When you move files between folders or buckets.

The Rule: Your SQL scripts cannot have semicolons (;) inside a single node if you want the system to parse it automatically.

b. Manual Parsing

Sometimes the system isn't "smart" enough to read your code—specifically with Spark or Python jobs. Since the logic in those jobs can be very complex, the system asks you to label the inputs and outputs yourself.

When to use it: Use this for MRS Spark, Python, or REST Client nodes ? 

How to do it:
1. Open your job in Data Development.
2. Click on the specific node (like your Spark node).
3. Look for a tab called lineageInfo.
4. Manually pick your Input (where the data comes from) and Output (where the data goes).

## 3. Viewing Data Lineage
