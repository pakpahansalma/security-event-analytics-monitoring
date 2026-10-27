# Security Event Analytics & Monitoring

**Security Monitoring Dashboard · Python, Elasticsearch & Kibana**

A cybersecurity event analytics and monitoring project built using Python, Elasticsearch, and Kibana.

🌐 **Language:** [English](README_eng.md) | [Bahasa Indonesia](README.md)

---

## 📌 Project Navigation

Use the links below to access the main project files and documentation.

| Component | Description | Link |
|---|---|---|
| 📓 **Jupyter Notebook** | Data preparation, cleaning, transformation, and validation | [Open Notebook](security_event_analytics.ipynb) |
| 📊 **Kibana Screenshots** | Visual documentation of the dashboard and its visualizations | [View Screenshots](Kibana%20Screenshot/) |
| ⚙️ **Elasticsearch Mapping** | Field definitions and index data types | [View `index_mapping.json`](elasticsearch/index_mapping.json) |
| 📁 **Raw Dataset** | The original dataset used for analysis | [Open `raw_data.csv`](raw_data.csv) |
| 🇮🇩 **Indonesian Documentation** | Project documentation in Indonesian | [Open README in Indonesian](README.md) |

## 1. Overview

This project develops a cybersecurity event analytics and monitoring workflow using a dataset obtained from Kaggle. The workflow covers data quality assessment, data cleaning, data transformation, indexing with Elasticsearch, and visualization through Kibana.

The dashboard provides an overview of security event severity, attack types, security actions, network protocols, anomaly scores, and event activity over time.

## 2. Project Objectives

- Assess and prepare cybersecurity event data.
- Perform documented data cleaning and transformation.
- Prepare a consistent data structure for Elasticsearch.
- Visualize cybersecurity event patterns and distributions using Kibana.
- Provide a dashboard for security event exploration and monitoring.

## 3. Technologies Used

| Technology | Purpose |
|---|---|
| Python | Data preparation, cleaning, transformation, and validation |
| Pandas | Dataset manipulation and quality assessment |
| Google Colab | Notebook development environment |
| Elasticsearch | Storage, indexing, and searching of security event documents |
| Kibana | Data visualization and dashboard monitoring |
| Kaggle | Dataset source |

## 4. Dataset

The project uses the `cybersecurity_attacks` dataset obtained from Kaggle.

### Dataset characteristics

- Original dataset: 40,000 rows.
- Original dataset: 25 columns.
- Columns selected for the Elasticsearch index: 16.
- The data represents network traffic information and cybersecurity events.

The dataset file included in this repository is [`raw_data.csv`](raw_data.csv).

The selected fields include timestamps, source and destination IP addresses, ports, protocols, packet lengths, attack types, severity levels, security actions, anomaly scores, attack signatures, packet types, traffic types, network segments, and log sources.

## 5. Project Workflow

1. Load the dataset using Python.
2. Inspect the dataset structure and data quality.
3. Perform data cleaning and consistency checks.
4. Select relevant fields for Elasticsearch.
5. Standardize field names and data types according to the index mapping.
6. Validate the transformed data before indexing.
7. Ingest the data into Elasticsearch.
8. Explore and visualize the data using Kibana.

The complete workflow is documented in [`security_event_analytics.ipynb`](security_event_analytics.ipynb).

## 6. Data Preparation & Cleaning

Data preparation was performed before indexing to improve structural consistency and support the intended security analysis.

### 6.1. Data Loading

The dataset was loaded using Pandas. The original data was retained in `df_raw`, while `df_clean` was used as the working copy for data quality assessment and cleaning.

This separation preserved a reference to the original dataset throughout the preparation process.

### 6.2. Data Quality Assessment

The following checks were performed:

- Dataset structure, row count, column count, and data types.
- Duplicate row detection.
- Categorical distributions and numerical ranges.
- IP address validation.
- Timestamp validity and range.
- Network port distributions.
- Consistency between `Traffic Type` and `Protocol`.

These checks helped identify potential data quality issues and understand the characteristics of the dataset before indexing.

### 6.3. Missing Value Analysis

Missing values were examined in several columns, including:

- `Malware Indicators`
- `Alerts/Warnings`
- `Proxy Information`
- `Firewall Logs`
- `IDS/IPS Alerts`

Missing values were not arbitrarily imputed or replaced with fabricated values.

These five columns were not included in the final 16-column Elasticsearch index. Therefore, `df_es` contains no missing values in its selected fields. This does not mean that all missing values in the original dataset were removed or repaired.

### 6.4. Timestamp Standardization

The `Timestamp` column was converted to datetime format and standardized to UTC.

Validation confirmed that all 40,000 timestamp values were successfully converted. The resulting time range was used as a reference when configuring the Kibana dashboard time filter.

### 6.5. Text and Consistency Checks

Additional checks covered whitespace, text placeholders, and categorical consistency.

Changes were applied carefully to avoid altering the meaning of the available data. According to the notebook validation results, the only column changed during the documented cleaning stage was `Timestamp`.

### 6.6. Data Cleaning Results

After the documented cleaning stage:

- The dataset retained 40,000 rows.
- `df_clean` retained all 25 columns.
- No duplicate rows were found.
- All timestamps were successfully converted.
- No rows were removed during the documented cleaning stage.

## 7. Data Transformation & Preparation for Elasticsearch

After cleaning, relevant columns were selected for analysis and dashboard development.

The selected dataset was prepared in `df_index` and then used to create `df_es`.

The transformation process included:

- Selecting 16 columns from the original 25.
- Converting column names to `snake_case`, such as `Source IP Address` to `source_ip`.
- Aligning data types with the Elasticsearch mapping.
- Validating the final dataset before indexing.

### Elasticsearch field mapping

| Field | Elasticsearch data type |
|---|---|
| `timestamp` | `date` |
| `source_ip` | `ip` |
| `destination_ip` | `ip` |
| `source_port` | `integer` |
| `destination_port` | `integer` |
| `protocol` | `keyword` |
| `packet_length` | `integer` |
| `attack_type` | `keyword` |
| `severity_level` | `keyword` |
| `action_taken` | `keyword` |
| `anomaly_score` | `float` |
| `attack_signature` | `keyword` |
| `packet_type` | `keyword` |
| `traffic_type` | `keyword` |
| `network_segment` | `keyword` |
| `log_source` | `keyword` |

The mapping definition is available in [`elasticsearch/index_mapping.json`](elasticsearch/index_mapping.json).

## 8. Data Validation & Elasticsearch Ingestion

The dataset was validated before and after indexing to check whether the ingestion process completed as expected.

| Validation | Result |
|---|---:|
| Original dataset rows | 40,000 |
| Original dataset columns | 25 |
| `df_es` rows | 40,000 |
| `df_es` columns | 16 |
| Missing values in `df_es` | 0 |
| Documents indexed in Elasticsearch | 40,000 |

The Elasticsearch index used for the project is `security-events-test`.

The document count was verified using the Elasticsearch Count API, which returned 40,000 indexed documents.

## 9. Kibana Dashboard

The **Security Event Analytics & Monitoring** dashboard provides an interactive overview of cybersecurity events in the dataset.

### Key Performance Indicators (KPIs)

- Total Security Events
- High Severity Events
- Blocked Events
- Average Anomaly Score

### Main Visualizations

1. **Attack Types Distribution** — displays the distribution of attack categories.
2. **Security Events by Severity** — shows events grouped by severity level.
3. **Security Actions Distribution** — displays the distribution of recorded security actions.
4. **Security Events Over Time** — presents event activity across the selected time range.
5. **Network Protocol Distribution** — shows the distribution of network protocols.

### Event Details

The dashboard also includes an event details table with fields such as timestamp, severity level, source IP, destination IP, attack type, action taken, anomaly score, and protocol.

These visualizations support the exploration of cybersecurity event distributions and trends.

### Dashboard Screenshots

Visual documentation is available in the [`Kibana Screenshot`](Kibana%20Screenshot/) folder.

The screenshots complement the notebook and Elasticsearch mapping by showing the resulting Kibana dashboard and its visualizations.

## 10. Repository Structure

```text
security-event-analytics-monitoring/
├── Kibana Screenshot/
│   └── [Dashboard screenshot files]
├── elasticsearch/
│   └── index_mapping.json
├── raw_data.csv
├── security_event_analytics.ipynb
├── README.md
└── README_eng.md
```

This structure reflects the repository files reported as uploaded. The screenshot filenames should match the actual files available in the GitHub folder.

## 11. Future Improvements

Potential improvements include:

- Adding more detailed visualizations for attack patterns.
- Developing anomaly detection using statistical methods or machine learning.
- Implementing alerts for selected security conditions.
- Improving pipeline reproducibility documentation.
- Automating data ingestion and monitoring.

## 12. Disclaimer

This project was developed for educational purposes, data exploration, and a cybersecurity analytics portfolio. The findings reflect the dataset used and do not necessarily represent real-world production network conditions.

## 13. Author

**Salma Cristianes Roito Pakpahan**

Areas of interest: Data Analytics | Cybersecurity Analytics | Python | Elasticsearch | Kibana
