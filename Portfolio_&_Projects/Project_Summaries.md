# Project Summaries - Nurlaila Tahawan (นูรไลลา ตะฮาวัน)

---

### Project 1: Queery Chinook Database (Relational SQL Triage)
* **Live Notion Case Study:** [chip-curler-7a4.notion.site/Queery-Chinook-Database](https://chip-curler-7a4.notion.site/Queery-Chinook-Database-3f87c0a9a17a8098a683e490efeffa55)
* **Tech Stack:** SQL (SQLite / PostgreSQL), Relational Database Joins, Aggregate Functions
* **Key Achievements:**
  - Designed multi-table queries joining up to 5 tables (`customers`, `invoices`, `employees`, `tracks`, `genres`).
  - Directly joined the `customers` table to `employees` via `SupportRepId` to calculate spending per client alongside employee account management workload.
  - Formulated advanced filtering queries utilizing `LIKE` pattern matching, `GROUP BY`, and `ORDER BY Total_Spent DESC LIMIT 5`.

---

### Project 2: ID Card Parser & Thai-to-English Date Normalization
* **Live Notion Case Study:** [chip-curler-7a4.notion.site/Project-ID-Card-Parser](https://chip-curler-7a4.notion.site/Project-ID-Card-Parser-TH-to-EN-Date-3ec7c0a9a17a80b39f56eb9efa1382f8)
* **Tech Stack:** Regular Expressions (Regex), Google Sheets Formulae, Data Cleansing
* **Key Achievements:**
  - Applied `REGEXEXTRACT`, `REGEXMATCH`, and `REGEXREPLACE` to isolate specific data patterns (e.g., 13-digit Thai national IDs, names, postal codes) from messy unstructured text strings.
  - Programmed date transformation logic parsing non-standard Thai Buddhist Era dates (e.g. "12 ธ.ค. 2563") and outputting normalized international ISO standard dates (`YYYY-MM-DD`).

---

### Project 3: Easy Hardcode & Dynamic Query Precedence
* **Live Notion Case Study:** [chip-curler-7a4.notion.site/Easy-Hardcode-Dynamic-Query](https://chip-curler-7a4.notion.site/Easy-Hardcode-Dynamic-Query-3ec7c0a9a17a808dab83eabfa9d8398a)
* **Tech Stack:** Dynamic SQL / Query Functions, Boolean Logic, Spreadsheet Architecture
* **Key Achievements:**
  - Implemented dynamic parameter querying linked to interactive dropdown menus.
  - Solved operator precedence hazards by enforcing parentheses `(West OR South) AND H >= 1000`, preventing unwanted record leakage in sales transaction reports.

---

### Project 4: Interactive Web Portfolio & Diagnostic Sandbox
* **Tech Stack:** HTML5, CSS3 (Glassmorphism / Modern Tech theme), JavaScript, Git
* **Live File:** [`index.html`](../index.html)
* **Key Achievements:**
  - Engineered simulated diagnostic sandboxes for SQL querying, terminal log grep, and SLA triage.
  - Designed an interactive modal lightbox showcasing verified certificates from Microsoft Thailand, DataRockie School, and the U.S. Embassy Bangkok.