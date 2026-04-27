# Heating Consumption Optimisation

**Technical Manual & Service Specification v1.0**

**Author:** VEO
**Last updated:** 9 Feb 2026

---

## 1. Business Context & Definitions

This service focuses on optimising thermal energy consumption in district heating networks at both:

* **District level** (boiler rooms)
* **Building level** (substations)

The service analyses **historical and near-real-time operational data** to:

* Identify inefficiencies
* Detect energy losses
* Reveal suboptimal operating conditions

It combines **data-driven analysis and AI-based models** to:

* Improve system performance
* Reduce energy consumption
* Support operational decision-making

By analysing consumption patterns together with **external temperature and operational context**, the service enables:

* More efficient control strategies
* Demand-side optimisation
* Better system balancing

---

### Key Terms

* **Building Portfolio**
  Buildings managed by Veolia.

* **Energy Consumption**
  Total thermal energy consumption of buildings, including:

  * Space heating
  * Domestic hot water (DHW)
  * Supply and return temperatures
  * Flow rates (caudal)
  * Valve positions

* **Resolution / Granularity**
  Data is analysed at **15-minute intervals**.

---

## 1.1 Veolia Context

Veolia uses the **Torrelago district heating network** as a real-world environment to:

* Develop
* Test
* Validate

AI-based optimisation services.

The network consists of:

* Centralised heat production (boiler rooms)
* Distributed building substations

---

### Operational Goals

* Reduce total heat consumption
* Improve boiler efficiency
* Minimise energy losses
* Balance **biomass and gas boiler usage**

---

### Practical Use

The service:

* Detects abnormal consumption peaks
* Identifies inefficient behaviours such as:

  * High return temperatures
  * Excessive consumption during mild weather
* Provides actionable insights for operators

---

## 2. Problem Statement

The goal is to optimise heat consumption and production by analysing:

* Historical data
* Near real-time operational data

from:

* Boiler rooms
* Building substations

---

### Core Functionality

The service:

* Estimates **baseline thermal consumption profiles**
* Detects deviations from expected behaviour
* Identifies inefficiencies and optimisation opportunities

---

### Inputs

* Heat energy consumption
* Fuel usage
* Supply and return temperatures
* Weather data

All inputs are:

* Time-aligned
* Standardised

---

### End-User Value

The service enables:

* Operators → optimise system performance
* Residents → reduce consumption and costs

---

## 3. Data Description

The dataset includes:

* Operational variables
* Substation consumption data
* Hydraulic variables
* Temperature measurements
* Control signals

---

## 3.1 Data Dictionary of Buildings

### Operation and Energy Variables

| Variable                    | Variable name                | Type  | Unit | Description                      | Example    |
| --------------------------- | ---------------------------- | ----- | ---- | -------------------------------- | ---------- |
| General water meter         | central_water_meter          | Float | m³   | Total water consumption at plant | 6.00       |
| Boiler 1 energy counter     | boiler1_energy_counter       | Float | kWh  | Boiler 1 energy                  | 17,147.01  |
| Boiler 2 energy counter     | boiler2_energy_counter       | Float | kWh  | Boiler 2 energy                  | 20,638.85  |
| Boiler 3 energy counter     | boiler3_energy_counter       | Float | kWh  | Boiler 3 energy                  | 22,881.29  |
| Gas boilers energy (15 min) | gas_boilers_energy_15min     | Float | kWh  | Gas boiler energy                | 27,414.70  |
| Corrected gas counter       | corrected_gas_counter        | Float | Nm³  | Gas consumption                  | 21,962,436 |
| Central electricity meter   | central_electric_meter_15min | Float | kWh  | Electricity usage                | 19,404.00  |
| Phase 1 energy              | phase1_energy_counter        | Float | kWh  | Phase 1 consumption              | 39,031.50  |
| Phase 2 energy              | phase2_energy_counter        | Float | kWh  | Phase 2 consumption              | 25,587.50  |

---

### Substation Energy Consumption

| Variable                     | Variable name             | Type  | Unit | Description         | Example  |
| ---------------------------- | ------------------------- | ----- | ---- | ------------------- | -------- |
| Heating energy               | heating_energy_substation | Float | kWh  | Heating consumption | 1,928.79 |
| DHW energy                   | dhw_energy_substation     | Float | kWh  | DHW consumption     | 1,565.46 |
| Heating energy (2nd circuit) | heating_energy_2nd        | Float | MWh  | Secondary circuit   | 1,998.16 |

---

### Hydraulic and Power Variables

| Variable          | Variable name     | Type  | Unit | Description   | Example |
| ----------------- | ----------------- | ----- | ---- | ------------- | ------- |
| Heating flow rate | heating_flow_rate | Float | m³/h | Flow rate     | 11.88   |
| DHW flow rate     | dhw_flow_rate     | Float | m³/h | DHW flow      | 5.40    |
| Heating power     | heating_power     | Float | kW   | Thermal power | 169.20  |
| DHW power         | dhw_power         | Float | kW   | DHW power     | 0.40    |

---

### Temperature Variables

| Variable       | Variable name         | Type  | Unit | Description | Example |
| -------------- | --------------------- | ----- | ---- | ----------- | ------- |
| Heating supply | heating_supply_temp   | Float | °C   | Supply temp | 63.89   |
| Heating return | heating_return_temp   | Float | °C   | Return temp | 52.06   |
| DHW supply     | dhw_supply_temp       | Float | °C   | DHW supply  | 76.46   |
| DHW return     | dhw_return_temp       | Float | °C   | DHW return  | 46.98   |
| Setpoint       | heating_temp_setpoint | Float | °C   | Setpoint    | 65.00   |

---

### Control Variables

| Variable       | Variable name  | Type  | Unit | Description     | Example |
| -------------- | -------------- | ----- | ---- | --------------- | ------- |
| Valve position | valve_position | Float | %    | Valve opening   | 99.51   |
| Pump speed     | pump_speed     | Float | %    | Pump regulation | 94.96   |

---

## 4. Analytics, Scope & Update Frequency

### Temporal Scope

* Uses **up to 5 years of historical data (2020–2024)**
* Rolling analysis

---

### Update Frequency

* Every **15 minutes**

---

### Analytical Approach

The service follows a structured pipeline:

#### 1. Baseline Modelling

* Establishes normal consumption patterns by:

  * Season
  * Day of week
  * Operating conditions

---

#### 2. Contextual Peak Detection

* Uses rolling windows (e.g., 7-day)
* Detects anomalies relative to local behaviour
* Avoids fixed thresholds

---

#### 3. Correlation Analysis

* Links consumption to:

  * Outdoor temperature
* Identifies inefficiencies:

  * High demand during mild conditions

---

#### 4. AI-Based Modelling

* Techniques:

  * LSTM
  * XGBoost

Used for:

* Pattern detection
* Operational segmentation
* Inefficiency identification

---

### Outputs

* Anomalous consumption peaks
* Years with abnormal behaviour
* Estimated optimisation potential
* Energy consumption forecasts

---

### API Input / Output

#### Input

JSON via REST API:

* Historical data
* Weather forecasts
* Production schedules

#### Output

JSON:

* Time-series forecasts
* Peak demand predictions
* Cost impact estimates

---

## 5. Evaluation Protocols & Metrics

### 5.1 Data Usage & Analytical Protocol

* Backtesting on historical data
* Comparison against known patterns

---

### 5.2 Data Gaps and Exceptions

* Missing/invalid data handled explicitly
* Impact reflected in evaluation

---

### 5.3 KPIs

* Model accuracy (% similarity real vs simulated)
* Simulation processing time
* Peak detection accuracy
* False positive reduction
* Recurring anomaly detection

---

### Impact Evaluation

* Estimated energy savings
* Comparison vs baseline operation
* Identification of inefficiency drivers

---

## 6. Deliverables & Submissions

### 6.1 Reports

1. Pre-Service Report
2. Intermediate Report
3. Final Report

---

## 6.2 Technical Specifications

### Data Access

* CSV (historical data, 15-min resolution)
* CLIDi REST API

---

### API Details

* JSON responses
* Authentication via credentials
* Internal VM or IP-restricted access

---

### Deployment

* Defined by provider
* Multiple deployment options supported

---

### Handover

* API manual (EN/ES)
* Postman collection
* Credentials
* Onboarding session

---

### Security

* Controlled data sharing
* Project-only usage
* No external redistribution

---

## Implementation & Availability

* Developed in **Jupyter Lab**
* Currently used for **offline analysis**
* Designed for future integration into **EnerTEF platform**

---

### Integration Vision

* Connected via Data Space
* Accessible via APIs or platform UI

