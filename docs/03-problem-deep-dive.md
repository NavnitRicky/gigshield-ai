
# Problem Deep Dive

##  The Income Protection Gap

Gig workers (Zomato, Swiggy, Zepto) face unpredictable income loss due to:

-  Extreme weather (heatwaves, heavy rain)
-  Sudden drop in demand
-  Mobility restrictions (curfews, strikes)

 The biggest issue:
There is **no real-time system** to detect and compensate income loss automatically.

---

##  Core Problem

Current systems fail because they:

- Require **manual claims**
- Take **days or weeks to process**
- Do not measure **actual income loss**
- Are not designed for **gig-based earnings**

Result:  
**Workers bear 100% of the financial risk**

---

##  Hidden Problem: Trusting the Wrong Signal

Most parametric systems rely on:

Weather Data + GPS Location = Payout


But this creates a **critical vulnerability**.

---

##  Adversarial Threat: GPS Spoofing

Fraudsters can exploit the system by:

- Using **fake GPS apps**
- Setting location to high-risk zones
- Triggering false payouts without working

###  Example Attack

1. Worker sits at home  
2. Uses GPS spoofing tool  
3. Sets location to heatwave/flood zone  
4. System detects disruption  
5.  Fake payout is triggered  

---

##  Why This Is Dangerous

### 1. Financial Loss
- Large-scale fake payouts  
- Insurance pool drains quickly  

### 2. System Failure
- Automated system becomes exploitable  
- Parametric model breaks  

### 3. AI Corruption
- Fraud data looks like real data  
- Models learn incorrect patterns  

### 4. Loss of Trust
- Platform becomes unreliable  
- Stakeholders lose confidence  

---

##  Root Cause

> The system assumes location data is always true.

But:

GPS can be manipulated  
 Location ≠ Reality  

---

##  Real Problem Statement

GigShield must solve:

- How to detect **real income loss**
- How to validate **actual worker activity**
- How to prevent **fake claims at scale**
- How to maintain **instant payouts without fraud**

---

##  Key Insight

> **The challenge is not detecting disruption — it is verifying reality.**

---

##  Required Shift

From:

But this creates a **critical vulnerability**.

---

##  Adversarial Threat: GPS Spoofing

Fraudsters can exploit the system by:

- Using **fake GPS apps**
- Setting location to high-risk zones
- Triggering false payouts without working

###  Example Attack

1. Worker sits at home  
2. Uses GPS spoofing tool  
3. Sets location to heatwave/flood zone  
4. System detects disruption  
5.  Fake payout is triggered  

---

##  Why This Is Dangerous

### 1. Financial Loss
- Large-scale fake payouts  
- Insurance pool drains quickly  

### 2. System Failure
- Automated system becomes exploitable  
- Parametric model breaks  

### 3. AI Corruption
- Fraud data looks like real data  
- Models learn incorrect patterns  

### 4. Loss of Trust
- Platform becomes unreliable  
- Stakeholders lose confidence  

---

