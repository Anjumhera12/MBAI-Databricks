
Navigating Databricks and Delta Sharing 
=========================================

**Log in to Databricks**: 

* Navigate to the Catalog and access Delta Sharing. 

.. figure:: images/1.png

Retrieve the Sharing Identifier 
--------------------------------

Click on **Shared with me** and then copy the Sharing Identifier. Each client has a unique identifier. 

**Note:** Clients must share using the main ID as the client ID. This helps identify shares and map them to the correct client ID. 

.. figure:: images/2.png

Creating and Sharing Catalogs 
==============================

Create Separate Catalogs 
-------------------------

A separate catalog must be created for each client to share their data and results. Each workspace can contain multiple catalogs. 

Create a new catalog by going into the **Catalog** → **Create catalog**. Catalog name should match with client_id provided by Matchbook AI. 

.. figure:: images/3.png

Provide an appropriate catalog name along with storage location based on your external location and click **Create**. 

.. figure:: images/4.png

Create a new Personal Access Token to call the Databricks API. Go to **Settings**. 

.. figure:: images/5.png

From the left pane, select **Developer**.

.. figure:: images/6.png

Now, click **Manage**.

.. figure:: images/7.png

Then, click on **Generate new token**.

.. figure:: images/8.png

Note the generated access token and click on **Done**.

.. figure:: images/9.png

Next, store the previously created Access Token in the compute cluster. To do this you will require the details listed below: 

* **DATABRICKS_HOST**: This is an URL of your **Databricks Instance URL**. 

* **DATABRICKS_TOKEN**: Previously created Personal **Access Token**.

* **DATABRICKS_CLUSTER_ID**: Compute Id. 

Go to the **Compute** → **Your Cluster** → **View JSON**.  

.. figure:: images/10.png

A JSON file opens, where you can view **cluster id**.

.. figure:: images/11.png

Go to the **Compute** → **Your Cluster** → **Advance Options** . 

.. figure:: images/12.png

Once Advanced options is opened, click on **Spark** then two sections **Spark config** and **Environment Variables** are displayed.

.. figure:: images/13.png

Go to the **2 - Onboarding - Cleansematch**, fill values and run the script by clicking on the **Run all** button. 

.. figure:: images/14.png

.. list-table::
    :header-rows: 1

    * - Field
      - Required
      - Data Type
      - Description
    * - client_id
      - Yes
      - String
      - Uniquely identifies different clients in delta sharing mode.
    * - source_schema
      - Yes
      - String
      - Name of schema where source data is available.
    * - source_table
      - Yes
      - String
      - Name of table in which source data is available to do the matching.
    * - destination_schema
      - Yes
      - String
      - Name of schema where you want to store match output data.
    * - destination_table
      - Yes
      - String
      - Name of table where you want to store match output data.
    * - Batch_size
      - Yes
      - String
      - Size of batch, allows parallel Matchbook API calls; must be between 1 to 50.
    * - delta_sharing_mode
      - No
      - String
      - If running in delta sharing mode, pass this parameter; otherwise, do not include it.
    * - tags
      - No
      - String
      - Optional tags to include in the Matchbook API call.

Go to the **3 - Onboarding - Download Matched Data**, fill values and run the script by clicking on the **Run all** button.

.. figure:: images/15.png

.. list-table::
    :header-rows: 1

    * - Field
      - Required
      - Data Type
      - Description
    * - client_id
      - Yes
      - String
      - Uniquely identifies different clients in delta sharing mode.
    * - source_schema
      - Yes
      - String
      - Name of schema where we want to store the match output stage data.
    * - source_table
      - Yes
      - String
      - Name of table in which we want to store the match output stage data.
    * - destination_schema
      - Yes
      - String
      - Name of schema where match output data is available.
    * - destination_table
      - Yes
      - String
      - Name of table where match output data is available.
    * - batch_size
      - Yes
      - String
      - Size of batch; allows parallel Matchbook API calls. Must be between 1 to 50.
    
Go to the **4 - Onboarding - Process Enrichment Data**, fill values and run the script by clicking on the **Run all** button. 

.. figure:: images/16.png

.. list-table::
    :header-rows: 1

    * - Field
      - Required
      - Data Type
      - Description
    * - client_id
      - Yes
      - String
      - Uniquely identifies different clients in delta sharing mode.
    * - source_schema
      - Yes
      - String
      - Name of schema where match output data is available.
    * - source_table
      - Yes
      - String
      - Name of table where match output data is available.
    * - destination_schema
      - Yes
      - String
      - Name of schema where you want to store the enrichment data.
    * - destination_table
      - Yes
      - String
      - Name of table where you want to store the enrichment data.
    * - batch_size
      - Yes
      - String
      - Size of batch; this allows processing data in chunks.
    * - enrichment_types
      - Yes
      - String
      - Type of enrichments you want to consider for processing.

Go to the **5 - Onboarding - Secrets**, fill values and run the script by clicking on the **Run all** button. 

.. figure:: images/17.png

.. list-table::
    :header-rows: 1

    * - Name
      - Data Type
      - Retrieval
      - Usage
    * - api_key
      - string
      - Matchbook AI portal
      - Required to call Matchbook AI APIs.
    * - api_secret
      - string
      - Matchbook AI portal
      - Required to call Matchbook AI APIs.
    * - api_url
      - string
      - https://api.my.matchbookservices.com
      - Required to call Matchbook AI APIs.
    * - auth_token
      - string
      - Databricks portal
      - Required to call Databricks' REST APIs. Used to trigger Process Enrichment Data job from Cleansematch & Download Matched Data.
    * - catalog
      - string
      - Databricks portal
      - Unity catalog name where input data is available. 

         * If mode is delta sharing, then catalog belongs to where you want to store your output results. 

         * If mode is direct then, then catalog belongs to where your input data is available and want to store the output result as well. 
    * - column_mapping
      - string
      - Custom
      - Workflow needs to call Matchbook API with specific fields, if your data does not have exact columns then you can create a mapping where you can map your columns to Matchbook AI’s conventions. 
    * - databricks_instance_url
      - string
      - Databricks portal
      - Required to call Databricks' REST APIs.
    * - delta_sharing_identifier
      - string
      - Databricks portal
      - Required only in delta sharing mode. Optional otherwise.
    * - job_id
      - string
      - Databricks portal
      - This job id belongs to the Process Enrichment Data. Go to the Process Enrichment Data workflow. 
    * - primary_key
      - string
      - Input dataset
      - Column name from source data which is uniquely identified across entire dataset. This is needed to properly functions jobs, making updates in the dataset, etc. 
    * - subdomain
      - string
      - Matchbook AI portal
      - Required to call Matchbook AI APIs. We pass it in the API call while retrieving token. 

**Note**: 
1. Under job id, Go to the Process Enrichment Data workflow. 

.. figure:: images/18.png

2. Copy the job Id from below:

.. figure:: images/19.png

Task Scheduling / Workflows 
----------------------------

To streamline data operations and ensure high-quality, enriched records, our Databricks environment leverages three key **automated workflows**. Each workflow plays a specific role in the data pipeline from matching and cleansing to enrichment and downstream delivery.  

The three different workflows are: 

1. **Cleansematch workflow**

This workflow handles the initial preparation and entity resolution process. It standardizes incoming records, removes duplicates, and performs intelligent matching against reference data (such as D&B or internal golden records). 

2. **Download Matched Data workflow** 

Once matching is complete, this workflow retrieves and stores the matched output in a structured format. It organizes the data for easy querying, analytics, or handoff to other systems. 

3. **Process Enrichment Data workflow**

This workflow applies data enrichment by appending additional attributes (e.g., firmographics, hierarchy data, or industry codes) from third-party sources like D&B. It enhances the matched data with rich context to support better decision-making. 

*How to start the workflow*
~~~~~~~~~~~~~~~~~~~~~~~~~~

To start the workflow, simply click the **play** button located on the right side of the workflow row in the Databricks interface.

.. figure:: images/20.png

*How to schedule recurring workflow run* 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Click on the workflow in which you need to set up a recurring run. 

.. figure:: images/21.png

Click on **Add Trigger** in the **Schedules & Triggers** section located on the left side of the workflow page.

.. figure:: images/22.png

A pop-up window will appear. In the dropdown menu, select **Scheduled** as the trigger type.

.. figure:: images/23.png

By default, the **Trigger Status** is set to **Active**, the **Trigger Type** is set to **Scheduled**, and the **Schedule Type** is set to **Simple**. In the Simple mode, you can specify the number of days for the recurrence. If you select **Advanced**, you'll also be able to specify the exact time for the trigger. You can optionally check the box next to **Show cron syntax** to view or customize the schedule using cron expressions. Select the appropriate **Periodic** option based on your requirement, then click **Save** to apply the recurring schedule for the workflow.  

If you need to pause the schedule, simply change the **Trigger Status** to **Paused**.

.. figure:: images/24.png

Sharing the catalog
--------------------

If the client wants to share the catalog with themselves, click on '**New Recipient**' then a new window will open.

.. figure:: images/25.png

Provide a recipient name, enter the sharing identifier, and provide any related comments. Once done click create.

.. figure:: images/26.png

Use case:
~~~~~~~~
 
Let's assume that a client wants to share some data with Matchbook in the workspace, and these are my inputs or input records. 

**Set Up the Recipient** 

1. Go to Delta Sharing and set up the recipient. Matchbook will share the identifier with the client. 

2. Select a new recipient (e.g., Matchbook AI) and provide the data sharing identifier.

Share Data
============

Click on '**Share Data**' to open a new screen.

.. figure:: images/27.png

Follow the specific format provided by Matchbook to ensure proper identification.

.. figure:: images/28.png

Share Name
----------

1. The share name must follow a specific format. 

2. We provide the client with a subdomain to ensure we can identify the client at Matchbook. 

3. Click 'Save and Continue.'

Adding Data assets
------------------

.. figure:: images/29.png

Once you’ve added the required assets click Save and Continue.

Adding Notebooks
----------------

Add the required data assets and notebooks and click **Save and Continue** to proceed. 
 
**Note**: Prepare the recipient to receive data and ensure they have the necessary access and setup.

.. figure:: images/30.png

Add Recipient
-------------

**Recipient Preparation** 

* Ensure the recipient is prepared to receive data. 

**Data Sharing** 

* User shares data, typically sample data, identified with a client ID like MBQA1. 

**Asset Addition** 

* Add assets to be shared, which could be any data set. 

* Data is available within a specific schema, offering options to share either the schema or the table. 

**Granting Access** 

* Perform the grant share action to provide access to the recipient. 

* Verify the added recipient and perform the grant share action to grant access. 

**Confirmation** 

* Confirm that Matchbook now has access to the shared data. 

**Process Establishment**

**Customer's Responsibilities**: 

* Follow the steps outlined to share data via Delta Sharing. 

**Our Responsibilities**: 

* Configure processes and setups based on the shared data. 

**Process Configuration**: 

**Jobs Creation** 

* Establish jobs such as cleanse, match, classify, enrich data, and refresh. 

* Within each job, configure specific settings and parameters. 

* Start with configuring the share name in the cleanse match job. 

**Note**: This sharing process is manual, and our system will automatically generate a catalog linked to your client ID for seamless access. Each client will have a unique identifier. 

Delta Sharing enables the creation of larger workspaces that can be shared with customers or clients for enhanced collaboration and data accessibility.

To view the table shared with you, follow these steps: 

Managing the key vault
=======================

* Databricks does not facilitate the addition of secrets through the user interface. Therefore, options are available either via API or the CLI. 

* We can organize secrets within scopes. Each scope can contain specific secrets tailored to individual clients. For instance, we'll establish client-specific scopes where API keys and secrets will be managed. Let me demonstrate this by listing the secrets within a scope. I'll specify the scope corresponding to the client's name. Within this scope, we can manage and update various items such as tokens, API keys, and others. 

Managing Auth token 
====================

* This triggers a specific process automatically, namely the data enrichment process. To achieve this, we make calls to the Databricks REST API. The token used for this purpose can be generated directly from Databricks. It's important to note that this token is specifically for the Databricks database and is not our own token. It is utilized solely to trigger their API.  

Retrieving the Token for the Client
====================================

Each job generates a token at the outset, which is utilized throughout its execution. A new token is not generated multiple times during the job execution; instead, a new token is created only if the current one has expired. This ensures efficient token management and prevents unnecessary token creation. 

The output table will contain new records, and each new job will trigger automatically.

Import Data
------------

To import data from a source file into Matchbook AI, perform the following steps.  

1. Log into platform and navigate to the Import Data section in the main menu.

.. figure:: images/31.png

2. Click on the **Import Data** button and select the **Import File** option.

.. figure:: images/32.png

Note: By selecting Import Data, you have the option to import data through UI or automated import.

3. The Import Data dialog box is displayed.

.. figure:: images/33.png

4. Select the file type of the file that you want to import data from.  

.. figure:: images/33.png

Data can be imported from the following file types:  

* Excel file   

* CSV file  

Select the **Import Type**. This allows users to match and improve the imported data. 

There are two ways to import your data:  

i. **Cleanse, Match & Enrichment**: This feature enables you to verify and align imported data with data from third-party providers to guarantee its correctness, utility, and precision. After the data has been verified and aligned, it is augmented with pertinent information sourced from third-party data providers.   

.. figure:: images/33.png

ii. **Enrichment Only**: This functionality permits the enhancement of data that has undergone cleaning and alignment through a comparison with data from third-party data providers. Subsequently, the data is enriched to enhance or expand its content with additional pertinent information from external referential sources. To select this option, perform the following steps. 

.. figure:: images/34.png

Users can also import data by manually entering it as a single record, use the toggle button on the top to switch between **Cleanse, Match & Enrichment or Enrichment Only**. The screenshot below displays the form to do Cleaning, Matching and Enrichment on a Single record. Users can manually enter the details on this screen. 

.. figure:: images/35.png

The screenshot below displays the form to perform **Enrichment Only**, on a Single record. Users can manually enter the details on this screen. 

After selecting the required import type, click **Next** to proceed.  

.. figure:: images/34.png

iii. To upload the source data file, click on the **Browse** button. 

.. figure:: images/36.png

You can also select any existing templates to define the data import, by selecting it from the list of options provided in the Select File Template dropdown menu. Choose the file template. This allows users to save a mapping of the file in the Matchbook AI platform.  

Specify whether the source file contains headers by checking the **Has Header** checkbox. This lets the system know that the data mapping is to be performed based on the Header Name and not the position of data.  

**Note**: If the input file does not contain headers, then the headers can be manually mapped during Data Mapping as an Input Header. For more information on how to add/modify input headers refer to the Data Mapping section of this guide.  

.. figure:: images/37.png

Choose the file you want to import from its location on your system and click **Next**.  

**Note**: For Importing the file, it is mandatory that the file is closed in the background. 
 
Select the required Sheet.  

**Note**: This step is required Excel or CSV required formats, that may contain multiple sheets. This feature enables you to choose and import the specific sheet you need. 

.. figure:: images/38.png

Process Enrichment Data
=========================
 
* A Python script is crafted for each client, consolidating all necessary values such as MB_QA1, API keys, and secrets for initial configuration. This script automates the generation of required components. Clients will establish these jobs, enabling direct invocation of the REST API and eliminating the need for manual intervention entirely. 

**Enrichment Classes Optimization** 

This optimization focuses on completing three key tasks: 

* Stage Table Optimization: Enhance the structure and performance of the stage table. 

* Centralize All Parameters: Consolidate and organize all parameters in a centralized location for easier management.

Optimization
--------------

* Optimize the structure and performance of the stage table. 

* Centralize all parameters to facilitate easier management. 

* Implement input merge updates.

.. figure:: images/26.png

Run Jobs
---------

* Navigate to the Jobs tab in the Jobs run section and select the client's name. 

* Click on the Workflow tab to access scripts that can be run manually or automatically. 

.. figure:: images/39.png

Within the left-hand menu, locate and click on the Workflow tab. This tab provides scripts that can be run manually or automatically on a recurring basis. 

**Note**: You can select the cluster on which to assign a job based on your requirements.

Assign Jobs to Clients
------------------------

* To assign the job to the client through Compute: 

* Navigate to SQL Warehouse and select the Starter Warehouse. 

* Share the script with the client and set up the workflow. 

Once these steps are completed, the job assignment process will be complete.

.. figure:: images/39.png

Running the Cleansematch Process
----------------------------------

Initial Setup 
~~~~~~~~~~~~~~

1. Cleansematch is the first mandatory process that should be run initially. Click on the **Run Now** button. 

2. Configure each task under the Tasks tab, ensuring all settings are correct. 

Configuration and Validation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Check the batch_size configuration (must be between 1 and 50). It will throw an error if not within this range. 

* Validate the delta_sharing_mode configuration parameter. 

* Ensure the share name matches the client_id. 

These steps will help ensure proper configuration and operation. 

.. figure:: images/40.png

**Catalog and Table Management**

* If the catalog does not exist, it will be created automatically. 

* If the destination_table is not available, clone the source table and add a Status column set to "NEW" by default. 

If the delta_sharing_mode configuration parameter is provided, it will validate the provider using delta_sharing_identifier. If a valid provider does not exist, an error will be thrown. 

When the client shares data using delta sharing, they need to ensure the share name matches the client_id. This is necessary to identify which share belongs to which client when multiple clients are sharing data with us. If the share name does not match the client_id, an error will be thrown. 

Once the share is identified, if the catalog does not exist, we will create it automatically. 

If the destination_table (where we will store the match output result) is not available: 

* Clone the source table. 

* Add a new column called Status and set its default value to "NEW". 

* This setup ensures that new records are automatically inserted with the status "NEW". 

* If the destination_table is available: 

* We will sync the source_table and destination_table using the primary key provided by the user. We will match records and add any that do not exist in the destination_table. 

* We will also sync records with status "ERROR" that have been updated in the source_table, updating them in the destination_table and setting their status to "NEW" for reprocessing. During the syncing between source_table and destination_table, we will save the current UTC timestamp in the table to use as a checkpoint for syncing records with status "ERROR" in the future. 

For all records in the destination_table: 

* If a record is NEW and Tags is NULL or EMPTY, update Tags to NULL if it is EMPTY. 

* If the Tags parameter is provided, update records with the provided value in Tags. 

* If a record is not processed (Processed = False), consider it for Cleansematch processing. 

We will call the MatchEnrich API based on the batch_size value, creating chunks and invoking the API with parallelism determined by batch_size. 

After processing each chunk, we will update the results back to the destination_table. 

Upon completion of this process, we will automatically trigger the Process Enrichment Data job, but only if there is data to be processed. 

These steps ensure efficient API utilization and automation based on the data processing requirements. 

.. list-table::
    :header-rows: 1

    * - Field
      - Description
    * - The catalog
      - Unity catalog name where input data is available.  
        If the mode is delta sharing, then the catalog belongs to where you want to store your output results.  
        If the mode is direct, then the catalog belongs to where your input data is available and where you want to store the output result as well.
    * - Client identifiers
      - In Delta Sharing mode, each client is uniquely identified to facilitate data sharing and management.
    * - Source schema
      - The name of the schema where the source data is available.
    * - Detailing data
      - 
    * - Destination schema
      - The name of the schema where you want to store the match output data.
    * - API URL
      - This indicates that it is necessary to call Matchbook AI APIs.
    * - Batch size
      - The batch size, which allows for parallel Matchbook API calls, must be between 1 and 50.
    * - A primary key
      - Column name from the source data that uniquely identifies each record across the entire dataset.  
        This is essential for job functions such as making updates and ensuring dataset integrity.
    * - An instance URL
      - This indicates that calling Databricks' REST APIs is necessary.

Note: This pertains to our instance, as it is running on our dataset. 

.. list-table::
    :header-rows: 1

    * - Field
      - Description
    * - Source schema
      - The name of the schema where the source data is available.
    * - Source table
      - The name of the table where the source data is available for matching.
    * - Column mapping
      - The workflow needs to call the Matchbook API with specific fields.  
        If your data does not have the exact columns required, you can create a mapping to align your columns with Matchbook AI’s conventions.
    * - Parallel API calls
      - The number of parallel Matchbook API calls to be made. The maximum allowed is 50.
    * - Scrd ID
      - Each record is associated with a source record ID for identification and tracking.

Process Enrichment Data 
-------------------------

.. figure:: images/41.png

**Note**: To create a token for Databricks, click on the right-hand side menu, select Settings, navigate to Developer, and then click on Manage next to Access Tokens.

Downloading Matched Data  
-------------------------

This job oversees the retrieval of manually reviewed and updated records from the Matchbook AI portal. It ensures that any data stewardship actions performed by users, such as corrections, approvals, or enhancements, are accurately reflected in the Snowflake environment.  

Navigate to the **Matchbook AI portal**, and from the **left-hand menu**, select **Data Stewardship**. Under this section, choose **Low Confidence** to access records that require manual review.  

.. figure:: images/42.png

The **Low Confidence** section in data stewardship is designed to help users manage records that require manual review due to lower confidence scores. These are typically cases where automated rules don't provide sufficient certainty for acceptance and need user intervention to ensure high data integrity. 

For example, if your **Auto-Acceptance** rule is configured to accept candidates with a **confidence code of 8, 9, or 10**, then any D&B candidate with a **confidence code of 7 or lower** will be routed to the low confidence queue for review. 

Within this section, users have powerful tools to **filter, accept, reject, and score** records. To begin reviewing, navigate to the **Order by Columns Equals** field and select **SrcRecordID** — this may already be pre-selected by default. Once selected, a table of candidates will be displayed. To view and interact with a specific record, simply click on the **‘+’ icon** next to it, as shown in the screen below. 

.. figure:: images/43.png

Under the **Matchbook Rank** column, you will see the **score percentage**. Based on the **highest score**, click on the **black rectangle** under the **Match** column, as shown below. Once selected, it will turn **green**, indicating that the record has been chosen. 

.. figure:: images/44.png

Now, click on **Update** on the top-right to update the selected record. 

.. figure:: images/44.png

Click on **Dashboard** from the **left-hand menu**. Under **Background Process Statistics**, you will see the **execution time** taken to complete the process. 

.. figure:: images/45.png

After manually performing data stewardship for low-confidence matching candidates in the MatchbookAI portal, we wait for the batch process to upload the manually matched data to the backend.  

Only after this step, we run the Download Matched Data workflow. This ensures that all updated matches, including those manually curated, are included in the download.

.. figure:: images/46.png

Following the enrichment process, we then download the matched data. For example, if a specific record previously had no matches, it may now have multiple matched candidates, and a corresponding DUNS number will be generated as part of the enriched output.  

Monitoring
===========

By integrating the Monitoring workflow with the Databricks environment, organizations can efficiently automate the process of tracking and managing data operations. This setup involves invoking two critical APIs—one for Download Monitoring Notifications and another for Download Transfer DUNS output. These API calls are orchestrated through a Databricks Workflow, which triggers the execution of a notebook that handles the API logic and writes the resulting data into designated Delta tables. This enables real-time visibility into data movements and provides clients with structured outputs that can be leveraged for further processing, auditing, or analytics.  

For the Databricks implementation, the solution is organized into three main components:  

1. **Integration Testing**: This component is responsible for securely managing the credentials required to make API calls. Using Databricks Secret Scopes, we store and retrieve critical values such as the client ID, API key, API secret, and the backend API endpoint (i.e., the Matchbook server API). These secrets are accessed within the notebook during execution, allowing the workflow to authenticate securely and interact with the APIs.  

On the Databricks dashboard, click **Workflows** from the left-hand menu, then select the job you want to run. 

.. figure:: images/47.png

Click on + **Add task**.

.. figure:: images/48.png

From the right-hand menu, click on Run now as shown in the screen below: 

.. figure:: images/49.png

The monitoring task runs successfully and displays a **"Succeeded"** message upon completion. 

.. figure:: images/50.png

2. **Monitoring Notebook**: This notebook contains the main logic of the monitoring workflow. It consists of three key functions: 

* **Access Token Generation**: This function generates an **access token** that is required to authenticate API calls with the Matchbook server. 

 * **Download Monitoring Notification API Call**: Once the access token is obtained, the second function uses it to call the **DownloadMonitoringNotification API**. The response, typically a large JSON payload, is written into the first target Delta table. 

 * **Download Transfer DUNS API Call**: The third function makes a call to the **DownloadTransferDUNS API**, and the response from this call is written into a second Delta table. 

3. **Workflow Execution and Testing**:  

From the left-hand menu, select **SQL Editor**, then click the **Run All** button. This will execute the queries sequentially, one by one.

.. figure:: images/51.png

Once the workflow is executed: 

When the first command is run, the **DownloadMonitoringNotification API** call runs successfully using the access token. The JSON response is written directly to the DOWNLOAD_MONITORING_OUTPUT Delta table.   

.. figure:: images/52.png

When the second command is run, the **DownloadTransferDUNS API** call follows, and its response is written into the DOWNLOAD_TRANSFER_DUNS_OUTPUT Delta table.

.. figure:: images/53.png

The output tables are:  

1. **DOWNLOAD_MONITORING_OUTPUT**: This table contains the full JSON payload from the first API.

2. **DOWNLOAD_TRANSFER_DUNS_OUTPUT**: This table contains the response from the second API call.
