# Air Quality Analytics + GenAI

This repo demonstrates a simple, production-style analytics flow:
external data ingestion → curated analytics models → SQL-based Q&A → GenAI narrative.

---
## Data Source

NYC Open Data – Air Quality Indicators

Raw table (managed by dlt):
- `https://data.cityofnewyork.us/resource/c3uy-2p5r.json`

---

## Analytics Models

Only curated tables are exposed to the chatbot.

### `monthly_pm25`
Monthly average PM2.5 by borough.

Columns:
- `month`
- `borough`
- `avg_pm25`
- `risk_level`

### `q4_pm25`
Q4 average PM2.5 by borough.

Columns:
- `borough`
- `q4_avg_pm25`

---

## Metric Definitions

- **PM2.5**: Mean concentration of fine particles (mcg/m³)
- **Low risk**: ≤ 9
- **Medium risk**: 9–12
- **High risk**: > 12

Thresholds are deterministic.

---

## Query Flow

1. User asks a question in natural language
2. LLM selects table, metrics, and time range from a fixed schema
3. SQL is generated and validated
4. DuckDB executes the query
5. LLM explains results using returned data only

The LLM never sees raw tables.

---

## Example Question

“How was air quality in Q4?”

Resolved as:
- Table: `q4_pm25`
- Metric: `q4_avg_pm25`
- Group by: `borough`

---

## Key Design Principles

- Separate ingestion from modeling
- No schema discovery by the LLM
- No vector store for structured analytics
- SQL is the source of truth
- GenAI is used for explanation, not computation

---

## Purpose

This demo shows how GenAI can sit safely on top of a BI system to answer
business-style questions using structured data.
