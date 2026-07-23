# Patient Monitoring Data Platform

Student: Reem Alsuhaim

Program: SDAIA Academy — DModern Data Engineering for AI Systems (DAICO)

Session dates: 19 July 2026 – 23 July 2026

Trainer: Mohammed Albeladi

## Project Overview

Patient Monitoring Data Platform is an end-to-end data engineering and AI pipeline designed to process healthcare monitoring data. 

The project simulates a real-world patient monitoring system that ingests vital sign records, validates incoming data, stores information using a Delta Lakehouse architecture, and provides grounded question answering through a Retrieval-Augmented Generation (RAG) pipeline.

The platform demonstrates a complete data lifecycle:

- Data ingestion
- Data validation
- Data storage and transformation
- Analytics generation
- AI-powered retrieval and question answering


## Problem Statement

Healthcare monitoring systems generate continuous streams of patient data that require reliable ingestion, structured storage, and intelligent analysis.

This project addresses the challenge of building a scalable pipeline capable of:

- Handling streaming healthcare data
- Ensuring data quality through schema validation
- Maintaining historical and processed datasets
- Providing reliable answers based on medical documents


## Project Scope

The project implements:

- Kafka-based data ingestion pipeline
- Schema validation using Pydantic
- Bronze, Silver, and Gold Delta Lake layers
- Data cleaning and upsert operations using MERGE
- Hybrid Retrieval-Augmented Generation (RAG)
- Document retrieval using BM25 and dense embeddings
- Result fusion using Reciprocal Rank Fusion (RRF)
- Response generation with citations


# System Architecture

```text
                         Patient Monitoring Data
                                  |
                                  v
                          Kafka Ingestion
                                  |
                                  v
                       Schema Validation Layer
                                  |
                    +-------------+-------------+
                    |                           |
                    v                           v
             Bronze Delta Lake          Quarantine Records
                    |
                    v
             Silver Delta Lake
          (Cleaning + MERGE/UPSERT)
                    |
                    v
              Gold Data Layer
        (Aggregations & Analytics)
                    |
                    v
              RAG Pipeline
                    |
                    v
          LLM Answer Generation
             + Source Citations
```

Pipeline flow:

1. Patient monitoring events are generated and streamed through Kafka.
2. Incoming records are validated using predefined schemas.
3. Valid records are stored in the Bronze Delta Lake layer.
4. Data is cleaned and updated in the Silver layer using MERGE operations.
5. Gold layer generates analytical summaries and aggregated metrics.
6. The RAG pipeline retrieves relevant documents and generates grounded answers.
   

# Project Components

## 1. Data Ingestion

The ingestion layer is responsible for receiving patient monitoring events.

Implemented features:

- Kafka producer for streaming data
- Kafka consumer for processing events
- Pydantic schema validation
- Invalid records handling through quarantine/dead-letter flow


## 2. Delta Lakehouse

The project follows the Medallion Architecture:

### Bronze Layer

Stores validated patient monitoring records received from the ingestion layer while preserving the original event structure.

### Silver Layer

Responsible for:

- Data cleaning
- Deduplication
- Business-key based MERGE operations
- Incremental updates and inserts

### Gold Layer

Contains analytical aggregates such as:

- Average heart rate by ward
- Average oxygen saturation by ward
- Patient counts per ward
- Critical patient counts (oxygen level < 92%)


## 3. RAG Pipeline

The Retrieval-Augmented Generation system enables users to ask questions and receive answers grounded in provided documents.

RAG Pipeline Features:
- Document chunking
- SentenceTransformer embeddings
- FAISS vector store
- BM25 keyword retrieval
- Reciprocal Rank Fusion (RRF)
- Cross-Encoder reranking
- Citation-based answer generation

# Prerequisites

Before running the project, ensure the following tools are installed:

- Python 3.11 or later
- Docker Desktop
- Docker Compose
- Jupyter Notebook
- Git


Recommended environment:

- Windows/Linux operating system
- Minimum 16 GB RAM

# Installation and Setup

## 1. Clone Repository

```bash
git clone <repository-url>
cd patient-monitoring-capstone
```

## 2. Install Python Dependencies

```bash
pip install -r requirements.txt
```

## 3. Start Kafka Services

```bash
docker compose -f docker/kafka-compose.yml up -d
```

## 4. Start Spark Services

```bash
docker compose -f docker/spark-compose.yml up -d
```

## 5. Execute Notebooks

Run notebooks in the following order:

1. 00_environment_test.ipynb
2. 01_ingestion_kafka_validation.ipynb
3. 02_delta_lakehouse.ipynb
4. 03_rag_pipeline.ipynb

# Failure Handling

The project demonstrates the following failure scenarios:

- Invalid Kafka records rejected through schema validation
- Invalid records routed to a dead-letter topic
- Delta Lake schema enforcement preventing invalid writes
- Merge operations handling updates and inserts correctly

# Expected Output

Successful execution produces:

- Valid patient records stored in Bronze
- Cleaned and merged records stored in Silver
- Aggregated ward statistics in Gold
- Invalid records routed to a dead-letter queue
- Grounded RAG responses with document citations


# Repository Structure
```text
patient-monitoring-capstone/
│
├── notebooks/
│   ├── 00_environment_test.ipynb
│   ├── 01_ingestion_kafka_validation.ipynb
│   ├── 02_delta_lakehouse.ipynb
│   ├── 03_rag_pipeline.ipynb
│   ├── 04_orchestration_simulation.ipynb
│   └── 05_openlineage.ipynb
│
├── data/
│   ├── documents/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── docker/
│   ├── kafka-compose.yml
│   ├── spark-compose.yml
│   └── airflow-compose.yml
│
├── orchestration/
│
├── requirements.txt
│
└── README.md
```

# Training Attribution
Completed as part of the SDAIA Academy Program:

Program: Modern Data Engineering for AI Systems

Session Dates:
19 July 2026 – 23 July 2026

Trainer:
Mohammed Albeladi

SDAIA Academy GitHub:
https://github.com/SDAIAAcademy
