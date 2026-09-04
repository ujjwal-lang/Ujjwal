# SourceIQ — Used Car Inventory Intelligence

## Overview

Used-car dealerships make a large number of inventory acquisition decisions every month. However, deciding which vehicles to purchase is often driven by a combination of dealer experience, historical intuition, auction availability, and fragmented market information.

This creates a significant business problem: **the wrong vehicle purchased at the wrong price can remain on the lot for months, tying up capital, requiring price reductions, and reducing dealership profitability.**

This project explores a data-driven **Vehicle Acquisition Intelligence Platform** that helps dealers decide:

* Which vehicles should I buy?
* What is the maximum price I should pay?
* How quickly is this vehicle likely to sell?
* What margin can I reasonably expect?
* Which vehicles should I avoid?

### Business Context

The case was designed around a hypothetical network of approximately **200 US used-car dealerships**.

Key observations:

| Metric                            | Current State |
| --------------------------------- | ------------: |
| Average days-on-lot               |      ~72 days |
| Gross profit / vehicle            | Down ~18% YoY |
| Vehicles sourced / dealer / month |       ~20–100 |
| Target days-on-lot                |      <45 days |
| Target gross profit / vehicle     |          +10% |

Dealers currently use a combination of tools such as market reports, platforms like vAuto, auction information, historical experience, and personal judgment.

The core opportunity is to turn this fragmented information into a **single acquisition decision layer**.

---

# Problem

A dealer does not simply need to know whether a particular car is "good."

They need to know whether **this particular car is a good purchase for their dealership, in their local market, at this price, right now.**

For example:

> A 2022 Honda Accord may be a highly desirable vehicle nationally, but that does not necessarily mean it is the right vehicle for a particular dealership.

Its attractiveness depends on:

* Local demand
* Existing inventory
* Competitive pricing
* Vehicle mileage
* Vehicle condition
* Acquisition cost
* Expected retail price
* Expected days-to-sale
* Seasonality
* Historical dealership performance
* Reconditioning cost

Therefore, the problem is fundamentally an **inventory acquisition optimization problem**.

---

# Product Hypothesis

If we can predict the expected **sale velocity, retail price, costs, and risk** of a vehicle before the dealer purchases it, then we can help dealers make better acquisition decisions.

The product should transform:

**Raw vehicle + market data**

into:

**Actionable acquisition recommendation**

For example:

> **BUY**
>
> Expected sale probability within 45 days: 82%
> Expected days-to-sale: 31 days
> Expected retail price: $27,800
> Estimated total cost: $23,900
> Expected gross profit: $3,900
> Maximum recommended acquisition price: $21,800

Instead of simply showing data, the product should answer:

> **"Should I buy this car?"**

---

# Proposed Solution

The proposed solution is an **AI-powered Vehicle Acquisition Intelligence Engine**.

The system combines predictive models, market data, dealership preferences, and optimization logic to produce a recommendation for every candidate vehicle.

### High-level flow

```text
Vehicle / VIN Data
        ↓
Data Normalization
        ↓
Local Market Analysis
        ↓
Demand Prediction
        ↓
Sale Velocity Prediction
        ↓
Retail Price Prediction
        ↓
Cost & Margin Estimation
        ↓
Risk Assessment
        ↓
Dealer Constraints
        ↓
Acquisition Optimization
        ↓
BUY / NEGOTIATE / PASS
```

The important product decision is to separate **prediction from decision-making**.

The ML models predict what is likely to happen.

The optimization layer decides what the dealer should do based on their goals and constraints.

---

# Core Product Features

## 1. Inventory Opportunity Dashboard

The dashboard gives the dealer a high-level view of the health and opportunity within their inventory.

Key metrics include:

* Average days-on-lot
* Inventory turnover
* Gross profit per vehicle
* Vehicles at risk of aging
* Vehicles with high expected demand
* Acquisition opportunities
* Vehicles requiring pricing/action

The dashboard is not intended to replace the acquisition workflow.

Its purpose is to help dealers quickly understand:

> **Where is money being made, where is money being trapped, and what should I act on?**

---

## 2. Vehicle Acquisition Workspace

The primary workflow is a ranked list of vehicles available for acquisition.

Example:

| Vehicle           | Acquisition Cost | Expected Retail | Days-to-Sale | Margin | Demand | Recommendation |
| ----------------- | ---------------: | --------------: | -----------: | -----: | -----: | -------------- |
| 2022 Honda Accord |          $21,000 |         $27,800 |           31 | $3,900 |   High | BUY            |
| 2021 Toyota Camry |          $23,500 |         $28,000 |           42 | $2,700 |   High | BUY            |
| 2020 Ford Escape  |          $20,800 |         $25,000 |           67 | $1,600 | Medium | NEGOTIATE      |
| 2019 BMW 3 Series |          $25,000 |         $28,500 |           82 |   $900 |    Low | PASS           |

Dealers can filter the opportunity set by:

* Budget
* Make/model
* Vehicle segment
* Mileage
* Location
* Expected days-to-sale
* Expected margin
* Demand score
* Risk

---

# 4. Vehicle Intelligence

Clicking on a vehicle opens a detailed analysis.

The dealer can see:

### Demand

* Local demand
* Historical sales velocity
* Similar vehicles sold
* Search/market activity
* Demand trend

### Pricing

* Estimated retail price
* Competitive market price
* Acquisition price
* Expected margin
* Maximum recommended purchase price

### Sales velocity

The system estimates the probability that the vehicle will sell within:

* 30 days
* 45 days
* 60 days
* 90 days

This is more useful than predicting a single number because dealers care about **inventory aging risk**.

---

# 5. Maximum Buy Price

One of the most actionable outputs is:

> **What is the maximum price I should pay for this vehicle?**

Instead of merely predicting the retail value, the system works backward.

Conceptually:

```text
Expected Retail Price
        -
Reconditioning Cost
        -
Selling Costs
        -
Target Gross Profit
        -
Risk Adjustment
        =
Maximum Acquisition Price
```

This allows the dealer to use the system during an auction or negotiation.

For example:

```text
Expected retail price        $27,800
Estimated recon cost          $1,200
Selling/other costs             $600
Target profit                 $3,500
Risk adjustment                  $700
--------------------------------------
Maximum acquisition price    $21,800
```

If the auction price is $20,500:

> **BUY**

If the auction price is $22,500:

> **NEGOTIATE / PASS**

This makes the product directly connected to the dealer's financial outcome.

---

# 6. Recommendation Engine

The final recommendation is intentionally simple.

### BUY

The vehicle has attractive expected economics and acceptable risk.

### NEGOTIATE

The vehicle could be attractive, but only below a certain acquisition price.

### PASS

Expected sale velocity, margin, demand, or risk does not justify the acquisition.

The complexity stays behind the scenes.

The dealer receives a simple decision.

---

# AI / ML Approach

The proposed architecture separates the system into two layers.

## Layer 1 — Prediction

Machine learning models estimate:

### A. Probability of sale

Probability that a vehicle sells within a given period.

For example:

```text
P(sale ≤ 30 days) = 68%
P(sale ≤ 45 days) = 82%
P(sale ≤ 60 days) = 91%
```

This can be approached using classification or survival-analysis techniques.

A survival approach is particularly relevant because the underlying business question is:

> "How long will this vehicle remain on the lot?"

rather than simply:

> "Will it sell?"

---

### B. Expected retail price

Predict the likely retail selling price using features such as:

* Make
* Model
* Year
* Mileage
* Trim
* Location
* Vehicle condition
* Historical market prices
* Competitive listings

---

### C. Expected costs

Estimate:

* Acquisition cost
* Reconditioning
* Transportation
* Selling costs
* Other dealer-specific costs

---

### D. Risk

Identify vehicles with elevated risk of:

* Long inventory duration
* Low demand
* High depreciation
* Large pricing uncertainty
* Poor local-market fit

---

# Layer 2 — Decision / Optimization

The prediction layer should not directly decide whether a vehicle should be purchased.

Instead, predictions are fed into a decision engine.

For example:

```text
Predicted sale velocity
Predicted retail price
Expected costs
Expected margin
Risk
Dealer budget
Dealer inventory
Dealer target margin
Dealer aging threshold
        ↓
Optimization
        ↓
BUY / NEGOTIATE / PASS
```

This distinction is important.

A vehicle with a 70% probability of selling within 45 days may be attractive for one dealer but unattractive for another dealer whose target is 30 days.

Therefore, the final decision should incorporate **dealer-specific preferences and constraints**.

---

# Use of LLM / AI Agent

An LLM should not be responsible for the numerical predictions.

The predictive models are better suited for structured numerical tasks.

The LLM can instead provide an intelligent interface over the underlying decision engine.

For example, a dealer could ask:

> "Why are you recommending that I avoid this BMW?"

The system can retrieve the relevant predictions and respond:

> "The vehicle is priced 8% above comparable local inventory and has an estimated 74-day time-to-sale. At the current acquisition price, expected gross profit is only $900. Your dealership's target is $3,000+, so I recommend passing."

The LLM can therefore be used for:

* Natural-language queries
* Explanation of recommendations
* What-if analysis
* Tool orchestration
* Summarization
* Conversational exploration of inventory

The LLM is **not the source of truth for the underlying financial calculations**.

---

# Example What-If Analysis

A useful conversational capability would be:

> "What happens if I can negotiate this vehicle down by $1,500?"

The system retrieves the existing prediction and recalculates the economics.

```text
Current acquisition price: $22,500
Negotiated price: $21,000

Expected retail price: $27,800
Expected total cost: $23,900 → $22,400

Expected gross profit:
$3,900 → $5,400

Recommendation:
PASS → BUY
```

This turns the product from a reporting dashboard into a **decision-support tool**.

---

# Product Metrics

The primary business metric should measure whether the product improves inventory economics.

## North-star / primary outcome

### Average Days-on-Lot

Target:

**72 → <45 days**

This directly measures inventory velocity.

However, optimizing only for days-on-lot could create a bad incentive: dealers could sell vehicles quickly by sacrificing margin.

Therefore, a second outcome metric is essential.

### Gross Profit per Vehicle

Target:

**+10% or more**

The product should therefore optimize for a balance between:

> **Speed × Margin × Risk**

---

# Supporting Metrics

### Acquisition metrics

* % of recommended vehicles purchased
* Recommendation acceptance rate
* Average acquisition price vs recommended maximum
* Number of vehicles evaluated

### Inventory metrics

* Average days-on-lot
* % inventory >45 days
* % inventory >60 days
* Inventory turnover

### Financial metrics

* Gross profit / vehicle
* Gross profit / inventory dollar
* Markdown frequency
* Loss-making vehicles

### Model metrics

* Sale-time prediction accuracy
* Price prediction error
* Calibration of sale probability
* Recommendation precision
* False-positive BUY recommendations

---

# Key Product Trade-off

A major trade-off is:

> **Should the system optimize for fastest sale or highest margin?**

Optimizing exclusively for speed may recommend vehicles that sell quickly but generate low profit.

Optimizing exclusively for margin may lead dealers to hold expensive inventory for too long.

Therefore, the system should optimize for **risk-adjusted expected profit over a target inventory horizon**.

Conceptually:

```text
Expected Value
=
Expected Gross Profit
-
Expected Holding Cost
-
Risk Cost
```

This creates a much closer connection between the model and the dealer's actual economics.

---

# Prototype

The prototype demonstrates the end-to-end acquisition workflow rather than attempting to build a production-grade ML system.

The primary screens are:

1. **Inventory Intelligence Dashboard**
2. **Acquisition Opportunities**
3. **Vehicle Detail / Intelligence**
4. **Buy / Negotiate / Pass Recommendation**
5. **What-If / Negotiation Analysis**

The prototype uses synthetic/sample data to demonstrate the product experience and decision logic.

The purpose of the prototype is to validate:

> **Can we make a traditionally intuition-driven vehicle acquisition decision more data-driven, transparent, and actionable?**

---

# Assumptions

Because this was a product case/prototype rather than a production deployment, several assumptions were made.

### Data availability

The solution assumes access to:

* Historical dealership sales
* Vehicle-level transaction data
* Auction inventory
* Local market listings
* Vehicle attributes
* Acquisition prices
* Retail prices
* Reconditioning costs

### Market granularity

Demand should ideally be modeled at a sufficiently local level because vehicle demand can vary substantially between markets.

### Dealer customization

Different dealers may have different:

* Target margins
* Inventory budgets
* Preferred vehicle segments
* Risk tolerance
* Aging thresholds

Therefore, recommendations should not be globally identical across dealerships.

---

# What I Would Build Next

If taking this from prototype to production, I would prioritize:

### Phase 1 — Data foundation

Build a normalized vehicle/VIN data model and establish reliable historical inventory and sales data.

### Phase 2 — Prediction

Develop:

* Sale-time model
* Retail-price model
* Reconditioning-cost model
* Risk model

### Phase 3 — Decision engine

Introduce dealer-specific:

* Budget constraints
* Target margins
* Inventory limits
* Aging thresholds

### Phase 4 — Workflow integration

Integrate recommendations directly into the dealer's acquisition workflow so that the dealer can evaluate vehicles while sourcing inventory.

### Phase 5 — Learning loop

Capture:

```text
Recommendation
      ↓
Dealer Decision
      ↓
Vehicle Purchased?
      ↓
Actual Sale
      ↓
Actual Days-on-Lot
      ↓
Actual Gross Profit
      ↓
Model Feedback
```

This creates a continuous learning loop and allows the system to improve over time.

---

# Key Takeaway

The central idea behind this project is not simply:

> **"Use AI to analyze used cars."**

It is:

> **"Move used-car acquisition from intuition-driven purchasing toward data-driven, risk-adjusted decision making."**

The product sits between **market intelligence, predictive analytics, and dealer decision-making**, ultimately answering the question that matters most:

### "Should I buy this car, and if so, what should I pay for it?"
