# **ElectionBot**

This repository implements an advanced election data query system using a combination of Python, SQLite, and LlamaIndex. The system enables seamless querying and retrieval of election-related data through natural language queries. This document serves as a complete guide to the setup, usage, and functionality of the system.

---

## **Table of Contents**
1. [Overview](#overview)
2. [Features](#features)
3. [Technologies Used](#technologies-used)
4. [Setup Instructions](#setup-instructions)
5. [Code Components](#code-components)
6. [Database Schema](#database-schema)
7. [Query Pipeline](#query-pipeline)
8. [Example Queries](#example-queries)
9. [Debugging and Logs](#debugging-and-logs)
10. [Visualization](#visualization)
11. [How to Contribute](#how-to-contribute)
12. [License](#license)

---

## **Overview**
The Election Data Query System is designed to analyze and query election-related data, providing insights such as:
- Candidate performance in various constituencies.
- Number of nominations filed.
- Forfeited deposits analysis.
- Voting statistics by year, gender, and region.

This system uses an SQLite database for storing election data and integrates with LlamaIndex for natural language understanding and query processing.

---

## **Features**
- **Natural Language Querying:** Enables users to ask questions in plain English.
- **SQL Query Generation:** Converts natural language queries into SQL commands.
- **Data Visualization:** Generates interactive Directed Acyclic Graphs (DAGs) of the query pipeline.
- **Database Insights:** Allows querying across multiple dimensions such as year, gender, and state.

---

## **Technologies Used**
- **Python:** Core programming language.
- **SQLite:** Database for storing election-related data.
- **LlamaIndex:** For natural language query processing and retrieval.
- **PyVis:** For visualizing the query pipeline.

---

## **Setup Instructions**

### Prerequisites
Ensure you have the following installed:
- Python 3.8 or above
- SQLite
- Necessary Python libraries (listed in `requirements.txt`)

### Installation Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/election-data-query-system.git
   cd electionbot
    ```

2. Install the required Python packages:
    ```bash
    pip install -r requirements.txt
    ```

3. Set up the SQLite database:
    - Place your `elections.db` file in the root directory.
    - Ensure the database schema matches the structure mentioned in the Database Schema section.

4. Run the application:
    ```bash
    python main.py
    ```

## Code Components

### 1. Query Pipeline
The core of the system is the query pipeline, defined using `llama_index`. It processes natural language queries in the following steps:
- Parses the input.
- Retrieves relevant table schemas.
- Converts the natural language query to SQL.
- Executes the SQL query.
- Synthesizes the final response.

### 2. Table Output Parser
The `table_output_parser` component retrieves and formats table context and example rows, ensuring accurate query processing.

### 3. Debugging Utilities
Custom debugging utilities are integrated to log inputs and outputs at each pipeline stage.

## Database Schema
The SQLite database `elections.db` includes the following key tables:

### Tables

- **candidates**
    - `id`: Candidate ID
    - `name`: Candidate name
    - `party_id`: Foreign key to parties
    - `constituency_id`: Foreign key to constituencies

- **parties**
    - `id`: Party ID
    - `abbreviation`: Party abbreviation

- **constituencies**
    - `id`: Constituency ID
    - `name`: Constituency name
    - `state_id`: Foreign key to states
    - `election_id`: Foreign key to elections

- **elections**
    - `id`: Election ID
    - `year`: Election year

- **states**
    - `id`: State ID
    - `name`: State name

- **election_stats**
    - `id`: Statistic ID
    - `constituency_id`: Foreign key to constituencies
    - `category`: Statistic category (e.g., "Forfeited Deposits")
    - `value`: Statistic value

## Example Queries

### 1. Query: Who got the highest votes in Hyderabad in 2019?
```python
response = qp.run(query="Who got the highest number of votes in 2019 in Hyderabad?")
print(response)
```
### 2. Query: How many Lok Sabha constituencies are there in Andhra Pradesh in 2019?
```python
response = qp.run(query="How many Lok Sabha constituencies are there in Andhra Pradesh in the year 2019?")
print(response)
```
### 3. SQL Execution Example
```python
conn = sqlite3.connect('elections.db')
cursor = conn.cursor()
cursor.execute("""
SELECT COUNT(*) FROM constituencies WHERE state_id = (SELECT id FROM states WHERE name = 'Andhra Pradesh');
""")
result = cursor.fetchone()
print(result)
conn.close()
```
## Visualization
The query pipeline is visualized as a Directed Acyclic Graph (DAG) using `pyvis`. To generate the visualization:

```bash
%pip install pyvis
```
```python
from pyvis.network import Network

net = Network(notebook=True, cdn_resources="in_line", directed=True)
net.from_nx(qp.dag)
net.show("agent_dag.html")
```
Open agent_dag.html to view the pipeline structure.
## Debugging and Logs
To enable detailed logs:

```python
import logging
import sys

logging.basicConfig(stream=sys.stdout, level=logging.DEBUG)
logging.getLogger().addHandler(logging.StreamHandler(stream=sys.stdout))
logging.getLogger().handlers = []
```
## How to Contribute
1. Fork the repository.
2. Create a new feature branch.
3. Commit your changes and submit a pull request.

## License

This project is licensed under the MIT License.

### MIT License Summary:
- **Permissions**: You are allowed to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.
- **Conditions**: The above rights are granted under the condition that the copyright notice and this permission notice appear in all copies or substantial portions of the Software.
- **Limitations**: The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, or noninfringement.

By using this software, you agree to the terms of the MIT License. 
For the full license text, please refer to the [LICENSE file](LICENSE).
