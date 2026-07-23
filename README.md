# Patient Monitoring Data Platform

Student: Reem Alsuhaim

Program: SDAIA Academy — Data Engineering for AI Systems (DAICO)

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
                Patient Data
                     |
                     v
             Kafka Producer
                     |
                     v
          Schema Validation
                     |
          +----------+----------+
          |                     |
          v                     v
   Bronze Delta Lake       Quarantine
          |
          v
   Silver Delta Lake
    (Cleaning + MERGE)
          |
          v
    Gold Aggregations
          |
          v
      RAG Pipeline
          |
          v
   LLM Answer + Citations

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

Stores validated raw events while preserving the original structure.

### Silver Layer

Responsible for:

- Data cleaning
- Deduplication
- Business-key based MERGE operations


### Gold Layer

Contains analytical aggregates such as:

- Ward-level statistics
- Patient monitoring summaries
- Alert-related metrics


## 3. RAG Pipeline

The Retrieval-Augmented Generation system enables users to ask questions and receive answers grounded in provided documents.

Pipeline:
Documents
|
Chunking
|
Embeddings
|
Dense Retrieval + BM25
|
Reciprocal Rank Fusion
|
Reranking
|
LLM Response Generation
|
Answer + Citations

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
## 2. Install Python Dependencies
## 3. Start Required Services

# Configuration and Environment Variables

The project uses configuration values to connect different components.

Important configurations include:

| Variable | Description | Example |
|----------|-------------|---------|
| Kafka Bootstrap Server | Kafka connection endpoint | localhost:9092 |
| Kafka Topic | Input data topic | patient_monitoring_raw |
| Spark Configuration | Spark and Delta settings | DeltaCatalog |
| Data Paths | Input/output locations | data/ |
| Model Configuration | RAG model settings | HuggingFace model name |


# Repository Structure
patient-monitoring-capstone/

├── notebooks/
│ ├── 00_environment_test.ipynb
│ ├── 01_ingestion_kafka_validation.ipynb
│ ├── 02_delta_lakehouse.ipynb
│ └── 03_rag_pipeline.ipynb
│
├── data/
│ ├── documents
│ ├── bronze
│ ├── silver
│ └── gold
│
│
├── docker/
│ ├── kafka-compose.yml
│ ├── spark-compose.yml
│ └── airflow-compose.yml
│
├── orchestration/
│
├── requirements.txt
│
└── README.md

# Training Attribution
Completed as part of ** Data Engineering for AI Systems** — SDAIA Academy (DAICO)

Cohort / session dates: 19 July 2026 – 23 July 2026 Trainer: Mohammed Albeladi

SDAIA Academy on GitHub: https://github.com/SDAIAAcademy