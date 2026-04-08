# Introduction
DataArts is basically a one stop data governance platform where you can do many things. If you want to manage, audit, usually you'd do it in this platform
In this page we will focus on how to see data lineage of your table, usually you'd want to see this if you want to track where you tables have been, and what have been modified and so on. 

## Initial Setup
Before we start into the DataArts itself please do note in Huawei Cloud DataArts we can only see data from Huawei Cloud's native products. So for example you'd want to track external data from outside of this environment, the DataArts cannot track it and will only track this data when it's inside of the Huawei's storage service. 

So before we start please prepare the sources that you'd want to monitor, for example you want to monitor a specific landing folder in OBS , please prepare that path.

## 1. Creating a Monitoring Task
First thing to do when you want to see your data lineage is actually creating a monitoring task for dataArts, this is so they can "see" what's happening to your data. 
Once you go to the DataArts product page, please click on the DataArts catalog. At first landing in this page won't really show you anything because we havent created a metadata collection. This job will be the main point on how we can get the data itself from your huawei cloud service to DataArts. 

Now on your left side of the navigation panel in DataArts Catalog there should be something called a 'Collection Task' , now let's jump into this section
<img width="1916" height="397" alt="image" src="https://github.com/user-attachments/assets/cb6461f7-6495-4718-9f31-7e9c990c0ced" />

Once you've clicked the collection task , you will see that you can create some sort of task, here i'd like to highlight again, alot of the sources can be seen if you click on the connection data connection type, here you will see many different huawei cloud products, usually we use big data in dataArts but you will see many databases and bucket as well, please do pick it in regard to your own scenario. Please note that some services will let you need to make a data connection first. 

<img width="1853" height="846" alt="image" src="https://github.com/user-attachments/assets/f5f2ec58-3575-4213-9224-4329f28a36a4" />

Now once you click "NEXT" you will see that you can try to change the scheduling, this is basically how often DataArts will capture the movement of your data. 
You can make this a reocurring this by clicking the repeating part

<img width="1023" height="256" alt="image" src="https://github.com/user-attachments/assets/07bd6e04-c925-4130-9fcc-72d0af11d889" />


## 2. Creating 
