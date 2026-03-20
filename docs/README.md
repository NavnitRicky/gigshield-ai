# GigShield — AI-Powered Parametric Insurance for India's Food Delivery Partners

### Guidewire DEVTrails 2026 — Phase 1 Submission  
**Team:** GigShield  
**Platform:** Web Application  

---

# 📑 Table of Contents

1. Executive Summary  
2. Chosen Persona & Rationale  
3. Problem Deep-Dive  
4. Solution Overview  
5. Persona-Based Scenarios & Workflow  
6. Weekly Premium Model  
7. Parametric Triggers  
8. AI/ML Integration Plan  
9. Fraud Detection Architecture  
10. Tech Stack & Architecture  
11. Development Plan (6-Week Roadmap)  
12. Platform Choice Justification  
13. Prototype Status  
14. Data Sources & References  

---

#  Executive Summary

GigShield is an AI-enabled parametric insurance platform that protects India's food delivery partners (Zomato, Swiggy, Zepto) against income loss caused by external disruptions — extreme weather, severe pollution, floods, unplanned curfews, and local strikes.

Unlike traditional insurance that requires claim filing and manual verification, GigShield uses parametric triggers — when weather data crosses predefined thresholds (e.g., temperature > 42°C, rainfall > 65mm/hr), claims are automatically initiated and payouts are processed instantly via UPI.

##  Key Differentiators

- **Zero-touch claims:** No paperwork, no waiting  
- **Weekly pricing:** ₹29–79/week aligned with gig cycles  
- **AI risk scoring:** Dynamic premium calculation  
- **Fraud detection:** Multi-factor anomaly scoring  
- **Hyper-local coverage:** Zone-level triggers  

---

# Chosen Persona & Rationale

## Persona
Food Delivery Partners (Zomato / Swiggy / Zepto)

## Why Food Delivery?

- Largest gig segment (15+ lakh workers)
- Maximum exposure to weather
- Predictable earnings structure
- Weekly income cycle
- No existing income protection

## 📊 Delivery Partner Profile

| Attribute | Value |
|----------|------|
| Daily earnings | ₹800 – ₹1,500 |
| Weekly earnings | ₹4,000 – ₹6,000 |
| Monthly earnings | ₹15,000 – ₹21,000 |
| Working hours | 8–11 hours |
| Earnings/hour | ~₹102 |
| Vehicle type | Bike / EV / Bicycle |
| Heatwave loss | Up to 40% |

---

#  Problem Deep-Dive

## The Income Loss Crisis

Delivery partners face a paradox:  
 High demand during bad weather  
 But physically unable to work  

## 📉 Research Insights

- 19% income loss per °C rise  
- 40% loss during heatwaves  
- Extreme temperatures (>49°C recorded)  
- Increased medical expenses  

##  Why Current Solutions Fail

| Current State | Problem |
|--------------|--------|
| No insurance | Full risk on workers |
| Traditional insurance | Slow & manual |
| Platform compensation | Minimal |
| Government schemes | Not gig-focused |

---

# 💡 Solution Overview
Disruption → Trigger → AI Validation → Auto-Claim → Fraud Check → Instant Payout

##  How GigShield Works


## Core Flow

1. Worker onboarding  
2. AI risk scoring  
3. Policy selection  
4. Dynamic premium calculation  
5. Real-time monitoring  
6. Auto-claim generation  
7. Fraud detection  
8. Instant payout  

---

#  Persona-Based Scenarios & Workflow

## Scenario 1: Heatwave (Delhi)

- Temperature: 47°C  
- Trigger activated  
- Claim generated  
- ₹750 payout  
- Processed in <60 seconds  

---

## 🌧️ Scenario 2: Heavy Rain (Mumbai)

- Rainfall: 95mm/hr  
- Severe trigger  
- ₹567 payout  
- Auto-approved  

---

## Scenario 3: Fraud Detection

- Multiple claims  
- GPS mismatch  
- Fraud score: 72  
- Flagged for review  

---

#  Weekly Premium Model

## Pricing Structure

| Plan | Weekly Premium | Coverage |
|------|--------------|---------|
| Basic | ₹29 | Heat, Rain |
| Standard | ₹49 | + Flood |
| Premium | ₹79 | + Curfew |

---

##  Formula
Weekly Premium = Base Rate × Earnings × Risk Multiplier

---

## 🤖 Risk Score Factors

| Factor | Impact |
|-------|-------|
| City risk | +3 to +15 |
| Vehicle type | +2 to +15 |
| Working hours | +5 to +10 |
| Experience | -8 to +12 |

---

# 🌦️ Parametric Triggers

## 📊 Thresholds

| Disruption | Threshold |
|-----------|----------|
| Heat | > 42°C |
| Rain | > 65 mm/hr |
| AQI | > 300 |
| Flood | > 1.5m |

---

##  Severity Levels

- Warning  
- Severe  
- Extreme  

---

#  AI/ML Integration Plan

## Phase 1
- Rule-based scoring  
- Fraud detection  

## Phase 2
- XGBoost (pricing)  
- Isolation Forest (fraud)  
- LSTM (prediction)  

## Phase 3
- GPS validation  
- Predictive analytics  
- Pattern detection  

---

#  Fraud Detection Architecture

##  Multi-Layer System

1. Parametric validation  
2. Behavioral analysis  
3. Location validation  
4. Pattern recognition  

---

##  Fraud Score

| Score | Action |
|------|-------|
| 0–29 | Auto-approve |
| 30–59 | Review |
| 60+ | Flag |

---

#  Tech Stack & Architecture

## Frontend
- React + TypeScript  
- Tailwind CSS  
- Recharts  

## Backend
- Node.js + Express  
- MongoDB (planned)  
- Zod validation  

## APIs
- OpenWeatherMap  
- Razorpay  
- News API  

---

## 📐 Architecture Diagram

---

## 🤖 Risk Score Factors

| Factor | Impact |
|-------|-------|
| City risk | +3 to +15 |
| Vehicle type | +2 to +15 |
| Working hours | +5 to +10 |
| Experience | -8 to +12 |

---

#  Parametric Triggers

## 📊 Thresholds

| Disruption | Threshold |
|-----------|----------|
| Heat | > 42°C |
| Rain | > 65 mm/hr |
| AQI | > 300 |
| Flood | > 1.5m |

---

##  Severity Levels

- Warning  
- Severe  
- Extreme  

---

#  AI/ML Integration Plan

## Phase 1
- Rule-based scoring  
- Fraud detection  

## Phase 2
- XGBoost (pricing)  
- Isolation Forest (fraud)  
- LSTM (prediction)  

## Phase 3
- GPS validation  
- Predictive analytics  
- Pattern detection  

---

#  Fraud Detection Architecture

## 🔍 Multi-Layer System

1. Parametric validation  
2. Behavioral analysis  
3. Location validation  
4. Pattern recognition  

---

## 📊 Fraud Score

| Score | Action |
|------|-------|
| 0–29 | Auto-approve |
| 30–59 | Review |
| 60+ | Flag |

---

#  Tech Stack & Architecture

## Frontend
- React + TypeScript  
- Tailwind CSS  
- Recharts  

## Backend
- Node.js + Express  
- MongoDB (planned)  
- Zod validation  

## APIs
- OpenWeatherMap  
- Razorpay  
- News API  

---

## 📐 Architecture Diagram
Frontend → API → Risk Engine → Claims → Fraud Detection → Payout



---

# 🗓️ Development Plan (6 Weeks)

##  Phase 1 (Completed)
- Research  
- Risk model  
- Prototype  

##  Phase 2
-Dashboard
- API integration  
- Automation  
- ML models  

##  Phase 3
- Advanced AI  
- Analytics  
- Scaling  

---

# Platform Choice Justification

| Factor | Web | Mobile |
|-------|-----|-------|
| Speed | Faster | Slower |
| Access | Easy | Requires install |
| Deployment | Instant | Delayed |

 Decision: Web Application  

---

# 🧪 Prototype Status

## Features Built

- Admin dashboard  
- Worker onboarding  
- Policy system  
- Claims engine  
- Fraud detection  
- Weather alerts  
- Simulation engine  

---

#  Live Demo

(Add your demo link here)

---

#  Data Sources & References

- Moneycontrol (2026)  
- EfD Initiative  
- Down To Earth  
- NPR  
- J-PAL  
- Deloitte  
- OpenWeatherMap  
- Open-Meteo  

---

