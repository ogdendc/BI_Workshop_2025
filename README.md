

# Databricks Hands-on AI/BI Introductory Workshop

## Workshop Overview
Welcome to the Databricks Hands-on AI/BI Introductory Workshop!

A fictional Wealth management and Investment advisory firm that makes money by providing 2 broad service categories (advisory and brokerage services). Though the data in this workshop may not be applicable to your specific business, the techniques you will learn can be applied to any industry.

This interactive workshop will guide you through essential data analysis and BI workflows, with a progression from data exploration and dashboards to advanced analytics features. You'll work directly with Databricks notebooks, tables, dashboards, and AI-powered tools.

### Session Outline
- Data Ingestion and Processing
- Data Exploration and Visualization
- Dashboards and Genie Spaces

---

## Workshop Setup Checklist
### Pre-Workshop Resources
- **Schema**: A catalog has automatically been created for you with your username. You should have read and write permissions to your personal schema in Unity Catalog.
- **Datasets**: Datasets have been uploaded to your Unity Catalog volume under `main.default.volume`, mentioned above. You should have the following datasets in your Volume:
  - CSV File: `asset_trend_data.csv` (this represents a fact table)
  - CSV File: `branch_market_region_metro.csv` (this represents a dimensions table)
  - Parquet File: `alpha_vantage_news_subset.snappy.parquet` (this represents some unstructured data scraped from websites)
- **Code Files**: Code files have been uploaded to your Databrick workspace folder. You should see the following code files in your workspace directory:
  - Notebook: `Bronze EDA`
  - SQL Query: `create metro map`
  - SQL Query: `create asset trend silver`
  - SQL Query: `create asset trend gold 1`
  - SQL Query: `create alpha vantage news table`
  - SQL Query: `AI functions on market news`

---

## Section 1: Notebooks, Data Ingestion & Analysis
In this section we will:
- Read CSV data from your uploaded Volume.
- Perform exploratory analysis.
- Save cleansed data as managed tables.

### Data Exploration
1. Open the notebook **“Bronze EDA”**
2. Clear any existing notebook outputs so you can run every step interactively and see your results fresh.
3. Fill in the widgets at the top of the notebook with the catalog, schema from Unity Catalog, and give the output table a name such as `asset_trend_bronze`.
4. As you work through the notebook:
   - Try both Python and SQL code cells.
   - Practice creating new cells and choosing between code and markdown.
   - Attach your notebook to compute to run code.
   - Experiment with collaboration features and sharing options.

### Explore Databricks’ AI Capabilities
- Explore the **Databricks Assistant**—use its chat for questions and edit mode for code generation or modification.

---

## Catalog Explorer Exploration
- Navigate to **Catalog Explorer** and find the table we just created.
- Accept AI suggested table description.
- Have AI generate column descriptions.
- Check out **Sample Data** tab
- **Permissions** (we should have `SELECT` on the table we created)
- Explore the **Lineage, Insights, and Quality** tabs.

> **Note on Lineage, Insights, and Quality tabs in Unity Catalog:**
> Since we don’t have any pipelines or queries created from this table, these tabs are empty. We will return to them later.

---

## Querying & Table Creation
1. Create a **Query** from the dropdown
2. In the **Query Editor**:
   - Open the **“create metro map”** query. This code references the `branch_market_region_metro.csv` file that was uploaded to your Unity Catalog Volume. Modify and populate the SQL parameters with appropriate catalog, schema, table values, and run the query.
   - Next, open the **“create asset trend silver”** query. This code references the tables created in the two prior steps. Populate the SQL parameters with appropriate catalog, schema, and `table_out` values. Check to make sure that the `FROM` part of the query is referencing the same table names you created in the two steps above, and run.
   - Next, open the **“create asset trend gold 1”** query. This code references the silver table created in the prior step. Populate the SQL parameters with appropriate catalog, schema, `table_in`, and `table_out` values. The `table_in` should reference the silver table created in previous step, and the `table_out` is a new table that will be created. Populate `table_out` with `asset_trend_gold_01`. Check to make sure that the parameters (catalog, schema, etc) are correctly populated, and run.
3. Navigate to the **Catalog Explorer**, and find the schema where your output tables are being stored.
4. Select the **`asset trend silver`** table. Have the AI tool create a table description and column descriptions.
5. Take a look at the **Lineage** for the `asset_trend_silver` data. This shows automatically captured relationships between datasets as we build queries and pipelines in Databricks, which is a very important data lineage and auditing tool.
6. With the `asset trend silver` table selected, use the drop-down in the upper right to **create a new query**. Give the new query a name such as **`auc trend insights 1`**. Use the Assistant (in Agent mode) to generate the SQL for you, generating some type of insights from the data that you’re interested in. Save your query.
7. Next, open the **“create alpha vantage news table”** query. This code references a parquet file uploaded to a Volume; and this file contains unstructured data from news articles (about companies) scraped from websites. Populate the SQL parameters with appropriate catalog, schema, and table name. Name the output table `alpha_vantage_news`. Run the query.
8. Next, let’s generate insights from these datasets…

### AI Functions
- Open the **“AI functions on market news”** query. Modify the SQL parameters with appropriate `catalog.schema` values, and run.  
  *Please note: this code references a hard-coded table name of `alpha_vantage_news`. If you named that table (created in previous step) something other than that, you’ll need to modify the code accordingly.*
- Discuss the output while referencing the AI functions docs and discussing other AI functions (e.g. the `ai_forecast`).

---

## Section 3. Genie Spaces
### What is a Genie Space?
Genie Spaces allow data analysts and consumers to ask business questions in plain english which then get turned into analytical queries and executed on top of their own business data. You can ask questions and generate visualizations to understand your data better. This provides an additional way for users to get the insights they need on top of the ready-made visualizations a dashboard might offer.

The best part is, Genie's semantic knowledge continuously updates as your data changes and as users pose new questions, which means your insights evolve as your data and usage evolves.

Let’s create a Genie space to give our dashboard users another way to glean insights from our data. In Databricks, each Dashboard can be published with its own Genie space which means all the work that we just did in creating aggregate datasets, visualizations, and filters can be used by the Genie Space without much additional work needed from us.

1. Navigate to the Catalog where the `asset_trend_silver` resides.
2. While browsing the `asset_trend_silver` data, select **Genie space** from the **Create** dropdown in the upper right.
3. Select **Settings** and give the space a Title and a proper Description, such as:
   - **Title**: `Asset Trend text2sql Analyzer`
   - **Description**: `A self-service space to gain insights from data related to account services, organized by month and categorized by various service types and groups. It includes metrics such as total accounts, assets under management, and their distribution across different investment types.`
   - **Sample question**: `Which service type for brokerage accounts had the most total AUC in 2023?`
   - **Save** these settings.
4. Start a new chat and select that new sample question and observe the answer. Take a look at the SQL generated. Talk about the thumbs-up feature, etc.
5. Ask `show a monthly trend of average equity AUC per account for FA Managed services`
6. Talk about how an admin (and a focus group) should “kick the tires” and refine a Genie space before making it available to the masses. Talk about how the Genie space may need some “teaching”. For example…
   - Ask this question: `show a trend of all securities`
   - Observe the answer and discuss. Now…
   - Add a **Text instruction** of `for any questions about ‘securities’ you should include all AUC except Cash`
   - Start a new chat and re-ask the same question.
7. Ask this question: `show total AUC by state for 2024`. Observe the results and talk about how Genie doesn’t know what the ‘state’ is.
8. Add this statement to the **Instructions** under **Settings**:  
   `for any questions about 'state' use the value of the 'metro' column to create a mapping of metros to U.S. states based on your own knowledge of how those metros are associated with particular states.`
9. Start a new chat and ask the same `show total AUC by state for 2024` question.
10. Notice how so very few states are represented.
11. Talk about adding sample SQL Queries to the Instructions tab.
12. Quick sidebar about using the Databricks Assistant in Edit mode to generate and test a new SQL query to better handle how to answer questions about ‘state’.
13. Navigate to the Catalog where the `asset_trend_silver` resides.
14. **Create Query**
15. Open the Assistant in **Edit** mode and prompt as follows:  
    `using the distinct values of 'metro' from the asset_trend_silver data, create a case-when statement that maps the values of metro to 'states' where the states are a fairly comprehensive list of populous U.S. states. you should use at least 40 of the 50 U.S. states`
16. Copy the resulting **CASE WHEN** statement:

```sql
CASE
  WHEN metro IN ('New York', 'Buffalo', 'Rochester', 'Albany') THEN 'New York'
  WHEN metro IN ('Los Angeles', 'San Francisco', 'San Diego', 'Sacramento', 'San Jose') THEN 'California'
  WHEN metro IN ('Chicago', 'Peoria', 'Springfield') THEN 'Illinois'
  WHEN metro IN ('Houston', 'Dallas', 'Austin', 'San Antonio', 'El Paso') THEN 'Texas'
  WHEN metro IN ('Miami', 'Orlando', 'Tampa', 'Jacksonville') THEN 'Florida'
  WHEN metro IN ('Philadelphia', 'Pittsburgh', 'Harrisburg') THEN 'Pennsylvania'
  WHEN metro IN ('Phoenix', 'Tucson', 'Mesa') THEN 'Arizona'
  WHEN metro IN ('Atlanta', 'Savannah', 'Augusta') THEN 'Georgia'
  WHEN metro IN ('Boston', 'Worcester', 'Springfield') THEN 'Massachusetts'
  WHEN metro IN ('Detroit', 'Grand Rapids', 'Lansing') THEN 'Michigan'
  WHEN metro IN ('Seattle', 'Spokane', 'Tacoma') THEN 'Washington'
  WHEN metro IN ('Denver', 'Colorado Springs', 'Aurora') THEN 'Colorado'
  WHEN metro IN ('Minneapolis', 'St. Paul', 'Duluth') THEN 'Minnesota'
  WHEN metro IN ('Charlotte', 'Raleigh', 'Greensboro') THEN 'North Carolina'
  WHEN metro IN ('Indianapolis', 'Fort Wayne', 'Evansville') THEN 'Indiana'
  WHEN metro IN ('Columbus', 'Cleveland', 'Cincinnati') THEN 'Ohio'
  WHEN metro IN ('Baltimore', 'Annapolis') THEN 'Maryland'
  WHEN metro IN ('Portland', 'Salem', 'Eugene') THEN 'Oregon'
  WHEN metro IN ('Las Vegas', 'Reno') THEN 'Nevada'
  WHEN metro IN ('Nashville', 'Memphis', 'Knoxville') THEN 'Tennessee'
  WHEN metro IN ('Louisville', 'Lexington') THEN 'Kentucky'
  WHEN metro IN ('Milwaukee', 'Madison', 'Green Bay') THEN 'Wisconsin'
  WHEN metro IN ('Kansas City', 'St. Louis', 'Springfield') THEN 'Missouri'
  WHEN metro IN ('Oklahoma City', 'Tulsa') THEN 'Oklahoma'
  WHEN metro IN ('New Orleans', 'Baton Rouge') THEN 'Louisiana'
  WHEN metro IN ('Salt Lake City', 'Provo') THEN 'Utah'
  WHEN metro IN ('Richmond', 'Virginia Beach', 'Norfolk') THEN 'Virginia'
  WHEN metro IN ('Albuquerque', 'Santa Fe') THEN 'New Mexico'
  WHEN metro IN ('Birmingham', 'Montgomery', 'Mobile') THEN 'Alabama'
  WHEN metro IN ('Des Moines', 'Cedar Rapids') THEN 'Iowa'
  WHEN metro IN ('Omaha', 'Lincoln') THEN 'Nebraska'
  WHEN metro IN ('Honolulu') THEN 'Hawaii'
  WHEN metro IN ('Anchorage') THEN 'Alaska'
  WHEN metro IN ('Charleston', 'Huntington') THEN 'West Virginia'
  WHEN metro IN ('Providence') THEN 'Rhode Island'
  WHEN metro IN ('Manchester', 'Nashua') THEN 'New Hampshire'
  WHEN metro IN ('Portland', 'Bangor') THEN 'Maine'
  WHEN metro IN ('Little Rock', 'Fayetteville') THEN 'Arkansas'
  WHEN metro IN ('Boise') THEN 'Idaho'
  WHEN metro IN ('Jackson', 'Gulfport') THEN 'Mississippi'
  WHEN metro IN ('Columbia', 'Charleston') THEN 'South Carolina'
  WHEN metro IN ('Sioux Falls', 'Rapid City') THEN 'South Dakota'
  WHEN metro IN ('Fargo', 'Bismarck') THEN 'North Dakota'
  WHEN metro IN ('Wichita', 'Topeka') THEN 'Kansas'
  ELSE 'Other'
END AS state
```

17. Go back to the Genie space and add that CASE WHEN as a sample SQL with a description of **“use this for any questions about ‘state’”**
18. Save.  
19. Go into the **Text instructions** and remove the instruction about how to answer questions about ‘state’.
20. Save
21. Open a New chat and re-ask, yet again, this question: `show total AUC by state for 2024`
22. Ask `show the total AUC by metro associated with state = “Other”`.
23. Discuss the need for an iterative process of curating a Genie space to make it more intelligent.
24. Review the **Monitoring** tab & the potential for **Evaluation** set of questions.

### In the Data tab, discuss:
- The value of the **Value dictionary**, option of Genie-space-specific column metadata, **Synonyms**, etc
- Mention ability to **upload a CSV or Excel** file for additional data (e.g. lookups, experimental or ad hoc stuff, etc).

### Discuss:
- Publishing our dashboard with this curated Genie space integrated.
- Genie conversation API.
- Integrating this Genie space into external applications, or in a Databricks App, etc.
- Using this Genie space as an Agent within Databricks or as a UC-hosted MCP server, etc.
- Producers vs Consumers — Databricks One

---

## Section 4: Dashboards
### Create and customize dashboards
1. Click **Create** from the `asset_trend_silver` dataset in the Catalog Explorer and select the **Dashboard** option.
2. Rename the dashboard and description.
3. Rename the page **“Service Type Analysis”.**
4. Edit the default markdown cell with a better description that better suits the content of the dataset you’re working with.
5. Create a new visualization and ask Databricks Assistant for **“line graph showing total Total_AUC by Month”**. Once a visualization has been created, go into the side panel for the widget and add the **Forecast** option. Discuss what the forecasting function does, how it applies to the data at hand and its usefulness.

#### Edit the Widget panel on the right for the AUC by Month forecast trend:
- Select **Description** (next to Title).
- In the visual, edit the Description (e.g. “(cool forecasting via ai_forecast function)”).
- Delete the other Total AUC trend chart that lacks the forecast.

#### Give the Table at the bottom of the dashboard a name:
- Select the table. On the right in the Widget panel, select **Title** and **Description**.
- In the table, where it says ‘Widget Title’ type a new title such as **“Detailed Table”**.

#### Navigate to the dashboard’s **Data** tab
- Create a new Dataset by selecting **From Data:** → **Create from SQL**.
- Name the dataset **“Distinct Service Types”** and run the following query to create the dataset:

```sql
SELECT DISTINCT Account_Service_Type
FROM <your_catalog>.<your_schema>.asset_trend_silver
```

### Add a filter to the Dashboard
- File the global filters for the dashboard.
- In the global filter dropdown, edit the title of the Filter to: **“select Service Type”**
- **Filter**: Single Value
- **Fields**: Distinct Service Types → `Account_Service_Type`
- Click the drop-down and select a value, e.g. **“Advisory Fund”**. Notice how nothing changes on the charts (we’ll actually apply this filter below).

#### Go back to the Data tab:
- Modify the `asset_trend_silver` data to add a query parameter:

```sql
WHERE Account_Service_Type = :service_type
```

- Populate the default value of the query parameter widget with **`FA Managed`** (without quotes) and **Run** the query.
- Back to the Dashboard page, notice how the **‘Detailed Table’** now has been filtered per the value you selected in the Data tab.

#### While we’re in the Data tab, let’s also examine the code for the forecasted dataset that was created for us by Databricks. It should be called **“asset_trend_silver (Forecasted)”**
The beginning of the current forecast code should look something like this:

```sql
WITH original_table AS (
  SELECT
    DATE_TRUNC("MONTH", `Month`) AS Month,
    SUM(`Total_AUC`) AS Total_AUC
  FROM (
    SELECT * FROM <your_catalog>.<your_schema>.asset_trend_silver
  ) GROUP BY 1
```

Edit the query so that your new code looks like this:

```sql
WITH original_table AS (
  SELECT
    DATE_TRUNC("MONTH", `Month`) AS Month,
    SUM(`Total_AUC`) AS Total_AUC
  FROM (
    SELECT * FROM <your_catalog>.<your_schema>.asset_trend_silver
    WHERE Account_Service_Type = :service_type
  ) GROUP BY 1
```

- In the `service_type` at the bottom, type in **“Advisory Fund”** (without quotes) and run the query just to make sure it works.

### Cross and Dynamic Filtering
What we’re doing here is inserting a query level parameter across all the queries in the dashboard. This way, when the global filter is applied, it cross-filters across all of the underlying Datasets that power the dashboard which will automatically filter the tables and visualizations we have in the dashboard.

#### Back to the dashboard
- Modify the **‘select Service Type’** global filter’s Widget panel to parameterize both of the Silver datasets we’re working with:
  - Select **Parameters → asset_trend_silver → service_type**
  - Select **Parameters → asset_trend_silver (Forecasted) → service_type**
- Select various values for the filter dropdown, to observe that now both visuals in our table are dynamically changing!

### Discuss best practices for dashboard usability
Reflect back on the 3 different types of filters you added:
- **Global dashboard filters**
- **Query parameters**
- **Visualization filters**

This is a good time to discuss the different scopes of each type of filter and when to use which filtering capabilities for the best dashboard experience.

---

## Manually Creating Visualizations
So far, we’ve created visualizations through Databricks Assistant prompts. Let’s now try to add a visualization manually.

### Let’s create a Bar Chart.
- **Dataset**: `asset_trend_silver`
- **X-axis**: `region_name`
- **Y-axis**: `AVG` of `Total_AUC_per_acct`

> Note that the ‘select Service Type’ filter automatically works because we already applied that filter to the Data being used for this bar chart.

- Select one of the columns in the bar chart and see how the **‘Detailed Table’** automatically reflects that cross-filter.

#### A bit of housekeeping on this bar chart:
- Select **Title** and name it **“Avg Total AUC by Region”**
- Select the 3-dots menu next to **Widget** in the widget pane on the right, for the bar chart, and change the title to be **center-aligned** and change the **color** of the font.
- Select the 3-dots menu for the **X axis** and change the ‘show axis title’ to **‘Region’**.
- Select the 3-dots menu for the **Y axis** and:
  - Change ‘show axis title’ to **‘Avg Total AUC per Account’**
  - Select **‘Format’**, **Custom**, **$**
- In the Widget panel, select **‘Description’** and edit the description in the visualization to say **“(all time periods)”**.

### Now let’s clone this Bar chart:
- With the bar chart selected, click the 3-dots menu in the visualization, and **Clone**.
- Edit the **Description** to say **“(latest time period only)”**.
- In the Widget panel, select **+** next to **‘Filter fields’**, select **`Current_Flag`** and select **1** in that filter drop-down.
- Select the 3-dots menu next to **‘Widget’** in the Widget panel and change the **font color** to differentiate from the original version of this bar chart.

Keep exploring different options for different types of visualizations, customizing the axes, and data formats. Play around with themes, colors and other aspects of the visualizations to give the dashboard an experience appropriate for the intended target audience.

---

Let’s step away from our Dashboard for a minute and create a dataset from a standalone Query using the **SQL Query Editor** (this is different from the Data tab of the Dashboard).

> **Note:** If this step was already done in previous module and this gold output table has already been created, you can skip to the next bullet. Otherwise…let’s create a pre-aggregated Gold table:

In the Query editor, select **“new query”**, name it **‘create asset trend gold 1”** using the following SQL code:

```sql
CREATE OR REPLACE TABLE
 IDENTIFIER(:catalog || '.' || :schema || '.' || :table_out)
AS
SELECT
 Account_Service_Group,
 Account_Service_Type,
 Account_Tax_Category,
 SUM(Total_Accounts) AS total_accounts,
 SUM(Total_AUC) AS total_auc,
 SUM(Equity_AUC) AS equity_auc,
 SUM(Cash_AUC) AS cash_auc,
 SUM(MutualFund_AUC) AS mutualfund_auc,
 SUM(Other_AUC) AS other_auc
FROM
 IDENTIFIER(:catalog || '.' || :schema || '.' || :table_in)
GROUP BY
 Account_Service_Group,
 Account_Service_Type,
 Account_Tax_Category
ORDER BY
 Account_Service_Group,
 Account_Service_Type,
 Account_Tax_Category;
SELECT * FROM IDENTIFIER(:catalog || '.' || :schema || '.' || :table_out);
```

- Populate the widgets (catalog, schema, etc) as appropriate. The “silver” table is the same as what was created in previous steps.
- Call the `table_out` **“asset_trend_gold_01”**. **Run All**.
- Consider the aggregations that have been applied to create this dataset and examine the aggregated values in the returned results table. What other aggregations could you apply on this dataset ahead of time for downstream analytics and visualization?

Now that we’ve created our Gold dataset, let’s go back to our dashboard.

- On the **Data** tab, select **‘Add data source’**, and point to this new Gold table using the search functionality.
- Add the same filter via parameter logic to this code:

```sql
SELECT * FROM <your_catalog>.<your_schema>.asset_trend_gold_01
WHERE Account_Service_Type = :service_type
```

- Populate the value of `service_type` with **“Advisory UMA”** (without quotes) and **Run**.

### In the dashboard, add a new visualization.
In the Widget settings on the right panel:
- Select the **“asset_trend_gold”** data from the Dataset dropdown
- Change the **Visualization** type to **Pie**.
- **Angle** = `total_accounts` (transform = None), change the **Display name**.
- **Color** = `Account_Tax_Category`, change the **Display name**
- Select **Title** and call this **‘Accounts by Tax Category – All Groups’**

> **Remember:** we need to modify our “Service Type” filter if we want it to impact values in this Pie chart.
> To do so, select the filter, in the panel on the right and add the correct field to **Parameters**.

### Now, let’s create a version of this Pie chart that only applies to the **‘Brokerage’** service group.
- Click the 3-dots menu on the pie chart → select **Clone**.
- On the cloned visualization, add a **Filter field** of `Account_Service_Group`, and hand type in **Brokerage**.
- Add Title **‘Accounts by Tax Category – Brokerage Only’**
- Then go and select different service types in the global filter and notice how the different visualizations are filtered according to the interaction between the global, datasets and visualization-level filters that we have applied.

If your results look different than your neighbors, this is a good time to pause and discuss what you may have done differently.

---

This brings us to the end of our hands-on workshop covering data analysis using Databricks SQL, AI/BI dashboards and Genie Spaces.

## Additional Topics
- **Jobs & Pipelines** – scheduling the refresh/creation of tables + dashboard refresh.
- **AutoML** – e.g. predicting changes in AUC based on shifts in macroeconomics; and/or forecasting
- **Agent Bricks** – KA from 10Ks, Genie for structured, then MAS
- **Apps** – e.g. custom EJ app using MAS
- **Databricks One**

If you have any questions, don’t hesitate to reach out to your instructors. More information is always available in Databricks Academy, Databricks Tutorials and Databricks Docs.

Thank you for your time and attention in this workshop!

---

## The queries used in this workshop

- **Bronze EDA.ipynp** – to be shared prior and uploaded by each user from their local machine.

### create metro map
*creating a table that maps our branches to Metros — change the Volume path accordingly and populate the SQL variables for desired output location catalog, schema, table name*

```sql
CREATE OR REPLACE TABLE IDENTIFIER(:catalog || '.' || :schema || '.' || :table) AS
-- ogden_demos.bi_workshop.branch_metro_map
SELECT * FROM read_files(
  '/Volumes/ogden_demos_useast1_catalog/bi_workshop/files_uploaded/branch_market_region_metro.csv',
  format => 'CSV',
  header => true,
  schemaEvolutionMode => 'none' -- omit the rescueddata column
);
SELECT * FROM IDENTIFIER(:catalog || '.' || :schema || '.' || :table);
```

### create asset trend silver
*joining the metro designations onto the branch trend data — manually modify the catalog.schema.table(s) accordingly*

```sql
CREATE OR REPLACE TABLE IDENTIFIER(:catalog || '.' || :schema || '.' || :table_out) AS
-- ogden_demos.bi_workshop.asset_trend_silver
SELECT   
    a.*,  
    b.metro,  
    b.region_name  
FROM IDENTIFIER(:catalog || '.' || :schema || '.' || "asset_trend_bronze")  a  
LEFT JOIN   
     IDENTIFIER(:catalog || '.' || :schema || '.' || "branch_metro_map")  b  
ON   
    a.Branch_Market = b.branch_market  
    AND a.Branch_Region = b.branch_region    
;  
SELECT * FROM IDENTIFIER(:catalog || '.' || :schema || '.' || :table_out);
```

### create asset trend gold 1

```sql
CREATE OR REPLACE TABLE IDENTIFIER(:catalog || '.' || :schema || '.' || :table_out) AS
-- ogden_demos.bi_workshop.asset_trend_gold
SELECT  Account_Service_Group,  
        Account_Service_Type,  
        Account_Tax_Category,  
        SUM(Total_Accounts)           AS total_accounts,  
        SUM(Total_AUC)                AS total_auc,  
        SUM(Equity_AUC)               AS equity_auc,  
        SUM(Cash_AUC)                 AS cash_auc,  
        SUM(MutualFund_AUC)           AS mutualfund_auc,  
        SUM(Other_AUC)                AS other_auc  
FROM IDENTIFIER(:catalog || '.' || :schema || '.' || :table_in)  
GROUP BY Account_Service_Group, Account_Service_Type, Account_Tax_Category  
ORDER BY Account_Service_Group, Account_Service_Type, Account_Tax_Category  
;  
SELECT * FROM IDENTIFIER(:catalog || '.' || :schema || '.' || :table_out);
```

### create alpha vantage news table

```sql
CREATE OR REPLACE TABLE IDENTIFIER(:catalog || '.' || :schema || '.' || :table) AS  
SELECT *  
FROM parquet.`/Volumes/ogden_demos_useast1_catalog/bi_workshop/files_uploaded/alpha_vantage_news_subset.snappy.parquet`;
```

### AI functions on market news

```sql
SELECT  
  text_clean,  
  ai_analyze_sentiment(text_clean)                                                                  AS sentiment,  
  ai_classify(text_clean, array('investments', 'litigation', 'AI', 'other'))                        AS news_category,  
  ai_classify(text_clean, array('bearish', 'bullish', 'neutral', 'hold'))                           AS investment_category,  
  ai_extract(text_clean, array('location', 'organization'))                                         AS entities,  
    
  ai_gen(  
        'You are a confident financial advisor. Provide very concise investment advice based on this news article: ' || text_clean  
        )                                                                                           AS investment_advice,  
  ai_mask(text_clean, array('person', 'corporation', 'entity', 'company', 'ticker'))                AS text_masked,  
  ai_summarize(text_clean, 20)                                                                      AS text_sum_20words,  
  ai_query('databricks-gpt-oss-20b',    
           CONCAT('Summarize this article:\n', text_clean))                                         AS gpt_text_sum,  
  ai_query('databricks-meta-llama-3-3-70b-instruct',    
           CONCAT('Concisely list the corporate entities in this text:\n', text_clean))             AS corporate_entities  
    
FROM IDENTIFIER(:catalog || '.' || :schema || '.' || 'alpha_vantage_news')  
WHERE ticker = "TSLA"  
LIMIT 10; 
```
