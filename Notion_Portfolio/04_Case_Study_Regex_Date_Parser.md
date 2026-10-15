# 04. Case Study: Regex Text Parser & Thai-to-EN Date Normalizer

> **Notion Page Icon:** 🔍  
> **Tags:** `Regular Expressions` `Regex` `Data Cleansing` `Data Pipelines` `Google Sheets`  
> **Live Notion URL:** [chip-curler-7a4.notion.site/Project-ID-Card-Parser-TH-to-EN-Date-3ec7c0a9a17a80b39f56eb9efa1382f8](https://chip-curler-7a4.notion.site/Project-ID-Card-Parser-TH-to-EN-Date-3ec7c0a9a17a80b39f56eb9efa1382f8)  
> **Breadcrumb:** `Home / Case Studies / Regex Text Parser`

---

## 🎯 Project Overview & Problem Statement

In customer operations and data ingestion, user-submitted information rarely arrives in a clean format:
* Users copy-paste identification numbers with irregular spaces, hyphens, or prefixes (`ID: 1-1002-00345-67-8` vs. `ID1100200345678`).
* Thai users frequently input dates in the local **Buddhist Era (พ.ศ.)** with Thai month abbreviations (e.g. `12 ธ.ค. 2563`), which causes errors in standard databases that expect international ISO-8601 timestamps (`2020-12-12`).

This project demonstrates how I used **Regular Expressions (Regex)** and structured formula logic to automatically clean, validate, and normalize messy text records into standardized data structures.

---

## 🛠️ The Regex Engineering Toolkit

### 1. Extracting Thai National ID (13 Digits)
Users frequently enter ID numbers embedded inside paragraphs of text.
* **Regex Pattern:** `\b\d{13}\b` or `\d{1}[\s-]?\d{4}[\s-]?\d{5}[\s-]?\d{2}[\s-]?\d{1}`
* **Formula:**
  ```excel
  =REGEXEXTRACT(A2, "\d{13}")
  ```
* **Explanation:** Captures the continuous 13-digit sequence regardless of surrounding text.

---

### 2. Validating Record Formats (`REGEXMATCH`)
Validates that incoming support ticket codes conform to expected company prefixes:
* **Formula:**
  ```excel
  =IF(REGEXMATCH(A2, "^(VIP|CORP|STD)-\d{4}$"), "Valid Ticket", "Invalid Format")
  ```
* **Explanation:** `^` asserts line start, followed by permitted department tags (`VIP`, `CORP`, `STD`), a hyphen, and 4 digits to line end (`$`).

---

### 3. Transforming Thai Buddhist Era (พ.ศ.) to International ISO-8601

Thai dates are structured as: `[Day] [Thai Month Abbreviation] [BE Year]` (e.g., `12 ธ.ค. 2563`).

To convert this into ISO format (`2020-12-12`):
1. **Extract Day (`DD`):** Ensure zero-padded 2 digits.
2. **Translate Month:** Map Thai month strings to two-digit numbers (`ธ.ค.` $\rightarrow$ `12`).
3. **Normalize Year:** Convert Buddhist Era year to Gregorian CE year by subtracting 543 ($2563 - 543 = 2020$).

#### Unified Google Sheets Transformation Formula:
```excel
=LET(
  raw, A2,
  day, TEXT(VALUE(REGEXEXTRACT(raw, "^\d+")), "00"),
  month_text, REGEXEXTRACT(raw, "[ก-๙\.]+"),
  be_year, VALUE(REGEXEXTRACT(raw, "\d{4}$")),
  ce_year, be_year - 543,
  month_num, SWITCH(month_text, 
    "ม.ค.", "01", "ก.พ.", "02", "มี.ค.", "03", "เม.ย.", "04", 
    "พ.ค.", "05", "มิ.ย.", "06", "ก.ค.", "07", "ส.ค.", "08", 
    "ก.ย.", "09", "ต.ค.", "10", "พ.ย.", "11", "ธ.ค.", "12", "00"),
  ce_year & "-" & month_num & "-" & day
)
```

---

## 🧪 Verification & Test Cases

| Raw Unstructured Input | Extracted ID | Normalized Date (ISO) | Validation Status |
| :--- | :---: | :---: | :---: |
| `ชื่อ นายสมชาย เลขประจำตัว 1100200345678 เกิด 12 ธ.ค. 2563` | `1100200345678` | `2020-12-12` | ✅ Verified |
| `Ticket Ref: VIP-9042 เกิดวันที่ 5 ม.ค. 2566 ID: 3901100456123` | `3901100456123` | `2023-01-05` | ✅ Verified |
| `สมาชิกใหม่ 28 ก.พ. 2567 / เลข 1940200112234` | `1940200112234` | `2024-02-28` | ✅ Verified |

---

## 🛡️ Real-World Application to L1 Support & Data Ops

1. **Automated User Verification:** Automatically parsing national IDs and account codes from raw user emails and customer tickets before passing them into internal backend lookup systems.
2. **Preventing Database Ingestion Failures:** Pre-validating date strings before they hit SQL `INSERT` statements, preventing database type mismatch errors.
3. **Log Sanitization:** Regex replacement (`REGEXREPLACE`) to mask personally identifiable information (PII) such as credit card numbers or passwords before uploading support logs to public ticket trackers.
