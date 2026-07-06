# ERP Architect Identity & Program Summary

## Identity

My role is not simply to administer an ERP system. I see the ERP as the **digital constitution of the company**: a single, authoritative description of physical reality.

An ERP architect is responsible for ensuring that every business event has one trustworthy representation across planning, manufacturing, warehousing, finance, procurement, logistics, EDI, reporting, and executive decision making.

The objective is not automation for its own sake. The objective is **determinism**.

Core principles:

- The physical world always wins.
- Every duplicate source of truth is technical debt.
- Master data is a strategic asset.
- Exceptions should be explicit, measurable, and temporary.
- Automation should eliminate judgment where judgment adds no value.
- Executives deserve financial statements that reflect operational reality.

---

# Long-Term Vision

Transform Sellars from a collection of loosely coupled applications into an integrated manufacturing platform where:

- ERP reflects factory reality.
- Planning is trustworthy.
- Inventory is accurate.
- Costing reflects manufacturing physics.
- EDI operates without human intervention.
- Analytics become reliable because the underlying transactions are reliable.

---

# Major Programs

## 1. Project Reaffirmation

Goal:

Restore confidence in customer commitments.

Findings included:

- Six-day lead times were not sustainable.
- Physical capacity exceeded what ERP scheduling represented.
- Frequent expedites distorted planning.
- Forecasting and order promises conflicted.

Changes:

- Standard customer promise shifted toward realistic planning.
- Freeze fences introduced.
- Capacity-to-promise became the long-term objective.

---

## 2. Project Clarity

Purpose:

Create one trustworthy definition of dates.

Included:

- Order dates
- Requested ship dates
- Earliest ship dates
- Promise dates
- Fiscal calendar normalization
- Business calendar improvements

The philosophy became:

> Every date should answer exactly one business question.

---

## 3. Rolled Goods Transformation

The previous ordering process required substantial manual interpretation.

The redesigned workflow introduced staged progression:

Draft

↓

Committed

↓

Released

This separated customer intent from manufacturing commitment while allowing optimization before production.

---

## 4. Manufacturing Costing

One of the largest discoveries was that ERP already contained functionality that had never been fully implemented.

Work centered around:

- Labor rates
- Production rates
- Work center costing
- Alternate routings
- Alternate BOMs

The objective became making costs emerge naturally from manufacturing rather than manual adjustments.

---

## 5. Part Number Simplification

Several finished goods represented identical products with different manufacturing assumptions.

Goal:

Reduce unnecessary duplication while preserving costing accuracy through routing and manufacturing data rather than duplicated item masters.

---

## 6. EDI Modernization

The legacy process relied heavily on manual intervention.

Current direction:

850 → validation → staging → ERP → shipment → labels → ASN → invoice

Supported initiatives include:

- GS1 labels
- SSCC pallet labels
- Customer-specific cross references
- Automated exception handling
- Fastenal improvements
- Lowe's integration
- SPS Commerce modernization

---

## 7. Logistics

Shipping is treated as an extension of ERP rather than a separate process.

Projects include:

- Parcel modernization
- LTL integration
- Label generation
- Carrier mapping
- Shipment validation

---

## 8. Reporting

Reporting philosophy:

Never repair reports.

Repair the data that feeds the reports.

Power BI should become a window into reality—not a place where reality is reconstructed.

---

# Leadership Philosophy

Technology is only one layer.

The harder problems are:

- organizational incentives
- operational discipline
- trust
- governance
- master data ownership

ERP architecture therefore becomes organizational architecture.

---

# Architectural Principles

1. Single source of truth.
2. Physical execution defines system design.
3. Eliminate manual interpretation.
4. Prefer standards over customization.
5. Design for scale.
6. Capture variance instead of hiding it.
7. Reduce complexity whenever possible.
8. Make failures observable.

---

# Current Status

Significant progress has been made in:

- MRP redesign
- scheduling philosophy
- EDI automation
- logistics integration
- costing discovery
- fiscal calendar cleanup
- SQL reporting
- manufacturing master data

Remaining work focuses on:

- production costing implementation
- routing simplification
- continued EDI partner rollout
- planning optimization
- manufacturing standardization
- executive KPI framework

---

# Guiding Statement

> An ERP should not describe how we wish the business worked. It should faithfully describe how the business actually works—and make improving that business easier every day.
