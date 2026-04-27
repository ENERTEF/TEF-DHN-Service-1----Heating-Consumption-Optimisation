# Heating Demand Forecasting

**Technical Manual & Service Specification v1.0**

**Author:** VEO
**Last updated:** 16 Feb 2026

---

## 1. Business Context & Definitions

This service addresses the need for accurate short-term forecasting of heating and domestic hot water (DHW) demand in district heating networks. By leveraging historical consumption data, weather conditions, and operational variables, the service predicts future thermal demand at both district and building level.

The forecasts enable operators to anticipate load variations, plan heat production more efficiently, and avoid peak-related inefficiencies. The service supports improved operational planning, reduces overproduction, and enhances the overall reliability and resilience of the district heating network.

The service builds upon the analytical foundation developed in the **Heating Consumption Optimisation** service and extends it toward predictive capabilities. By analysing consumption patterns in relation to external temperature and operational conditions, it supports proactive decision-making and improved heat production and distribution planning.

---

### Key Terms

* **Building Portfolio**
  Buildings managed by Veolia.

* **Heating Consumption**
  Total thermal energy consumption of each building, expressed through:

  * Thermal power (space heating and DHW)
  * Supply and return temperatures
  * Flow rate (caudal)
  * Pump regulation
  * Valve position

* **Thermal Efficiency (ΔT)**
  Temperature difference between supply and return, used as a key indicator of system performance.

* **Hydraulic Operation**
  Behaviour of pumps and valves regulating heat distribution.

* **Weather Conditions**
  External temperature data used to contextualise demand and improve forecasting accuracy.

* **Resolution / Granularity**
  Data is analysed every **15 minutes**.

---

## 1.1 Veolia Context

Veolia applies this service within the **Torrelago district heating network** to support short-term demand forecasting.

The service combines:

* Historical consumption data
* Real-time or near-real-time operational data
* Weather forecasts (primarily outdoor temperature)

to anticipate demand patterns at both:

* District level
* Building / substation level

These forecasts improve:

* Boiler scheduling
* Peak load management
* Production planning
* System flexibility

Forecast performance is continuously evaluated to ensure reliable and actionable insights for both operational and strategic decision-making.

---

## 2. Problem Statement

The goal of this service is to forecast heat demand within the district heating network by analysing historical consumption data together with operational and external variables.

The service predicts short- and medium-term thermal demand profiles under normal operating conditions and supports proactive system management.

---

### Inputs

* Historical heat energy consumption
* Thermal power
* Supply and return temperatures
* Weather data (mainly outdoor temperature)
* Calendar information

All inputs are aligned at a common temporal resolution.

---

### Operational Purpose

The service enables operators and building managers to:

* Anticipate future heating demand
* Identify peak load periods
* Improve boiler scheduling
* Optimise production planning
* Reduce operational costs

---

## 3. Data Description

The dataset describes the building portfolio and includes operational, energy, hydraulic, temperature, and control variables.

---

## 3.1 Data Dictionary of Buildings

### Operation and Energy Variables

| Variable                    | Variable name                | Type  | Unit | Description                              | Example    |
| --------------------------- | ---------------------------- | ----- | ---- | ---------------------------------------- | ---------- |
| General water meter         | central_water_meter          | Float | m³   | Total water consumption at central plant | 6.00       |
| Boiler 1 energy counter     | boiler1_energy_counter       | Float | kWh  | Boiler 1 energy                          | 17,147.01  |
| Boiler 2 energy counter     | boiler2_energy_counter       | Float | kWh  | Boiler 2 energy                          | 20,638.85  |
| Boiler 3 energy counter     | boiler3_energy_counter       | Float | kWh  | Boiler 3 energy                          | 22,881.29  |
| Gas boilers energy (15 min) | gas_boilers_energy_15min     | Float | kWh  | Gas boiler energy                        | 27,414.70  |
| Corrected gas counter       | corrected_gas_counter        | Float | Nm³  | Gas consumption                          | 21,962,436 |
| Central electricity meter   | central_electric_meter_15min | Float | kWh  | Electricity usage                        | 19,404.00  |
| Phase 1 energy              | phase1_energy_counter        | Float | kWh  | Phase 1 consumption                      | 39,031.50  |
| Phase 2 energy              | phase2_energy_counter        | Float | kWh  | Phase 2 consumption                      | 25,587.50  |

---

### Substation Energy Consumption Variables

| Variable                     | Variable name             | Type  | Unit | Description         | Example  |
| ---------------------------- | ------------------------- | ----- | ---- | ------------------- | -------- |
| Heating energy substation    | heating_energy_substation | Float | kWh  | Heating consumption | 1,928.79 |
| DHW energy substation        | dhw_energy_substation     | Float | kWh  | DHW consumption     | 1,565.46 |
| Heating energy (2nd circuit) | heating_energy_2nd        | Float | MWh  | Secondary circuit   | 1,998.16 |

---

### Hydraulic and Power Variables

| Variable              | Variable name     | Type  | Unit | Description   | Example |
| --------------------- | ----------------- | ----- | ---- | ------------- | ------- |
| Heating flow rate     | heating_flow_rate | Float | m³/h | Flow rate     | 11.88   |
| DHW flow rate         | dhw_flow_rate     | Float | m³/h | DHW flow      | 5.40    |
| Heating thermal power | heating_power     | Float | kW   | Thermal power | 169.20  |
| DHW thermal power     | dhw_power         | Float | kW   | DHW power     | 0.40    |

---

### Temperature Variables

| Variable                   | Variable name         | Type  | Unit | Description | Example |
| -------------------------- | --------------------- | ----- | ---- | ----------- | ------- |
| Heating supply temperature | heating_supply_temp   | Float | °C   | Supply temp | 63.89   |
| Heating return temperature | heating_return_temp   | Float | °C   | Return temp | 52.06   |
| DHW supply temperature     | dhw_supply_temp       | Float | °C   | DHW supply  | 76.46   |
| DHW return temperature     | dhw_return_temp       | Float | °C   | DHW return  | 46.98   |
| Temperature setpoint       | heating_temp_setpoint | Float | °C   | Setpoint    | 65.00   |

---

### Control Variables

| Variable         | Variable name  | Type  | Unit | Description   | Example |
| ---------------- | -------------- | ----- | ---- | ------------- | ------- |
| Valve regulation | valve_position | Float | %    | Valve opening | 99.51   |
| Pump regulation  | pump_speed     | Float | %    | Pump speed    | 94.96   |

---

## 4. Analytics, Scope & Update Frequency

### Temporal Scope

Analytics are computed over rolling historical data using approximately **five years of data** to capture:

* Seasonal behaviour
* Daily demand patterns
* Relationships with external conditions

---

### Update Frequency

* Results refreshed every **15 minutes**
* Designed for near-real-time updates

---

### Analytical Approach

The service follows a structured forecasting pipeline:

* **Historical Pattern Analysis**
  Identification of recurring consumption patterns at building and district level

* **Time-Series Modelling**
  AI-based models such as:

  * LSTM
  * XGBoost
    used to capture temporal dependencies

* **Feature Engineering**
  Includes:

  * Weather variables (especially temperature forecasts)
  * Derived indicators (temperature trends, heating indices)

* **External Data Integration**
  Temperature forecast datasets are used to anticipate future demand

* **Model Validation & Selection**
  Multiple models are compared to ensure robustness and accuracy

---

### Input

JSON via REST API including:

* Historical consumption data
* Production schedules
* Weather forecasts
* Forecast parameters

---

### Output

JSON including:

* Time-series demand forecasts
* Point predictions
* Prediction intervals
* Peak demand predictions
* Cost impact estimates

---

### Output Format

For each substation (31 buildings), outputs include:

* Data timestamp
* Data ID
* Storage endpoint
* Forecasted demand profiles
* Peak demand indicators
* Prediction intervals

---

## 5. Evaluation Protocols & Metrics

The evaluation ensures reliable, accurate, and operationally useful forecasts.

---

### 5.1 Data Usage & Analytical Protocol

* Train-validation-test methodology
* Evaluation on historical datasets
* Validation across seasons
* Comparison with baseline models

---

### 5.2 Data Gaps and Exceptions

* Missing or invalid data handled explicitly
* Impact reflected in evaluation metrics
* Documented data-quality handling

---

### 5.3 Service Evaluation Metrics & KPIs

Forecast accuracy metrics:

* Mean Absolute Error (MAE)
* Root Mean Squared Error (RMSE)
* Coefficient of Determination (R²)

Operational and portfolio indicators:

* **NEPD (Normalised Energy Performance Deviation)**
* **SCIB (Share of Consumption in Inefficient Buildings)**
* **CCI (Cluster Cohesion Index)**

---

## 6. Deliverables & Submissions

---

## 6.2 Technical Specifications & Submissions

### Data Access

* CSV datasets (15-minute resolution)
* CLIDi REST API

---

### API Details

* JSON responses
* Authentication via credentials
* Internal VM access or IP-restricted external API

---

### Deployment

* Defined by provider
* Supports flexible deployment (local / platform integration)

---

### Configuration & Handover

* API manual (EN/ES)
* Credentials
* Postman collection
* Deployment documentation
* Onboarding session

---

### Security & Data Protection

* Controlled dataset access
* Project-specific usage
* No external redistribution

---

## Implementation & Availability

The service is implemented in a Python-based data science environment supporting:

* Model development
* Training and validation
* Deployment in scalable pipelines

It supports:

* Near real-time operation
* Integration with optimisation services
* Integration with digital twins
* Deployment within EnerTEF platform

---

**Integration-ready outputs enable direct use in:**

* Scheduling systems
* Optimisation services
* Energy management platforms

---


