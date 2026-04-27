# Heating Consumption Optimisation

**Technical Manual & Service Specification v1.0**

**Author:** VEO
**Last updated:** 9 Feb 2026

---

## 1. Business Context & Definitions

This service focuses on optimising thermal energy consumption in district heating networks at both:

* **District level** (boiler rooms)
* **Building level** (substations)

The core idea is to analyse **historical and near-real-time operational data** to:

* Identify inefficiencies
* Detect energy losses
* Highlight suboptimal operating conditions

By doing so, the service helps operators:

* Reduce unnecessary heat production
* Improve boiler performance
* Better balance heat supply and demand

---

### Key Terms

* **Building Portfolio**
  Buildings managed by Veolia.

* **Energy Consumption**
  Total energy consumption per building, monitored and expressed through:

  * Thermal power (space heating and DHW)
  * Supply and return temperatures
  * Flow rate (caudal)
  * Valve position

* **Resolution / Granularity**
  Data is analysed at **15-minute intervals**.

---

## 1.1 Veolia Context

Veolia uses the **Torrelago district heating network** as a real operational testbed for developing and validating AI-based optimisation services.

Using both:

* Historical data
* Near-real-time measurements

the service aims to:

* Identify inefficiencies
* Reduce overall heat consumption
* Improve thermal performance across the network

Key optimisation goals include:

* Better production control
* Reduction of heat losses
* Improved balance between **biomass and gas boilers**

---

## 2. Problem Statement

The objective of this service is to optimise heat consumption and production across a district heating network.

This is achieved by analysing:

* Historical data
* Near-real-time operational data

from:

* Boiler rooms
* Building substations

---

### Core Functionality

The service:

* Estimates **baseline thermal consumption profiles** under normal conditions
* Detects inefficiencies and abnormal usage patterns

---

### Inputs

* Heat energy consumption measurements
* Fuel usage
* Supply and return temperatures
* Weather data

All inputs are:

* Time-aligned
* Standardised to a common resolution

---

### User Impact

The service also enables **building residents** to:

* Understand their consumption patterns
* Identify high-demand periods
* Adjust usage behaviour

This leads to:

* Improved comfort
* Reduced energy costs

---

## 3. Data Description

The available dataset portfolio includes multiple groups of variables covering:

* Operational energy
* Substation consumption
* Hydraulic behaviour
* Temperature conditions
* Control settings

---

## 3.1 Data Dictionary of Buildings

### Operation and Energy Variables

| Variable                           | Variable name                | Type  | Unit | Description                                | Example    |
| ---------------------------------- | ---------------------------- | ----- | ---- | ------------------------------------------ | ---------- |
| General water meter                | central_water_meter          | Float | m³   | Total water consumption at central plant   | 6.00       |
| Boiler 1 energy counter            | boiler1_energy_counter       | Float | kWh  | Energy produced by boiler 1                | 17,147.01  |
| Boiler 2 energy counter            | boiler2_energy_counter       | Float | kWh  | Energy produced by boiler 2                | 20,638.85  |
| Boiler 3 energy counter            | boiler3_energy_counter       | Float | kWh  | Energy produced by boiler 3                | 22,881.29  |
| Gas boilers energy (15 min)        | gas_boilers_energy_15min     | Float | kWh  | Gas boiler production at 15-min resolution | 27,414.70  |
| Corrected gas counter              | corrected_gas_counter        | Float | Nm³  | Natural gas consumption                    | 21,962,436 |
| Central electricity meter (15 min) | central_electric_meter_15min | Float | kWh  | Central plant electricity consumption      | 19,404.00  |
| Phase 1 energy counter             | phase1_energy_counter        | Float | kWh  | Phase 1 energy consumption                 | 39,031.50  |
| Phase 2 energy counter             | phase2_energy_counter        | Float | kWh  | Phase 2 energy consumption                 | 25,587.50  |

---

### Substation Energy Consumption Variables

| Variable                     | Variable name             | Type  | Unit | Description                       | Example  |
| ---------------------------- | ------------------------- | ----- | ---- | --------------------------------- | -------- |
| Heating energy substation    | heating_energy_substation | Float | kWh  | Heating consumption at substation | 1,928.79 |
| DHW energy substation        | dhw_energy_substation     | Float | kWh  | Domestic hot water consumption    | 1,565.46 |
| Heating energy (2nd circuit) | heating_energy_2nd        | Float | MWh  | Secondary circuit heating energy  | 1,998.16 |

---

### Hydraulic and Power Variables

| Variable              | Variable name     | Type  | Unit | Description               | Example |
| --------------------- | ----------------- | ----- | ---- | ------------------------- | ------- |
| Heating flow rate     | heating_flow_rate | Float | m³/h | Heating circuit flow rate | 11.88   |
| DHW flow rate         | dhw_flow_rate     | Float | m³/h | DHW flow rate             | 5.40    |
| Heating thermal power | heating_power     | Float | kW   | Heating power             | 169.20  |
| DHW thermal power     | dhw_power         | Float | kW   | DHW power                 | 0.40    |

---

### Temperature Variables

| Variable                   | Variable name         | Type  | Unit | Description                | Example |
| -------------------------- | --------------------- | ----- | ---- | -------------------------- | ------- |
| Heating supply temperature | heating_supply_temp   | Float | °C   | Heating supply temperature | 63.89   |
| Heating return temperature | heating_return_temp   | Float | °C   | Heating return temperature | 52.06   |
| DHW supply temperature     | dhw_supply_temp       | Float | °C   | DHW supply temperature     | 76.46   |
| DHW return temperature     | dhw_return_temp       | Float | °C   | DHW return temperature     | 46.98   |
| Temperature setpoint       | heating_temp_setpoint | Float | °C   | Heating setpoint           | 65.00   |

---

### Control Variables

| Variable         | Variable name  | Type  | Unit | Description              | Example |
| ---------------- | -------------- | ----- | ---- | ------------------------ | ------- |
| Valve regulation | valve_position | Float | %    | Valve opening percentage | 99.51   |
| Pump regulation  | pump_speed     | Float | %    | Pump speed               | 94.96   |

---

## 4. Analytics, Scope & Update Frequency

### Temporal Scope

* Based on **rolling historical data (up to 5 years)**
* Updated as new data becomes available

---

### Update Frequency

* Results refreshed **every 15 minutes**

---

### Inputs

* JSON via REST API including:

  * Historical consumption data
  * Production schedules
  * Weather forecasts
  * Forecast request parameters

---

### Outputs

* JSON via REST API including:

  * Time-series forecasts
  * Point predictions
  * Prediction intervals
  * Peak demand predictions
  * Cost impact estimates

---

### Output Format

For each substation (**31 buildings**), outputs include:

* Date of data
* Data storage endpoint
* Data ID
* Structured indicators (consumption, forecasts, etc.)

---

## 5. Evaluation Protocols & Metrics

### 5.1 Data Usage & Analytical Protocol

(Not explicitly defined — to be specified during implementation)

---

### 5.2 Data Gaps and Exceptions

(Not explicitly defined — to be specified during implementation)

---

### 5.3 Service Evaluation Metrics & KPIs

* **Model accuracy**
  → % similarity between real and simulated behaviour

* **Processing time**
  → Simulation runtime (seconds/minutes)

---

## 6. Deliverables & Submissions

*(General deliverables not fully defined in this version)*

---

## 6.2 Technical Specifications & Submissions

### Service Interface Documentation

Veolia provides controlled access to data via:

#### Historical Data

* CSV format
* 15-minute granularity

---

#### CLIDi REST API

* Response format: JSON
* Authentication: project-specific credentials

Access modes:

* Internal API (via dedicated VM)
* External API (IP-restricted / whitelisted)

---

### Deployment Artefacts

* Deployment approach defined by provider
* Alternative models must be documented

---

### Configuration, Versioning & Handover

Includes:

* API manual (EN/ES) in PDF
* Project-specific credentials
* Postman JSON collection
* Deployment description
* Technical onboarding session (if needed)

---

### Security & Data Protection

* Controlled dataset export
* Data shared only within project scope
* Strict usage restrictions

---
