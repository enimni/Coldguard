# ColdGuard AI: Autonomous Cold Chain Recovery

![ColdGuard AI](https://img.shields.io/badge/Status-Active-success) ![Python](https://img.shields.io/badge/Python-3.12-blue) ![FastAPI](https://img.shields.io/badge/FastAPI-0.109.0-009688) ![Gemini](https://img.shields.io/badge/AI-Gemini%202.0-orange) ![Firebase](https://img.shields.io/badge/Database-Firebase-FFCA28)

## 🚨 The Problem

In the modern logistics industry, manual monitoring of cold chain breaches frequently leads to massive product spoilage, compromised safety (especially for vaccines and perishables), and severe financial loss. By the time a human operator detects a temperature spike, identifies the affected cargo, and manually coordinates a response, the damage is already done.

## 💡 The Solution

**ColdGuard AI** is a fully autonomous, real-time response system powered by Google's Gemini API. 

Instead of just triggering a dashboard alert, ColdGuard AI automatically ingests live telemetry (or unstructured field reports), performs financial and safety risk analysis, makes complex routing and replacement decisions, and executes the recovery plan—all within seconds.

## 🧠 Multi-Agent Architecture

The core of ColdGuard AI is a highly resilient 4-agent sequential pipeline:

1. **Sensor Monitor Agent**: Continuously ingests data from IoT sensors (or unstructured driver text messages). It dynamically classifies the severity of the breach based on threshold tolerances.
2. **Analysis Agent**: Cross-references the breaching truck with live Firebase logistics databases to determine what specific cargo is at risk (e.g., Vaccines vs. Dairy), calculates the exact spoilage probability, and estimates the financial loss in PKR.
3. **Decision Agent**: Acts as the tactical commander. Based on the financial and physical risk, it formulates the top 3 priority actions required to mitigate the disaster.
4. **Execution Agent**: Automates the physical response. It quarantines the shipment in the database, drafts professional notifications to stakeholders, and immediately triggers an emergency replacement order from the nearest cold storage facility to ensure the end customer is unaffected.

*Note: The pipeline features built-in fallback mechanisms to ensure 100% uptime and resilience against API rate limits.*

## 🚀 Quick Start

Follow these steps to launch the backend and test the AI pipeline locally:

1. **Install requirements:**
   ```bash
   pip install -r requirements.txt
   ```

2. **Configure Environment:**
   Copy `.env.example` to `.env` and add your `GEMINI_API_KEY` and the path to your Firebase service account JSON.

3. **Seed the Database:**
   Populate your Firebase Realtime Database with synthetic truck and shipment data:
   ```bash
   python scripts/seed_data.py
   ```

4. **Start the Server:**
   Launch the FastAPI backend:
   ```bash
   python run.py
   ```

5. **Test the API:**
   Open your browser and navigate to the interactive documentation:
   > [http://localhost:8000/docs](http://localhost:8000/docs)
   
   *Try the `POST /trigger-breach` or `POST /analyze-unstructured` endpoints to watch the agents go to work!*
