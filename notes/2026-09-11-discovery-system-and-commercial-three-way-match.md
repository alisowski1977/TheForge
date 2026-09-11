# Discovery System and Commercial Three-Way Match

## Core realization

The victory was not discovering a clever rule. The victory was fostering the conditions in which the rule could be discovered, challenged, tested, and made institutional.

The emerging operating loop is:

> Give capable people access → let them investigate → make discoveries visible → connect discovery to domain experts → test against reality → institutionalize what survives.

This is more important than any one automation rule because it creates a repeatable way for the organization to discover how its own processes actually work.

## Order intake philosophy

The objective is not maximum automation or headcount reduction. It is maximum appropriate allocation of work.

Humans should be used for judgment, not repetition.

A good automation boundary is:

> **Automate certainty. Route ambiguity. Learn from the boundary.**

If the same human exception is resolved the same way over and over, ask whether it is still judgment or whether the organization has discovered a rule.

## Payment terms discovery

Payment terms were nearly discarded from PO intake because the customer master already contained terms.

Anastasia investigated actual incoming POs and found a mismatch on approximately 9.5% of them.

Susan explained the operational process:
- CSR identifies the mismatch.
- CSR goes back to the customer.
- A corrected PO may be requested.
- Often the customer verbally confirms the intended terms instead.

This means payment-term mismatch is not a field-normalization problem. It is a legitimate business exception that should be routed to a human.

The intended pattern is:

```text
PO.PaymentTerms != CustomerMaster.PaymentTerms
    → Create exception
    → Human resolves
    → Record resolution
```

The human resolution itself becomes useful institutional knowledge and process evidence.

## Shadow-mode validation

Before production writes, run the order-intake process one day behind production in Dev.

For each incoming PO, compare what the new process would have done with what was actually entered in production.

Useful disagreement classes:

| Shadow result | Production behavior | Meaning |
|---|---|---|
| Accept | Accepted | Expected agreement |
| Exception | Human handled exception | Correct detection |
| Exception | Accepted normally | Possible false positive or undocumented rule |
| Accept | Human intervention | Dangerous false negative |
| Different interpretation | Order entered | Business-rule discovery opportunity |

Production behavior is evidence, not automatically ground truth.

The goal is not to train the new system to imitate every existing habit. The goal is to determine why the organization behaved differently and decide whether that behavior represents a valid rule, stale master data, an undocumented exception, or an error.

## Release philosophy

A reasonable first release could occur when roughly 75% of orders are confidently straight-through while the remaining 25% are safely routed to humans.

The important distinction is:

> 75% definitely right + 25% kicked out is acceptable.
>
> 97% probably right + 3% spectacular failure is not.

For material exceptions, false negatives should approach zero before removing human review.

Useful metrics:
- Straight-through rate
- Human-touch rate
- False-negative rate
- False-positive rate
- Exception rate by type
- Time to resolve exceptions
- Repeat exception rate
- Decision density = meaningful human decisions / human touches

## Commercial item resolution

Part-number matching is not the right abstraction.

The problem is **commercial identity resolution**.

Some customers:
- Send recognizable Sellars part numbers.
- Send their own customer part numbers.
- Send no reliable part number at all.
- Use UOM values such as EA and CASE in ways that are truly different.
- Use UOM values such as EA and CASE differently even when they mean the same commercial unit.

The customer-stated UOM is therefore evidence, but not necessarily truth.

### Sellars commercial three-way match

The strongest practical insight is:

> **Part + Price + FOB**

This is the commercial three-way match.

Sellars has six standard price lists plus roughly one hundred special price lists. The combination of:
- candidate part identity,
- price from the customer's assigned pricing structure,
- and freight responsibility / delivered vs pickup treatment,

can strongly constrain the commercial identity of an incoming line.

Example:

```text
Customer PO:
  Customer Part: ABC-447
  UOM: EA
  Price: $42.50
  Freight: Delivered

Sellars pricing:
  Candidate 54221 EACH delivered: $3.54
  Candidate 54221 CASE delivered: $42.50

Commercial interpretation:
  54221 / CASE is the only commercially consistent candidate.
```

The customer's `EA` may simply mean one Sellars CASE for that customer.

The mapping should preserve both sides:

```text
CUSTOMER INTENT
CustomerPart: ABC-447
CustomerUOM: EA
CustomerQty: 10
POPrice: 42.50
Freight: Delivered

INTERPRETATION
SellarsPart: 54221
SellarsUOM: CASE
SellarsQty: 10

MAPPING
Customer + CustomerPart + CustomerUOM
→ SellarsPart + SellarsUOM + QuantityRule
```

Do not discard the original customer representation after normalization.

## Resolution by constraints, not AI confidence

The goal should not be:

> "AI is 98.7% confident this is part 54221."

A stronger rule is:

> **Exactly one commercially consistent candidate exists.**

Evidence may include:
- Customer part mapping
- Description
- Assigned customer price list
- PO unit price
- Delivered/pickup price
- Freight responsibility
- Historical purchase behavior
- Known customer UOM semantics

Then:

```text
0 candidates  → Human
1 candidate   → Resolve if deterministic requirements are satisfied
>1 candidates → Human
```

Unknown or ambiguous cases should be allowed to say:

> **I don't know yet.**

That is a feature, not a failure.

## Learning loop

When Anastasia resolves an unknown customer part or UOM interpretation, do not merely resolve that order.

Capture the resolution as a proposed durable mapping.

```text
Unknown representation
    ↓
Human investigation
    ↓
Resolution
    ↓
Confirmed commercial mapping
    ↓
Future occurrences become deterministic
```

This is the broader pattern:

> **Ambiguity → human investigation → institutional knowledge → determinism**

AI can assist with discovery, extraction, explanation, and candidate suggestion, but it does not have to remain in the final critical path.

## Role of AI

AI has often occupied the gaps where the business model was not yet understood.

As understanding improves, many apparently agentic problems reduce to deterministic rules, event models, comparisons, and exception routing.

Useful principle:

> **Use AI to discover the rule. Do not automatically make AI the rule.**

AI can remain valuable for:
- extracting messy customer intent from documents,
- suggesting likely mappings,
- explaining exceptions,
- clustering recurring problems,
- helping humans investigate unfamiliar cases.

But the end goal is an operational system, not necessarily an autonomous agent.

## Organizational design insight

The important accomplishment is not that one person discovered `Part + Price + FOB`.

It is that the environment allowed:
- Anastasia to investigate data instead of merely following a specification,
- Andy's assumptions to be challenged by evidence,
- Susan's operational knowledge to be incorporated,
- findings to become durable knowledge,
- and the software to improve as the organization's understanding improves.

If the process depended on Andy already knowing every rule, payment terms would have been lost.

Instead, the system was capable of correcting its designer.

That is the stronger design.

## Forge principles reinforced

- Reduce Ambiguity; Increase Determinism.
- Make Knowledge Institutional.
- Responsibility Requires Authority.
- Establish Measurable Operations.
- Complexity Must Earn Its Existence.
- Humans should be employed for judgment, not tolerance for repetition.
- Evidence before attribution.
- Automate certainty. Route ambiguity. Learn from the boundary.

## Closing thought

The final rule may sound almost embarrassingly simple:

> **Part. Price. FOB.**

But finding the right three variables required creating a system in which the organization could discover them.

That is the actual victory.
