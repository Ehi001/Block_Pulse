#  Blockpulse Crypto Data Platform

This project is an end-to-end **data engineering pipeline** that ingests cryptocurrency market data from a public API, processes it using a Medallion architecture (Bronze, Silver, Gold), and makes it available for analysis in Power BI via **Azure Synapse Analytics**.

### Project Overview:

The rapid growth of the cryptocurrency market has created a rising demand for
timely, structured, and insightful data to support investment decisions, market
research, and performance monitoring. In response to this, a newly launched crypto
data analytics startup, BlockPulse Insights, is building a cloud-based data
engineering platform to provide reliable and historical insights into the cryptocurrency
space.

### OBJECTIVE:
The Objective of this project is to use cloud resources to develop a scalable, independent and reliable infrastructure that will provide go to information on current and historical crypto data. To this end, a scalable data pipeline will be designed and implemented to ingest, process, and store cryptocurrency market data
from public APIs into a cloud-based data warehouse. Providing Analysis ready data in Azure cloud that can be accessed for analysis. Data used for this project was sourced from www.coingecko.com a source for live information on crypto trading and other related activities. 

### ABOUT COINGECKO:
Started in 2014, CoinGecko is the world's largest independent crypto data aggregator that is integrated with more than 1,000 crypto exchanges and lists more than 17,000 coins. CoinGecko API offers the most comprehensive and reliable crypto market data through RESTful JSON endpoints.
CoinGecko Pro API now serves on-chain DEX data (Beta) across 200+ blockchain networks, 1,500+ decentralized exchanges (DEXes), and 6M+ tokens, powered by GeckoTerminal. Try it out for free: GeckoTerminal Public API
Thousands of forward-thinking projects, Web3 developers, researchers, institutions, and enterprises use our API to obtain price feeds, market data, metadata, and historical data of crypto assets, NFTs, and exchanges.
Here are some of the common use cases for clients who use CoinGecko API:
*	Crypto Exchanges (CEX, DEX), Trading Apps
*	Wallets (Hot, Cold)
*	Data Aggregator, Crypto Screener, Analytics Dashboard
*	Block Explorer, Portfolio Tracker
*	DeFi Protocols, NFT Marketplaces, Digital Bank
*	Backtesting Trading Strategy
*	Accounting, Tax, Audit, HR Payroll
*	Research & Analysis: Media, Institution, Academic, VC, Financial
*	Oracles, Bots, Payments, E-commerce



> 🔧 Built with production-grade tools across Azure — Databricks, ADF, Synapse, Delta Lake, and Power BI.

---

### 🔍 High level description of pipeline stages

- **Source**: Cryptocurrency market data from public REST API (e.g., CoinGecko)
- **Ingestion**: API data fetched daily into the Bronze layer
- **Processing**: Cleaned and transformed into structured Silver and Gold layers
- **Design**: Follows the **Medallion architecture** with **Slowly Changing Dimension (SCD) Type 2** logic
- **Storage**: Data is stored in **Azure Data Lake Gen2** using the **Delta Lake format**
- **Trigger**: Pipeline is trigerred to run everyday at 8:00PM MST everyday on Azure Data Factory
- **Presentation**: Served to **Power BI** via **Azure Synapse Analytics**
---


### 📌 Architecture
![image](https://github.com/user-attachments/assets/162ad910-741a-46f3-865f-dbc99247a9a5)
 

**Layers:**

- **Bronze**: Raw API data ingested via Databricks Notebooks and ADF pipelines
- **Silver**: Transformed into structured tables (facts and dimensions)
- **Gold**: Historical fact and dimension tables with SCD logic, optimized for BI
- **Power BI**: Visual dashboard connected to consolidated Synapse views

---

## 🛠 Tech Stack

| Layer       | Tooling                              |
|-------------|--------------------------------------|
| Ingestion   | Python, REST API, Azure Data Factory |
| Processing  | PySpark on Azure Databricks          |
| Storage     | Azure Data Lake Gen2, Delta Lake     |
| Orchestration | ADF Pipelines                      |
| Modeling    | SCD Type 2, Fact & Dimension tables  |
| Serving     | Azure Synapse SQL Serverless Pools   |
| Visualization | Power BI                           |
| Security    | Azure Key Vault (for secret management) |
|Architecture & ERD| Draw.IO|
---

## 🧱 Key Components

### ✅ ETL Pipeline
- Extracts API data using Python scripts
- Loads raw JSON into Bronze folders in ADLS
- Transforms into normalized structure in Silver
- Applies SCD Type 2 logic to dimension tables in Gold
- Appends facts incrementally in Gold

### ✅ Delta Lake + SCD
- Built an SCD Type dimension tables ( `crypto_asset`)
- Tracks current and historical records using `is_current`, `start_date`, `end_date`, and `hash`

### ✅ Synapse Integration
- Created external tables from Delta files
- Used CETAS to consolidate and expose unified tables for Power BI
- Optimized for BI performance

### ✅ Power BI
- Connected directly to Synapse workspace
- Built star-schema data model with:
  - `crypto_metrics` (fact)
  - `crypto_asset` (dimension)
  - `dim_date` (dimension)

---

## 📊 Dashboard

![image](https://github.com/user-attachments/assets/53b10a0c-4b59-4e67-8fa6-844b850e5197)

![image](https://github.com/user-attachments/assets/9d5071e2-afd5-473f-a25f-673ac77c44ff)

![image](https://github.com/user-attachments/assets/16c3e9fb-d829-466f-8f3c-e53145eea261)

![image](https://github.com/user-attachments/assets/16394a52-616d-4473-bb05-8f964aac512a)

![image](https://github.com/user-attachments/assets/cb2fce14-fe6d-4d81-9496-63b57eed8ff4)

 
---

## 🚦 Pipeline Execution Flow

1. ✅ **ADF Pipeline** triggers notebook to ingest API data into Bronze
2. 🧼 **Databricks Notebooks** transform and load into Silver & Gold
3. 🗃 **Synapse** exposes cleaned tables via external tables
4. 📈 **Power BI** connects to Synapse for dashboards

---

## 🔐 Secret Management

- All secrets (e.g., storage keys, tokens) are securely stored and retrieved via **Azure Key Vault**



### Data Structure From API
|  |Column Name	                |Data Type	      |Explanation|
|--|----------------------------|-----------------|------------------------------------------------|
|1 |id	                        |string	          |coin ID                                         | 
|2 |symbol	                    |string	          |coin symbol                                     | 
|3 |name	                      |string	          | coin name                                      |                                  
|4 |image	                      |string	          |coin image url                                  |
|5 |current_price	              |number	          |coin current price in currency                  |
|6 |market_cap	                |number	          |coin market cap in currency                     |
|7 |market_cap_rank	            |number	          |coin rank by market cap                         |
|8 |fully_diluted_valuation     |number           |coin fully diluted valuation (fdv) in currency  | 
|9 |total_volume	              |number	          |coin total trading volume in currency           |
|10|high_24h	                  |number	          |coin 24hr price high in currency                |
|11|low_24h	                    |number	          |coin 24hr price low in currency                 |
|12|price_change_24h	          |number	          |coin 24hr price change in currency              |
|13|price_change_percentage_24h	|number	          |coin 24hr price change in percentage            |  
|14|market_cap_change_24h	      |number	          |coin 24hr market cap change in currency         |
|15|market_cap_change_percentage_24h|number	      |coin 24hr market cap change in percentage       |
|16|circulating_supply	        |number	          |coin circulating supply                         |
|17|total_supply	              |number	          |coin total supply                               |
|18|max_supply	                |number	          |coin max supply                                 |
|19|ath	                        |number      	    |coin all time high (ATH) in currency            |
|20|ath_change_percentage	      |number	          |coin all time high (ATH) change in percentage   |
|21|ath_date	                  |date-time	      |coin all time high (ATH) date                   |
|22|atl	                        |number	          |coin all time low (atl) in currency             |
|23|atl_change_percentage	      |number	          |coin all time low (atl) change in percentage    |
|24|atl_date	                  |date-time	      |coin all time low (atl) date                    |
|25|roi	                        |string	          |last_updated                                    |
|26|last_updated	              |date-time	      |coin last updated timestamp                     |

---

### API RAW DATE SCHEMA
  ![image](https://github.com/user-attachments/assets/8c782276-7468-458b-be56-3d226c50a71c)
---
### TRANSFORMED DATA 
The final ERD is shown below, it’s a star schema that has a fact table (crypto_metrics) and two dimension tables (crypto_asset and date)

![image](https://github.com/user-attachments/assets/87d2ebe9-bb00-464d-92f8-2028b6074c2f)
---
AZURE SYNAPSE SCREEN SHOT
![image](https://github.com/user-attachments/assets/25b3e0c2-42e0-485c-8966-118c9bde98c8)
 
AZURE DATA FACTORY SCREEN SHOT
![image](https://github.com/user-attachments/assets/15537b8f-5b0f-4a1c-b963-88b5eb2d54b6)
 

