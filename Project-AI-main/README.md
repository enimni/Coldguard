# ColdGuard AI — Fully Autonomous Cold Chain Recovery & Mitigation System

<p align="center">
  <img src="https://img.shields.io/badge/ColdGuard%20AI-Autonomous%20Cold%20Chain-blue?style=for-the-badge&logo=google-cloud&logoColor=white" alt="ColdGuard AI Logo">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Active-success?style=flat-square" alt="Status Active">
  <img src="https://img.shields.io/badge/Response%20Time-%3C%2030%20Seconds-orange?style=flat-square" alt="Response Time">
  <img src="https://img.shields.io/badge/Tech-FastAPI%20%7C%20Gemini%20%7C%20Firebase%20%7C%20Flutter-blueviolet?style=flat-square" alt="Stack">
  <img src="https://img.shields.io/badge/OS-Windows%20%7C%20Android%20%7C%20iOS%20%7C%20Web-lightgrey?style=flat-square" alt="Platforms">
</p>

---

## 🚨 1. The Core Cold Chain Problem

In modern logistics, transporting temperature-sensitive cargo (e.g., life-saving vaccines, insulin, fresh dairy, and frozen meats) is extremely volatile. A single cooling failure, compressor malfunction, or extended border delay can breach temperature limits, causing irreversible product degradation. This poses severe public health risks and results in massive financial waste.

### The Reactive Bottleneck
Existing cold-chain telemetry systems are strictly **reactive**:
1. Sensors monitor temperatures and display them on a map.
2. If a breach occurs, the system triggers a simple visual dashboard alert or sends an email.
3. The burden of recovery falls entirely on human operators, who must manually:
   - Identify the vehicle and cross-reference active cargo manifests.
   - Compute thermal spoilage timelines and physical degradation risks.
   - Calculate total financial exposure in local currencies.
   - Design a recovery plan (quarantining, rerouting, safety inspections).
   - Coordinate communication with drivers, dispatchers, end clients, and insurance claims adjusters.

This manual process takes **30–60 minutes** on average. In a cold chain breach, every minute counts: a 30-minute delay can mean a total cargo write-off worth **PKR 10M to 25M** ($35K to $90K USD).

---

## 💡 2. The Autonomous ColdGuard Solution

**ColdGuard AI** is a fully autonomous, real-time breach recovery pipeline. By leveraging **Google's Gemini API** and a resilient **4-agent sequential pipeline**, ColdGuard compresses the entire containment and recovery lifecycle from **30–60 minutes down to under 30 seconds**—completely eliminating the human bottleneck. 

When a refrigerated truck's temperature crosses a critical threshold, ColdGuard instantly:
- Ingests IoT telemetry.
- Performs rigorous multi-system physical and financial risk analysis.
- Chooses optimal, situation-specific recovery commands.
- Automatically executes actions (updates databases, quarantines compromised cargo, schedules replacement dispatches).
- Drafts localized alerts to all stakeholders.

---

## 🛠️ 3. High-Level System Architecture

ColdGuard is designed as an event-driven, real-time architecture where state changes synchronize instantly across all components.

```mermaid
graph TD
    %% Define System Nodes
    subgraph Sensors ["IoT Sensors & Drivers"]
        SensorSim["Structured Telemetry (Sensor Sim)"]
        DriverMsg["Unstructured Text (Driver SMS)"]
    end

    subgraph Backend ["FastAPI Application Backend (Python)"]
        FastAPI["FastAPI Web Server"]
        Orchestrator["Agent Pipeline Orchestrator"]
        
        subgraph Agents ["Resilient Multi-Agent Pipeline"]
            Agent1["Sensor Monitor Agent (Assessment)"]
            Agent2["Analysis Agent (Risk/Finance)"]
            Agent3["Decision Agent (Command)"]
            Agent4["Execution Agent (Automation)"]
        end
    end

    subgraph DB ["Cloud Storage / State"]
        Firebase["Firebase Realtime Database"]
    end

    subgraph Clients ["Cross-Platform Client Apps"]
        FlutterApp["Flutter Mobile Application"]
        SSE["SSE Realtime Stream Connection"]
    end

    %% Define Relationships and Data Flow
    SensorSim -->|HTTP POST /trigger-breach| FastAPI
    DriverMsg -->|HTTP POST /analyze-unstructured| FastAPI
    
    FastAPI -->|Extracts/Triggers| Orchestrator
    Orchestrator -->|Chains Sequentially| Agent1
    Agent1 -->|Breach Confirmed| Agent2
    Agent2 -->|Risk Analyzed| Agent3
    Agent3 -->|Decisions Approved| Agent4
    
    Agent4 -->|Write Action Payload| Firebase
    FastAPI -->|Query Live State| Firebase
    
    Firebase -.->|Real-time Sync (Listeners)| FlutterApp
    FastAPI -->|SSE Stream /agent-trace/stream| SSE
    SSE -.->|Live Trace Updates| FlutterApp
    
    %% Styles
    classDef main fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef agent fill:#efebe9,stroke:#4e342e,stroke-width:2px;
    classDef db fill:#fffde7,stroke:#fbc02d,stroke-width:2px;
    classDef client fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    
    class SensorSim,DriverMsg,FastAPI,Orchestrator main;
    class Agent1,Agent2,Agent3,Agent4 agent;
    class Firebase db;
    class FlutterApp,SSE client;
```

---

## 🧠 4. The 4-Agent Pipeline Deep-Dive

The core intelligence of ColdGuard is its 4-Agent sequential pipeline. The output of each agent acts as a strictly typed JSON input for the next, establishing a cohesive logical context.

```
[IoT Telemetry] 
      │
      ▼
┌────────────────────────────────┐
│ 1. Sensor Monitor Agent        │ -> Validates threshold deviation and severity
└────────────────────────────────┘
      │
      ▼
┌────────────────────────────────┐
│ 2. Analysis Agent              │ -> Cross-references cargo databases, calculates PKR spoilage & losses
└────────────────────────────────┘
      │
      ▼
┌────────────────────────────────┐
│ 3. Decision Agent              │ -> Evaluates tactical recovery options, chooses best 3 actions
└────────────────────────────────┘
      │
      ▼
┌────────────────────────────────┐
│ 4. Execution Agent             │ -> Writes to Firebase, drafts SMS/email alerts, triggers replacement
└────────────────────────────────┘
      │
      ▼
[Contained Incident]
```

### Agent Roles & Specifications

#### Agent 1: Sensor Monitor Agent (Assessment)
- **Role**: The first line of defense. It ingests telemetry, validates it against active safety thresholds, and determines if a breach is active.
- **Inputs**: `truck_id`, `current_temp`, `threshold_temp`, `cargo_type`, `timestamp`.
- **Output JSON Schema**:
  ```json
  {
    "breach_detected": true,
    "severity": "CRITICAL",
    "deviation": 4.5,
    "reasoning": "A breach was detected because the current temperature of 12.5C exceeds the critical threshold of 8.0C for Vaccines by 4.5C."
  }
  ```

#### Agent 2: Analysis Agent (Risk & Financial Assessment)
- **Role**: The auditor. It cross-references current shipments to locate cargo types, calculates spoilage probability based on duration and heat, and evaluates total financial exposure in PKR.
- **Inputs**: Output of Agent 1 + shipment database records.
- **Output JSON Schema**:
  ```json
  {
    "cargo_at_risk": "COVID-19 Vaccines",
    "spoilage_probability": 0.85,
    "financial_loss_pkr": 24000000,
    "impact_summary": "High-value Vaccines are highly sensitive. A current temperature of 12.5C introduces an 85% spoilage risk within 45 minutes, exposing the business to PKR 24,000,000 in direct cargo losses."
  }
  ```

#### Agent 3: Decision Agent (Tactical Commander)
- **Role**: The commander. It evaluates logical options based on severity and location. It decides whether to quarantine, reroute, or immediately replace the shipment.
- **Inputs**: Outputs of Agent 1 and Agent 2.
- **Output JSON Schema**:
  ```json
  {
    "quarantine_required": true,
    "reroute_required": false,
    "emergency_replacement_required": true,
    "recommended_actions": [
      "Quarantine shipment SHP-004 immediately to prevent degraded distribution.",
      "Instruct driver to halt securely at the nearest terminal.",
      "Issue emergency replacement dispatch from the central hub."
    ]
  }
  ```

#### Agent 4: Execution Agent (Automated Executor)
- **Role**: The executioner. It commits the quarantine status to the database, initiates logs, schedules replacements, and drafts specific, ready-to-send alerts to stakeholders.
- **Inputs**: Outputs of Agents 1, 2, and 3.
- **Output JSON Schema**:
  ```json
  {
    "incident_id": "INC-20260519-001",
    "actions_executed": [
      {
        "action": "SHIPMENT_QUARANTINED",
        "details": "Shipment SHP-004 status set to QUARANTINED in Firebase."
      },
      {
        "action": "DRIVER_SMS_SENT",
        "recipient": "Zahid Khan",
        "content": "ALERT: Critical breach of 12.5C. Pull over safely at nearest bay. Replacement dispatched."
      },
      {
        "action": "CLIENT_NOTIFIED",
        "recipient": "Metro Pharma Pakistan",
        "content": "Important: Shipment compromised under Incident INC-20260519-001. Quarantined. Rerouted. Replacement scheduled."
      }
    ]
  }
  ```

---

## 🎛️ 5. API Endpoints Specification

The Python backend is built with FastAPI. It exposes a powerful suite of endpoints documented interactively via Swagger at `/docs`:

| Method | Endpoint | Description |
|---|---|---|
| **GET** | `/sensors` | Returns active telemetry, status, cargo thresholds, and routes for all 10 fleet trucks. |
| **GET** | `/shipments` | Returns all active shipments, cargo details, and shipment status flags. |
| **POST** | `/trigger-breach` | Simulates a temperature spike, executing the 4-agent pipeline synchronously. |
| **POST** | `/analyze-unstructured` | Ingests raw text (e.g., "Driver reports TRK-004 unit failed, temp is 12C"), parses details, and triggers the orchestrator. |
| **GET** | `/agent-trace/stream/{id}/{temp}` | Server-Sent Events (SSE) stream. Streams agent execution states and logs real-time. |
| **GET** | `/reset-demo` | Clears active incidents and restores initial database states. |

---

## 📱 6. Mobile Application Specification

The mobile dashboard is built in Flutter using **Riverpod** for reactive state management and listens to Firebase Realtime Database for instant synchronization.

### 🎨 Premium Visual Elements & Micro-Animations
- **Fleet Dashboard**: Features glassmorphic cards, harmonized status halos (Green, Amber, Red), and glowing action banners.
- **Liquid Card Expansion**: Uses `AnimatedSize` (500ms, `easeInOutCubic`) inside trace cards to expand details smoothly without height snaps.
- **Float-In Solutions**: Utilizes `flutter_animate` to slide and fade in agent results over 450ms when completed.
- **Real-Time Temperature Charts**: Interactive historical plotting powered by `fl_chart`.
- **Containment Success Portal**: When all breaches are resolved, a custom animated modal displays total protected values, time elapsed, and offers direct redirection to the Reports Audit Portal.

### 📋 Interactive Reports Portal
Accessed directly from the grid menu:
- Displays fleet-wide statistics ($1.05M prevented loss, 1.8s avg containment time, 100% success rate).
- Renders detailed audit logs for all past incidents.
- Clicking any history card dynamically reconstructs telemetry states and drills down to the corresponding complete Outcome report.

---

## 🗺️ 7. Localized Pakistani Domain Context

ColdGuard operates inside a fully localized Pakistani logistics framework to guarantee immediate industrial relevance:

### Regional Shipping Corridors
- **North Hub**: Islamabad, Rawalpindi, Peshawar, Abbottabad.
- **Central Hub**: Lahore, Gujranwala, Faisalabad, Multan.
- **South Hub**: Karachi, Hyderabad, Sukkur.

### Cargo Thresholds & Valuations
- **COVID-19 Vaccines**: Spoilage > 8°C. Value: **PKR 18.5M** ($65,000 USD)
- **Insulin Vials**: Spoilage > 8°C. Value: **PKR 24M** ($85,000 USD)
- **Fresh Dairy**: Spoilage > 8°C. Value: **PKR 3.5M** ($12,000 USD)
- **Frozen Meat**: Spoilage > -18°C. Value: **PKR 4.8M** ($17,000 USD)

---

## 🛡️ 8. Resiliency & Fallback Systems

To ensure zero downtime during critical demonstrations, ColdGuard incorporates a robust three-tier fallback architecture:

1. **Gemini API Shielding (`429` / Rate Limit Handling)**:
   If the Gemini API encounters rate limits, timeouts, or invalid keys, the FastAPI backend catches the error and triggers a dynamic local context generator. This generates professional, cargo-specific, and structurally identical recovery JSONs, completing the pipeline in milliseconds without displaying error states.
2. **Flutter Demo Mode**:
   A global local-first toggle. When enabled, Riverpod disconnects active Firebase listeners and runs on high-fidelity, state-aware Dart mock loops, allowing complete presentations even in zero-bandwidth environments.
3. **SSE Connection Recovery**:
   If the live Server-Sent Events stream disconnects, the client app dynamically falls back to standard REST polls to retrieve partial or full trace results from Firebase.

---

## 🚀 9. Quick Start Guide

### 🐍 Backend (Python / FastAPI)

1. **Clone & Setup Environment**:
   ```bash
   pip install -r requirements.txt
   ```
2. **Environment Configuration**:
   Create a `.env` file in the root directory:
   ```env
   GEMINI_API_KEY=your_gemini_api_key_here
   FIREBASE_DB_URL=https://your-project.firebaseio.com/
   FIREBASE_CREDENTIALS_JSON=path/to/firebase-credentials.json
   ```
3. **Seed Firebase Database**:
   ```bash
   python scripts/seed_data.py
   ```
4. **Start Web Server**:
   ```bash
   python run.py
   ```
   Interactive Swagger docs will be available at [http://localhost:8000/docs](http://localhost:8000/docs).

### 💙 Frontend (Flutter)

1. **Setup Flutter SDK**:
   Ensure Flutter is installed and configured (Targeting Flutter 3.19+ / Dart 3.3+).
2. **Install Flutter Dependencies**:
   Navigate to the `mobile` directory:
   ```bash
   cd mobile
   flutter pub get
   ```
3. **Run Mobile Client**:
   Ensure the backend is active, then launch:
   ```bash
   flutter run -d chrome  # Or iOS / Android simulators
   ```

---

*Document Author: Google Antigravity Coding Assistant*  
*Project Role: Principal System Architect & Developer*  
*Last Updated: May 19, 2026*  
