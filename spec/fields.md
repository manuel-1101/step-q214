
# STEP-Q214 Field Registry

Version: v0.1  
Status: Draft  
Maintainer: holydraft

---

## 1. Purpose

This document defines all registered STEP-Q214 fields, including their
semantics, data types, formats, and interpretation rules.

Only fields defined in this registry are considered normative.

---

## 2. General Rules

### 2.1 Naming

- All fields shall use the prefix `Q_`
- Uppercase letters and underscores only
- No spaces or special characters

### 2.2 Data Types

| Type    | Description                         |
|---------|-------------------------------------|
| String  | UTF-8 text                          |
| Integer | Signed integer (base 10)            |
| Float   | Decimal number (dot as separator)   |
| Enum    | Controlled vocabulary               |
| Date    | ISO 8601 format (YYYY-MM-DD)        |
| Bool    | true / false                        |

### 2.3 Units

All units shall follow SI conventions unless explicitly defined.

Currency values shall be expressed in ISO 4217 format where applicable.

---

## 3. Core Fields

---

### Q_PART_ID

**Type:** String  
**Required:** No  
**Description:**  
Unique identifier for the part or assembly within the RFQ context.

**Format:**  
Free text, recommended pattern: `[A-Z0-9-_]+`

**Examples:**

- HD-2026-001
- PART_4711
- A-9832-B

**Fallback:**  
If missing, internal system IDs shall be assigned.

**Common Errors:**

- Duplicate IDs within one RFQ
- Use of whitespace

---

### Q_MATERIAL

**Type:** String  
**Required:** No  
**Description:**  
Primary material designation of the part.

**Format:**  
Standardized material names according to applicable norms where possible.

**Examples:**

- EN AW-6082
- S355MC
- 1.4301
- PA12

**Fallback:**  
Material must be clarified manually.

**Common Errors:**

- Trade names without specification
- Incomplete alloy definitions

---

### Q_PRIMARY_PROCESS

**Type:** Enum  
**Required:** No  
**Description:**  
Main manufacturing process used for cost estimation.

**Reference:**  
See `spec/enumerations.md`

**Examples:**

- milling
- laser_cutting
- additive

**Fallback:**  
Derived from geometry if possible.

**Common Errors:**

- Multiple conflicting processes
- Process not matching geometry

---

### Q_QUANTITY

**Type:** Integer  
**Required:** No  
**Unit:** pcs  
**Description:**  
Requested order quantity.

**Format:**  
Positive integer (>0)

**Examples:**

- 1
- 50
- 1000

**Fallback:**  
Assumed quantity = 1

**Common Errors:**

- Zero or negative values
- Decimal quantities

---

### Q_TOLERANCE_CLASS

**Type:** Enum  
**Required:** No  
**Description:**  
General dimensional tolerance class.

**Reference:**  
See `spec/enumerations.md`

**Examples:**

- ISO2768-m
- ISO2768-f

**Fallback:**  
Default: ISO2768-m

**Common Errors:**

- Mixed tolerance systems
- Missing drawing reference

---

### Q_SURFACE

**Type:** Enum  
**Required:** No  
**Description:**  
Surface treatment or finish requirement.

**Reference:**  
See `spec/enumerations.md`

**Examples:**

- anodized
- powder_coated
- polished

**Fallback:**  
Assumed: raw / untreated

**Common Errors:**

- Ambiguous terms (e.g. "nice finish")
- Missing specification for visible parts

---

### Q_DRAWING_REFERENCE

**Type:** String  
**Required:** No  
**Description:**  
Reference to external drawing or PMI document.

**Format:**  
File name or document ID.

**Examples:**

- DWG-1023
- 2026-ALU-BRKT-A1.pdf

**Fallback:**  
Geometry and PMI are authoritative.

**Common Errors:**

- Broken references
- Outdated revisions

---

### Q_TARGET_PRICE

**Type:** Float  
**Required:** No  
**Unit:** ISO 4217 currency  
**Description:**  
Target price per unit.

**Format:**  
Decimal, dot as separator.

**Examples:**

- 4.50
- 12.00

**Fallback:**  
Ignored for automatic pricing.

**Common Errors:**

- Total price instead of unit price
- Missing currency context

---

### Q_DELIVERY_DATE

**Type:** Date  
**Required:** No  
**Description:**  
Requested delivery date.

**Format:**  
YYYY-MM-DD

**Examples:**

- 2026-03-15
- 2026-06-01

**Fallback:**  
Standard lead time applies.

**Common Errors:**

- Locale-dependent formats
- Impossible dates

---

### Q_CERTIFICATE

**Type:** Enum  
**Required:** No  
**Description:**  
Material or compliance certificate requirement.

**Reference:**  
See `spec/enumerations.md`

**Examples:**

- EN10204-3.1
- EN10204-2.1

**Fallback:**  
No certificate assumed.

**Common Errors:**

- Non-standard certificates
- Missing scope definition

---

### Q_PACKAGING

**Type:** Enum  
**Required:** No  
**Description:**  
Packaging requirements.

**Reference:**  
See `spec/enumerations.md`

**Examples:**

- bulk
- individual
- vacuum

**Fallback:**  
Standard bulk packaging.

**Common Errors:**

- Missing protection requirements
- Over-specification

---

### Q_COMMENTS

**Type:** String  
**Required:** No  
**Description:**  
Free-text remarks for special requirements.

**Format:**  
UTF-8 text, max. 1024 characters recommended.

**Examples:**

- Visible surface on side A
- Protect edges

**Fallback:**  
Ignored in automation.

**Common Errors:**

- Critical info hidden in comments
- Unstructured requirements

---

## 4. Deprecation Policy

Deprecated fields shall be marked explicitly.

They remain supported for at least one major version.

---

## 5. Change Management

All new fields must be proposed via pull request.

Each proposal shall include:

- Technical description
- Business rationale
- Examples
- Backward compatibility analysis
