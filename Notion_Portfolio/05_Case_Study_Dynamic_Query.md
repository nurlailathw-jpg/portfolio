# 05. Case Study: Dynamic Querying & Boolean Operator Precedence

> **Notion Page Icon:** ⚡  
> **Tags:** `Dynamic SQL` `Boolean Logic` `Spreadsheet Architecture` `Data Reporting`  
> **Live Notion URL:** [chip-curler-7a4.notion.site/Easy-Hardcode-Dynamic-Query-3ec7c0a9a17a808dab83eabfa9d8398a](https://chip-curler-7a4.notion.site/Easy-Hardcode-Dynamic-Query-3ec7c0a9a17a808dab83eabfa9d8398a)  
> **Breadcrumb:** `Home / Case Studies / Dynamic Query & Precedence`

---

## 🎯 Project Overview & The "Logic Leak" Bug

In data filtering, writing simple queries often leads to unintended logic leaks when multiple `OR` and `AND` operators are combined. 

In standard SQL and spreadsheet query engines:
$$\text{AND has higher operator precedence than OR}$$

If parentheses are omitted, the query engine evaluates the conditions in an order that violates business requirements.

In this project, I:
1. Demonstrated and diagnosed the common **boolean precedence trap**.
2. Designed a robust solution using **parentheses enforcement**.
3. Built a **dynamic parameterized query system** where non-technical stakeholders can filter sales transactions through dropdown controls without editing code.

---

## ⚠️ The Operator Precedence Trap

### The Business Requirement:
*"Retrieve all transactions from either the **West** OR **South** regions that had sales volume **$\ge 1,000$**."*

### ❌ The Flawed Query (Without Parentheses):
```sql
SELECT A, B, C, D, H 
WHERE D = 'West' OR D = 'South' AND H >= 1000
```

**Why this fails:**
Because `AND` has higher precedence, the engine evaluates `D = 'South' AND H >= 1000` first. The full condition becomes:
$$\text{Condition: } (D = 'West') \quad \mathbf{OR} \quad (D = 'South' \text{ AND } H \ge 1000)$$

**Consequence:**
* All transactions from `West` are returned, **including transactions under $1,000** (e.g. $12 purchases).
* Generates false reporting numbers and corrupts downstream executive dashboards.

---

### ✅ The Correct Query (Enforced Parentheses):
```sql
SELECT A, B, C, D, H 
WHERE (D = 'West' OR D = 'South') AND H >= 1000
```

**Why this works:**
Parentheses explicitly bind the regional evaluation first, guaranteeing that only records with `H >= 1000` are returned from either target region.

---

## 🎛️ Building the Dynamic Parameterized Formula

To allow non-technical team members to filter records interactively:
* Region 1 selected in cell `K1` (e.g., `"West"`)
* Region 2 selected in cell `K2` (e.g., `"South"`)
* Minimum Sales threshold set in cell `K3` (e.g., `1000`)

### Dynamic Google Sheets `=QUERY()` Formula:
```excel
=QUERY(
  Transactions_Data!A1:H1000,
  "SELECT A, B, C, D, H 
   WHERE (D = '" & K1 & "' OR D = '" & K2 & "') 
   AND H >= " & K3 & " 
   ORDER BY H DESC",
  1
)
```

---

## 📊 Precedence Evaluation Matrix

| Record ID | Region (Col D) | Amount (Col H) | Flawed Query Result | Correct Query Result | Status |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **TX-101** | West | $4,500 | Included | Included | ✅ Correct |
| **TX-102** | West | **$120** | **INCLUDED (Bug!)** | **EXCLUDED** | 🛡️ Prevented Leak |
| **TX-103** | South | $1,250 | Included | Included | ✅ Correct |
| **TX-104** | South | $350 | Excluded | Excluded | ✅ Correct |
| **TX-105** | North | $5,000 | Excluded | Excluded | ✅ Correct |

---

## 🛡️ Real-World Application to L1 Support & Incident Response

1. **Investigating Logic Discrepancies:** When a SaaS customer submits a ticket complaining *"Your filter tool is showing orders from last month that don't match my search criteria!"*, I immediately check for missing boolean parentheses in the query generator.
2. **Dynamic Internal Tooling:** Building fast, lightweight triage sheets for the support team to filter high-priority tickets by region, status, and SLA thresholds without writing manual SQL each time.
3. **Reproducing Edge-Case Bugs:** Communicating logical flaw issues clearly to backend developers with clear boolean truth tables.
