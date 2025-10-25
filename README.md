# 🏦 Largest Banks ETL Pipeline

> An automated ETL pipeline that extracts, transforms, and loads data about the world's largest banks by market capitalization.

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/soulaymanaabaid123/largest_banks_pipeline/graphs/commit-activity)

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Technologies Used](#-technologies-used)
- [Project Architecture](#-project-architecture)
- [Installation](#-installation)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Data Pipeline](#-data-pipeline)
- [Sample Output](#-sample-output)
- [Logging](#-logging)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

## 🎯 Overview

This project implements a complete **ETL (Extract, Transform, Load)** pipeline that:
- 📊 Extracts data about the top 10 largest banks worldwide from Wikipedia
- 💱 Transforms market capitalization values from USD to multiple currencies (GBP, EUR, INR)
- 💾 Loads the processed data into both CSV files and SQLite database
- 📝 Maintains comprehensive logs of all operations

Perfect for financial analysts, data engineers, and anyone interested in automated data processing pipelines!

## ✨ Features

- 🌐 **Web Scraping**: Automated data extraction from Wikipedia using BeautifulSoup
- 🔄 **Multi-Currency Conversion**: Converts USD to GBP, EUR, and INR using real exchange rates
- 🗄️ **Dual Storage**: Saves data in both CSV format and SQLite database
- 📊 **SQL Querying**: Built-in queries for data analysis
- 📝 **Comprehensive Logging**: Timestamped logs for every operation
- 🔁 **Reusable**: Designed for quarterly or periodic execution
- ⚡ **Efficient**: Optimized for performance with pandas

## 🛠️ Technologies Used

| Technology | Purpose |
|------------|---------|
| ![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) | Core programming language |
| ![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white) | Data manipulation and analysis |
| ![SQLite](https://img.shields.io/badge/SQLite-07405E?style=flat&logo=sqlite&logoColor=white) | Database storage |
| ![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup-43B02A?style=flat) | Web scraping |
| ![Requests](https://img.shields.io/badge/Requests-FF6F00?style=flat) | HTTP library |

## 🏗️ Project Architecture

```
┌─────────────────┐
│   Wikipedia     │
│   (Data Source) │
└────────┬────────┘
         │ Extract
         ▼
┌─────────────────┐      ┌──────────────┐
│  BeautifulSoup  │      │ Exchange     │
│  Web Scraper    │◄─────┤ Rates CSV    │
└────────┬────────┘      └──────────────┘
         │ Transform
         ▼
┌─────────────────┐
│  Pandas         │
│  DataFrame      │
└────────┬────────┘
         │ Load
         ▼
┌─────────────────┐      ┌──────────────┐
│  CSV File       │      │  SQLite DB   │
│  Output         │      │  (Banks.db)  │
└─────────────────┘      └──────────────┘
```

## 📦 Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/soulaymanaabaid123/largest_banks_pipeline.git
   cd largest_banks_pipeline
   ```

2. **Create a virtual environment** (recommended)
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install required packages**
   ```bash
   pip install -r requirements.txt
   ```

   Or install manually:
   ```bash
   pip install pandas beautifulsoup4 requests numpy
   ```

## 🚀 Usage

### Basic Execution

Run the main ETL pipeline:

```bash
python banks_etl.py
```

### Expected Workflow

1. **Extract**: Scrapes data from Wikipedia
2. **Transform**: Converts USD to GBP, EUR, INR
3. **Load**: Saves to CSV and SQLite database
4. **Query**: Executes sample SQL queries
5. **Log**: Records all operations with timestamps

### Configuration

Edit the configuration section in `banks_etl.py`:

```python
url = 'https://web.archive.org/web/20230908091635/https://en.wikipedia.org/wiki/List_of_largest_banks'
exchange_rate_path = 'exchange_rate.csv'
db_name = 'Banks.db'
table_name = 'Largest_banks'
output_csv_path = 'Largest_banks_data.csv'
```

## 📁 Project Structure

```
largest_banks_pipeline/
│
├── banks_etl.py              # Main ETL script
├── exchange_rate.csv         # Currency exchange rates
├── requirements.txt          # Python dependencies
├── README.md                 # Project documentation
│
├── output/
│   ├── Largest_banks_data.csv    # Transformed data (CSV)
│   └── Banks.db                  # SQLite database
│
└── logs/
    └── code_log.txt              # Execution logs
```

## 🔄 Data Pipeline

### 1️⃣ Extract Phase

```python
def extract(url, table_attribs):
    """
    Extracts bank data from Wikipedia
    Returns: DataFrame with Name and MC_USD_Billion
    """
```

**Data Source**: [Wikipedia - List of Largest Banks](https://en.wikipedia.org/wiki/List_of_largest_banks)

### 2️⃣ Transform Phase

```python
def transform(df, exchange_rate_path):
    """
    Converts market cap to multiple currencies
    Adds columns: MC_GBP_Billion, MC_EUR_Billion, MC_INR_Billion
    """
```

**Exchange Rates** (from CSV):
- USD to GBP
- USD to EUR
- USD to INR

### 3️⃣ Load Phase

```python
def load_to_csv(df, output_path):
    """Saves DataFrame to CSV file"""

def load_to_db(df, sql_connection, table_name):
    """Loads DataFrame into SQLite database"""
```

## 📊 Sample Output

### CSV Output (`Largest_banks_data.csv`)

| Name | MC_USD_Billion | MC_GBP_Billion | MC_EUR_Billion | MC_INR_Billion |
|------|----------------|----------------|----------------|----------------|
| JPMorgan Chase | 432.92 | 346.34 | 402.62 | 35910.71 |
| Bank of America | 231.52 | 185.22 | 215.31 | 19204.58 |
| ... | ... | ... | ... | ... |

### SQL Queries

The pipeline executes these queries:

```sql
-- Get all banks
SELECT * FROM Largest_banks;

-- Calculate average market cap in GBP
SELECT AVG(MC_GBP_Billion) FROM Largest_banks;

-- Get top 5 banks
SELECT Name FROM Largest_banks LIMIT 5;
```

## 📝 Logging

All operations are logged with timestamps in `code_log.txt`:

```
2024-10-25-14:30:45 : Preliminaries complete. Initiating ETL process.
2024-10-25-14:30:47 : Data extraction complete. Initiating Transformation process.
2024-10-25-14:30:48 : Data transformation complete. Initiating loading process.
2024-10-25-14:30:49 : Data saved to CSV file.
2024-10-25-14:30:50 : SQL Connection initiated.
2024-10-25-14:30:51 : Data loaded to Database as table.
2024-10-25-14:30:52 : ETL process completed successfully.
```

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. 🍴 Fork the repository
2. 🔨 Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. 💾 Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. 📤 Push to the branch (`git push origin feature/AmazingFeature`)
5. 🎉 Open a Pull Request

### Ideas for Contribution

- Add more currencies
- Implement real-time exchange rate API
- Add data visualization dashboard
- Include historical data tracking
- Add unit tests
- Implement error handling improvements

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Contact

**Soulayman Aabaid**

[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/soulaymanaabaid123)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/soulaymanaabaid)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:your.email@example.com)

---

## 🌟 Acknowledgments

- Data source: [Wikipedia - List of Largest Banks](https://en.wikipedia.org/wiki/List_of_largest_banks)
- Inspired by real-world ETL pipelines in financial data engineering
- Special thanks to the open-source community

---

<div align="center">

**⭐ Star this repository if you find it helpful!**

Made with ❤️ by [Soulayman Aabaid](https://github.com/soulaymanaabaid123)

</div>

---

## 📈 Project Stats

![GitHub last commit](https://img.shields.io/github/last-commit/soulaymanaabaid123/largest_banks_pipeline)
![GitHub issues](https://img.shields.io/github/issues/soulaymanaabaid123/largest_banks_pipeline)
![GitHub stars](https://img.shields.io/github/stars/soulaymanaabaid123/largest_banks_pipeline)
![GitHub forks](https://img.shields.io/github/forks/soulaymanaabaid123/largest_banks_pipeline)

## 🎓 Learning Outcomes

This project demonstrates proficiency in:
- ✅ ETL Pipeline Development
- ✅ Web Scraping Techniques
- ✅ Data Transformation & Cleaning
- ✅ Database Management (SQLite)
- ✅ Python Programming Best Practices
- ✅ Logging & Error Handling
- ✅ Documentation Skills
