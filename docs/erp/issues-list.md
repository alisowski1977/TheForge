# ERP Issues Register

This document tracks ERP-related issues where the system behavior, business process, or integration design does not match operational requirements. The intent is to preserve institutional knowledge, identify interim controls, and define long-term remediation paths.

## ERP-001 — No Backorder Process for Partial Shipments

**Module:** Order Entry / Shipping  
**Priority:** High  
**Status:** Open

### Issue
The ERP does not have a standardized backorder process. When a customer order is only partially shipped, Customer Service closes the original sales order and creates a new sales order for the remaining balance.

To distinguish the new order, Customer Service appends a suffix to the customer purchase order number, such as `PO12345(1)`.

### Business Impact
- Customer purchase order numbers no longer match the original EDI transaction.
- ASNs and invoices may fail validation or be rejected because the transmitted PO number differs from the customer's original purchase order.
- Manual intervention increases for Customer Service, EDI, and invoicing.
- Customer-facing documents become inconsistent with the customer's purchasing system.

### Root Cause
There is no defined ERP backorder process that preserves the original customer purchase order while maintaining the relationship between the shipped quantity and the remaining balance.

### Interim Recommendation
For EDI customers, manually enter affected shipment and invoice transactions through the trading partner portal until a standardized backorder process is implemented.

### Long-Term Recommendation
Implement a standardized ERP backorder process that:

- Preserves the original customer purchase order number.
- Maintains the relationship between the original order and any remaining balance.
- Supports partial shipments without requiring manual recreation of sales orders.
- Produces EDI-compliant ASNs and invoices that reference the customer's original purchase order.

---

## ERP-002 — Inconsistent Ship-To Address Source Logic

**Module:** Order Entry / Shipping  
**Priority:** High  
**Status:** Open

### Issue
Order Entry uses inconsistent logic to determine the Ship-To address. Depending on the transaction, screen, or downstream process, the Ship-To address may be read from:

- `shipment_master`
- `SO_Master`
- `customer_master`

This makes it unclear which address is authoritative for a given order or shipment.

### Business Impact
- Users cannot reliably predict which Ship-To address will be used.
- Incorrect shipment documents, labels, ASNs, invoices, or customer-facing paperwork may be generated.
- Troubleshooting becomes difficult because the visible address may not match the address used downstream.
- Customer Service, Shipping, and EDI may each be looking at different versions of the same address.

### Root Cause
Ship-To address logic is fragmented across multiple ERP tables without a clearly defined system of record.

### Interim Recommendation
Document which screens and processes read from each table. For EDI or customer-sensitive orders, manually verify Ship-To accuracy before shipment.

### Long-Term Recommendation
Define a single authoritative Ship-To source for each stage of the order lifecycle and standardize the ERP logic so Order Entry, Shipping, ASN generation, and invoicing all reference the correct address consistently.

---

## ERP-003 — Lowe's Orders Use Customer Ship Dates Instead of ERP Lead-Time Dates

**Module:** Order Entry / Scheduling / EDI  
**Priority:** High  
**Status:** Open

### Issue
Lowe's orders are expected to ship based on the actual Lowe's ship dates provided by the customer. These dates should be treated as the customer commitment dates, rather than being replaced by Sellars' internally calculated lead-time dates.

The ERP currently relies on internal lead-time logic that can produce dates that do not align with Lowe's expectations.

### Business Impact
- Orders may be scheduled using incorrect ship dates.
- There is increased risk of late shipments, early shipments, or retailer compliance failures.
- Customer Service and Scheduling must manually reconcile ERP dates against Lowe's dates.
- EDI documents may reflect dates that do not match the customer's intended shipping schedule.

### Root Cause
The ERP defaults to internally calculated lead-time dates rather than honoring the actual customer ship dates transmitted or provided by Lowe's.

### Interim Recommendation
For Lowe's orders, Customer Service and Scheduling should reference the actual Lowe's ship dates instead of relying solely on the ERP-calculated lead-time dates.

### Long-Term Recommendation
Modify the order import and scheduling process so Lowe's ship dates are preserved and used as the authoritative customer commitment dates. Internally calculated dates may remain available for planning, but they should not override Lowe's actual requested ship dates.

---

## ERP-004 — Lowe's Change Orders Are Not Supported

**Module:** EDI / Order Entry  
**Priority:** High  
**Status:** Open

### Issue
Lowe's change orders do not work reliably through the ERP/EDI process. The intended process was for Lowe's change orders to be handled through the Lowe's supplier portal only, but that portal-only process was not set up or enforced.

### Business Impact
- Change orders may not be processed correctly.
- There is increased risk of shipping incorrect quantities, dates, or products.
- Customer Service must manually reconcile order changes.
- The authoritative version of the order becomes unclear.
- The business may assume change orders are being handled when the intended portal-only control is not actually in place.

### Root Cause
The ERP does not support Lowe's change order processing reliably, and the expected portal-only workflow was not implemented as the standard process.

### Interim Recommendation
Process all Lowe's order changes manually through the Lowe's supplier portal. Do not rely on ERP processing for Lowe's change orders until a supported workflow is established.

### Long-Term Recommendation
Establish and document the standard business process for Lowe's order changes. Either:

- Fully implement portal-only processing with appropriate operational controls, or
- Enhance the ERP/EDI integration to support Lowe's change order transactions while maintaining synchronization with the original sales order.

The key governance question is why the ERP was allowed to accept or attempt to process Lowe's change orders if the business decision was portal-only.

---

## ERP-005 — Tax Codes Are Not Carried Forward to Sales Orders

**Module:** Order Entry / Taxation  
**Priority:** High  
**Status:** Open

### Issue
Tax codes are not consistently carried forward to sales orders during order creation. As a result, orders may be created without the appropriate tax code assigned.

### Business Impact
- Sales tax may be calculated incorrectly or omitted.
- Customer Service or Accounting must manually review and correct affected orders.
- Invoicing errors and customer disputes may occur.
- Tax reporting and compliance may be affected if tax is applied incorrectly.

### Root Cause
The order creation process is not consistently inheriting the appropriate tax code from the designated customer or transaction setup.

### Interim Recommendation
Review and validate the tax code on all applicable sales orders before invoicing. Manually assign the correct tax code when it is missing.

### Long-Term Recommendation
Modify the order entry process to consistently inherit the correct tax code from the designated source during sales order creation. Establish a single authoritative source for tax code assignment and implement validation to prevent orders from being created without a valid tax code where required.
