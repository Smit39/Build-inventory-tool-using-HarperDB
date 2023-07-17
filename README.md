# Building an inventory tool using DronaHQ and HarperDB

In this article, we will create an inventory tool using the DronaHQ Studio platform having database integration with HarperDB using REST API connector.

The inventory tool application will possess operational functionalities for effectively managing and observing the products within its HarperDB inventory.

HarperDB is a NoSQL database with SQL semantics. With flexible user-defined APIs, simple HTTP/s interface, and a high-performance single-model data store that accommodates both NoSQL and SQL workloads, HarperDB scales with your application from proof of concept to production. Install and manage on your hardware, or have them host it with HarperDB Cloud.

## Prerequisite
1. You should have an account on [DronaHQ](https://www.dronahq.com/signup/?utm_source=github&utm_medium=cubejs&utm_campaign=smit) with [Studio Login](https://studio.dronahq.com/login).

2. You should know how to configure the REST API connector. You can learn more about configuring REST API with various authentication methods from [here](https://community.dronahq.com/search?q=rest%20api).

3. You should have some knowledge of SQL/NoSQL language and queries.

Rest assured, everything will be covered in this article.

## Setting Up: HarperDB
A HarperDB account is required. One can create a free account from [here](https://www.harperdb.io/). 

HarperDB offers three main ways to access the service, namely:

1. HaperDB Studio: It is a web-based graphical user interface that allows users to create and manage HarperDB instances right from the interface. It also has several tools and features to aid the management process. You can find more info about this feature [here](https://docs.harperdb.io/docs/install-harperdb).

2. HarperDB Local Instance: This option allows users to install HarperDB locally on their computers. There are options to install HaperDB using docker, WSL, on Windows, or even on Linux OS. To find out more about this, click [here](https://docs.harperdb.io/docs/install-harperdb).

3. HarperDB Cloud: The cloud option allows you to access and use HarperDB from your application remotely without having a server running locally on your machine. Using this option, you can create a database instance online and use an API to communicate with the database. To find out more about this option, you can check [here](https://docs.harperdb.io/docs/harperdb-cloud).

Users can manage their local or cloud instances through the studio if they choose.

Once you log in to the HarperDB account, you will land on its `studio` page.

![create new HarperDB cloud instance](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image29-1024x532-1.png) 

Before starting with anything, we need to create HarperDB cloud instance. Choose **AWS or Verizon Wavelength HarperDB**. It is a free Harper cloud hosting service.

![select instance type](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image31-1.png) 
 
Click on **Instance Info**.

![select cloud provider](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image30-1.png) 
 
Provide the necessary details and make sure to keep your username and password of the database.

![create instance credential](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image33-1-768x516.png)
 
![instance space pricing](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image32-1.png)

Confirm instance details.

It will show you an overview of all the options that you have chosen. Click on the button to agree to the terms and submit the form.
You should now see that it has begun creating your database instance on its cloud system.
You should now see that it has begun the process to create your database instance on its cloud system.

![creating cloud instance](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image36-1-1024x373.png) 

Creating an instance will take a few seconds. Once it’s done, we need to add a table and data to our database. Click on your instance. It will take you to a page where you can add tables and data.

On the left-hand side, we need to provide a name for our new schema. Let it be `inventory`.

![create a schema](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image35-1024x535-1-1024x540.png)

Now, add a table name `product` with _hash attr._ Of `product_id`. The hash attribute is analogous to “Primary Keys” in relational databases and is a unique identifier for every entry in the Products table we just created.

![product records](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image41-1024x529-1-1024x534.png) 

## Adding data: HarperDB
Now to add data, on the top right-hand side, we will use the option of uploading CSV.

![updated time](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image39-1.png)

Once we click on it, we get various options to import our CSV file. I have a `CSV file` ready for the database of inventory.

Click on **Insert Records**.

![insert records](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image46-1024x251-1-1024x258.png) 

Your table will look something like this:

![table will look like this](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image42-1024x442-1-1024x448.png)

## Building Inventory tool with DronaHQ
Now that we have data in our HarperDB instance table, next, we will build our product-managing microapp using [DronaHQ Studio](https://studio.dronahq.com/login). 

Create a new app, this should bring you to the development environment like the one shown below:

![inventory tool](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image44-1024x534-1-1024x539.png) 

## Configuring REST API connector: HarperDB
First, we need to set up the connection of the Studio with HarperDB by configuring the REST API connector. The HarperDB has a basic auth REST API configuration, you can read more about Configuring [REST API – Basic Auth](https://community.dronahq.com/t/configuring-rest-api-connectors-basic-auth/416) from our community article.

We can get our HarperDB API/Instance URL and header authorization key in the config section of the cloud instance dashboard.

![instance config](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image47-1024x390-1-1024x396.png) 

Add a connector of **REST API**.

Select the authentication type as `Basic Authentication`. Provide the necessary details such as the connector name, username, and password. The username and password are the same as we have created for our HarperDB instance

![database connector](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image49-1-768x797.png) 

![add connector](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image51-1-768x771.png) 
 
Paste the Instance URL and provide a header.

The value of Authorization in the headers is usually of the format: `Basic <Instance API Auth key>`.

Do **Test Connection & Save**.

![configure test API](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image52-1-768x721.png) 

## Fetching Inventory Details
We will now add a sub-API to our configured HarperDB REST API connector to fetch the data from the database. Find your connector from the connectors list and click on **+ADD API**.

![Add API in Harperinventory SD](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image9-1024x174-2-1024x182.png)

Enter the API name. Select the method as `POST`. Notice that even though we are retrieving data, instead of `GET` we are using the `POST` method.

Here we have provided the sub-API name as `getData`.Click on **Advance**, and select content type as `RAW`. 

![Content type](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image15-1-768x833.png) 
 
Under the **Body/Form parameter** section, we will write a query as part of the request body to fetch all the products from the Product table in our database using SQL query. Irrespective of not being a relational database, HarperDB can read SQL queries and interact with them as though it were a relational database.

``` sql
{

    “operation”: “sql”,

    “sql”: “SELECT product_id, name, description, price, quantity, backorderLimit, backordered FROM inventory.product”

}
```

Here, we have made the operation type a “SQL” query and have written the SQL to query for all the data in the “product” table located under our earlier created “inventory” schema.

Click on **Test API & Save**.

![add api query](https://cdn1.dronahq.com/wp-content/uploads/2023/05/HarperDB-51-1.png) 
 
Your sub-API to fetch data will be added. 

![click getData](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image55-768x128-1-768x136.png)

## Displaying Data
Now, let’s go to our studio builder view and drop a table grid control, to view our products. Go to the data bind section of the table grid control and select the _connector_.

![expected format](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image20-768x340-1-768x346.png)

Simply select your HarperDb connector with the `getData` sub-API. 

Make sure that the keys of columns are selected to bind to the control. Do a **Test & Finish**.

![add HarperDB connector name](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image21-1.png) 

You can view the data from the database being populated in the table grid control. You can provide data formatting to the table grid columns. For instance, we can set the data type to the `number` of `Price`, `Quantity`, and `BackorderLimit` columns with the help of **Format Data**.

![data will appear](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image22-1024x525-1-1024x530.png) 

Add the column name and data type respectively to it. Click **Finish**.

![customise data format](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image23-1.png) 
 
## Submitting Inventory Details via Form
Next, the task for this inventory tool is to save new entries in our HarperDB. To send the data, we need to add another sub-API.

Find your connector from the connecter list and click on **+ADD API**.

![add API](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image9-1024x174-2-1-1024x182.png)

Enter the API name. Select the method as `POST`. 

Here we have provided the sub-API name as `sendData`.Click on **Advance**, and select content type as `RAW`.

![configure API for connector](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image24-1-768x965.png)

Under the **Body/Form parameter** section, we will write a query as part of the request body to send data of a product to the Product table in our database using SQL query. 

**Query**:
``` sql
{

    “operation”: “insert”,

    “schema”: “inventory”,

    “table”: “product”,

    “records”: [

        {

            “name”: “{{name}}”,

            “description”: “{{description}}”,

            “price”: “{{price}}”,                        

            “quantity”: “{{quantity}}”,

            “backorderLimit”: “{{backorderLimit}}”,

            “backordered”: “{{backordered}}”

        }

    ]

}
```

Here, we have made the operation type as an `insert` query with **8schema and table** details.

Then the SQL query is provided inside `records`. The value of all the attributes is written as variables. This will help us to send data dynamically from our app.

![body format](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image25-1-768x915.png) 
 
In the above image, we are providing records of the product table using the variables.

Click on **Test API & Save**.

![response of output](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image26-1-768x715.png) 

Your sub-API to update data will be added.

![send data](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image27-1024x171-1-1-1024x179.png)

## Submitting Data via Form

To take input details from the user in our microapp, we will create a form template. This form will take input regarding product details from the user and on-click of submit button, the data will be inserted in the `product` table of HarperDB instance using the `sendData` sub-API from the action builder.

In the below image, we have created a form template. We can see that for the `back ordered` column, we are using the `toggle` control values are Boolean, and that corresponds to the toggle control, which can represent both states as true or false.

![insert new product](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image28-1.png) 

Now let’s bind actions to the `submit` button.

![add button click](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image11-1.png)

Select the **HarperDB** connector from the Server-Side action list and select the `sendData` query.Bind the controls by using their keywords in the variable sections.add raw response 
  
![HPinv](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image12-1.png)

Click **Continue > Finish**. I am also adding an action of `reset control data` to reset the table grid control on the success of the action with the `sendData` API call to get the refreshed response of the products.

![Action flow](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image13-768x359.png)

Preview the app and insert details of a new product and hit `Submit`.

![insert new product and raw response](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image14-1024x474-1-1024x479.png) 

> You can view the logs of API calls from our **Logs** feature. In the above image, we can see that the API call of `sendData` was successful.

## Edit inventory details via table grid
Next, the task for this inventory tool is to update entries in our HarperDB. To update the data, we need to add another sub-API.

Find your connector from the connecter list and click on **+ADD API**.

![test API](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image9-1024x174-2-3-1024x182.png)

Enter the API name. Select the method as `POST`. 

Here we have provided the sub-API name as `updateData`.Click on **Advance**, and select content type as `RAW`.

![configure AP for your connector](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image15-1-2-768x833.png)

Under the **Body/Form parameter** section, we will write a query as part of the request body to send updated data of a product to the Product table in our database using SQL query. 

**Query**:
``` sql
{

    “operation”: “update”,

    “schema”: “inventory”,

    “table”: “product”,

    “records”: [

        {

            “product_id”: “{{proID}}”,

            “name”: “{{name}}”,

            “description”: “{{description}}”, 

            “price”: “{{price}}”,                         

            “quantity”: “{{quantity}}”, 

            “backorderLimit”: “{{backorderLimit}}”, 

            “backordered”: “{{backordered}}”

        }

    ]

}
```

Here, we have made the operation type as an `update` query with **schema and table** details.

Then the SQL query is provided inside `records`. The value of all the attributes is written as variables. This will help us to send the updated data dynamically from our app.

![app information about product](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image16-1-768x963.png)

In the above image, we are updating records of cilantro using the variables.

Click on **Test API & Save**.

![test of production](https://cdn1.dronahq.com/wp-content/uploads/2023/05/HarperDB-52-1-1.png)
 
Your sub-API to update data will be added.

![add different API](https://cdn1.dronahq.com/wp-content/uploads/2023/05/HarperDB-53-1.png)

## Edit data using table grid property
To update the data, we will use the edit columns property of table grid control. Simply go to your table grid control and in the property section, select the columns you want to make editable.

![SUB API](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image19-1024x328-1-1024x335.png)
You will notice the edit sign on the top of each selected column. Select every column except the ID.

Next, go to the _table grid > events > save changes_. This will open an action builder, through which we will trigger an event on Save Changes to execute an action of API call to update the database.

![INVENTORY TOOL](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image1-768x271-1-768x277.png)

Since there can be multiple rows with updated changes, we will use a client-side action of the iterate task to loop through each change.

![ENTER COUNT](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image2-10-768x339.png)

We will iterate with the help of `tablegrid.PROPERTIES.EDITEDROWS` property.

![JS CODE](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image3-10.png) 
 
Next, we have to save each of the properties in different output variables so that we can use it later to bind as keywords in our update query request of the connector.

![CONFIGURE YOUR ACTION](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image4-10.png) 
 
Under the JS Code editor add the client-side action of the configured connector, selecting the update query.
Now, bind the query variables with their appropriate keywords saved in variables from the previous JS Code editor.

![ENDPOINT](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image12-1-1.png) 

Click **Continue** then **Finish**.

**NOTE**: Make sure to add a refresh control action from the On-Screen Actions to view the updated data after saving the changes in the table grid.

![Preview of add](https://cdn1.dronahq.com/wp-content/uploads/2023/05/HarperDB-57-1.png)

Preview the App.

Table Grid before update-

![table grid update should save changes](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image7-1024x362-1-1024x368.png)

Table Grid after update-

![updated table grid](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image8-1024x245-1-1024x253.png)

> You can view the logs of API calls from our **Logs** feature. In the above image, we can see that the API call of `updateData` was successful.

## Deleting products from inventory tool
Finally, our last task for the inventory tool would be Deleting the data. To delete the data, we need to add another sub-API.

Find your connector from the connect list and click on **+ADD API**.

![select test API +ADD API](https://cdn1.dronahq.com/wp-content/uploads/2023/05/image9-1024x174-2-4-1024x182.png)

Enter the API name. Select the method as `POST`. 

Here we have provided the sub-API name as `deleteData`.Click on **Advance**, and select content type as `RAW`.

![DELETED DATA](https://cdn1.dronahq.com/wp-content/uploads/2023/05/HarperDB-50-1.png) 

**Query**:
''' sql
{

    “operation”: “sql”,

    “sql”:”delete FROM inventory.product WHERE product_id = \”{{id}}\””

}
```
Here, we have made the operation type as a `delete` query with schema and table details.

The operation type  “SQL” query and have written the SQL to query to delete the data in the “product” table located under our earlier created “inventory” schema with respect to the provided `product_id`.

PRODUCT ID USING VARIABLE 
In the above image, we are providing product id using the variables.

Click on Test API & Save.

TEST OF PRODUCTION 
Your sub-API to delete data will be added.

SUB API 
Delete data using the table grid property
To delete records using Table Grid control, toggle the `Delete` to ON from the properties. This will give us a nice and clean button of delete(TrashCan) with which we will bind the action to delete.

TABLE GRID CONTROL 
Go to the Events of the Table Grid control then select `delete_click`, an action builder will open.

select delete click 
Select the HarperDB Configured connector with `deleteData` sub-API from the server-side action list.

Bind the keyword of product ID to send it via API call.

product id 
Click Continue then Finish.

NOTE: Make sure to add a refresh control action from the On-Screen Actions to view the updated data after saving the changes in the table grid.

preview the app and check out delete function 
Preview your app and try out the delete functionality. 

delete function 
Building better inventory tools: Unleashing the potential of DronaHQ and HarperDB
Use HarperDB as the database backend to store and manage inventory data. Create tables to store information about products, stock levels, orders, transactions, and any other relevant data fields. HarperDB’s flexibility and real-time capabilities will enable efficient data storage and retrieval.

Utilize DronaHQ’s low code development platform to build the user interface and application logic for your inventory management tool. With DronaHQ’s visual development environment, you can create custom forms, tables, and workflows tailored to your specific inventory management requirements.

Establish a connection between DronaHQ and HarperDB to enable seamless data synchronization and real-time updates. Use DronaHQ’s integration capabilities to fetch data from HarperDB and display it in the application interface. Any changes made in the inventory management tool can be stored back in HarperDB for data consistency.

Leverage the features of both platforms to enhance your inventory management tool. For example, you can incorporate barcode scanning functionality, automated stock alerts, order tracking, reporting and analytics, user access controls, and more using DronaHQ’s capabilities.

By combining the strengths of HarperDB and DronaHQ, you can develop a robust and efficient inventory management tool that meets your specific requirements, providing real-time data management, streamlined workflows, and an intuitive user interface.

Build your own internal tools. Get started here!
