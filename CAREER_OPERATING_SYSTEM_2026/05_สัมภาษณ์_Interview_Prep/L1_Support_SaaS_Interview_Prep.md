# L1 Support (SaaS / Enterprise) Interview Preparation Guide 🚀

This guide is specifically tailored for **Nurlaila Tahawan** interviewing for the **100% Remote L1 Support Engineer** role at an international SaaS company.

---

## Part 1: The 60-Second English Elevator Pitch ("Tell Me About Yourself")

> **Interviewer:** *"Tell me about yourself and why you're interested in this L1 Support role."*

**Your Answer (Practice this out loud):**
> "Hi, I'm Nurlaila. I graduated with a degree in English Education, and I recently completed an intensive Data Science bootcamp at DataRockie as a scholarship recipient.
> 
> My sister works in IT and was the one who first introduced me to the world of remote tech work. That inspired me to start learning—I took courses in SQL, Google Sheets, and R, and earned certificates in all of them. Today, I'm comfortable using SQL to query databases and check records, and I'm actively self-studying Python and Bash scripting.
> 
> Thanks to my teaching background, I feel completely comfortable using English every day, breaking down technical steps with patience, and staying calm under pressure. I saw that you're building a remote support team in Thailand for your SaaS platform. I'm applying because I want an entry-level L1 role where I can combine my English communication, my technical foundation, and my strong work ethic to support your team and global customers."

---

## Part 2: Addressing Background & Career Transition Smoothly

### Q: "Your degree is in English Education. Why transition into SaaS Technical Support?"
**Your Strategy:** Frame teaching as the ultimate training ground for customer support and troubleshooting!
> **Answer:**
> "Teaching and Technical Support actually share the exact same core DNA: **diagnosing user frustration, active listening, and explaining technical solutions with clarity and empathy.** In teaching, when a student struggles, you don't just give the answer; you troubleshoot *why* they are stuck and guide them step-by-step. 
> 
> At the same time, I've always had a passion for technology. I completed an intensive 100-hour Data Science bootcamp where I practiced SQL querying, Python scripting, and system logic. L1 Support is the sweet spot where my fluent English communication skills and my technical problem-solving intersect."

### Q: "Are you ready to handle high ticket volume (e.g., 40-70+ tickets/day)?"
**Your Strategy:** Emphasize stamina, organization, prioritization, and keyboard shortcuts/templates!
> **Answer:**
> "Absolutely. In my past coordination roles, I managed inquiries and requirements for hundreds of individuals across tight deadlines. I stay efficient by:
> 1. **Prioritizing ruthlessly by SLA and severity** (e.g., platform outages vs. minor UI questions).
> 2. **Mastering quick macros and canned responses**, while always personalizing the opening and closing so the customer feels genuinely heard.
> 3. **Keeping diagnostic steps structured**: verify the issue, check logs/SQL, isolate variables, and resolve or escalate with clear notes. I find fast-paced environments energizing."

### Q: "What is your current level in Python and Bash? Can you write scripts?"
**Your Strategy:** Be 100% honest, humble, and emphasize your solid SQL base + fast learning curve!
> **Answer:**
> "I want to be completely transparent: my strongest technical base right now is **SQL**, which I practiced thoroughly in my Data Science bootcamp to query records, filter logs, and run multi-table joins. 
> 
> For **Python and Bash**, I am currently studying them actively. I understand terminal navigation, basic file operations, and the core syntax of Python scripts. While I am still growing my scripting skills, I am disciplined about self-study, and I can quickly pick up your team's existing script templates, runbooks, and diagnostic commands to execute them on the job."

---

## Part 3: Technical Questions & Cheat Sheet (SQL, Python, Bash, SaaS)

### 1. SQL Essentials for L1 Support
In SaaS L1 support, SQL is typically used for **read-only database diagnostics**: checking if a user exists, finding transaction status, or verifying timestamps.

* **Find user by email or ID:**
  ```sql
  SELECT id, email, status, created_at 
  FROM users 
  WHERE email = 'customer@example.com';
  ```
* **Diagnose a failed transaction/ticket (JOIN):**
  ```sql
  SELECT u.email, o.order_id, o.status, o.error_message, o.created_at
  FROM users u
  JOIN orders o ON u.id = o.user_id
  WHERE u.id = 1042 AND o.status = 'FAILED'
  ORDER BY o.created_at DESC
  LIMIT 5;
  ```
* **Aggregate error counts per error type:**
  ```sql
  SELECT error_code, COUNT(*) AS total_failures
  FROM system_logs
  WHERE created_at >= NOW() - INTERVAL '24 HOURS'
  GROUP BY error_code
  ORDER BY total_failures DESC;
  ```

### 2. Bash / Linux Commands for Log Inspection
* `tail -f /var/log/app.log` : Stream real-time logs to watch an issue live as the customer reproduces it.
* `grep -i "error" /var/log/app.log | tail -n 20` : Search for the word 'error' (case-insensitive) and show the last 20 lines.
* `awk '{print $1, $7, $9}' access.log | grep " 500 "` : Quick check on client IP, requested URL, and 500 error status.
* `curl -I https://api.saasapp.com/health` : Check HTTP response headers and service status code.

### 3. Python for Quick Automation & Health Checks
* **Checking endpoint status / API troubleshooting:**
  ```python
  import requests

  try:
      response = requests.get("https://api.example.com/v1/status", timeout=5)
      print(f"Status Code: {response.status_code}")
      if response.status_code == 200:
          print("Service Healthy:", response.json())
      else:
          print("Issue detected:", response.text)
  except requests.exceptions.RequestException as e:
      print("Connection failed:", e)
  ```
* **Parsing a log file for 5xx errors:**
  ```python
  with open("server.log", "r") as f:
      for line in f:
          if "HTTP/1.1 500" in line or "CRITICAL" in line:
              print(line.strip())
  ```

### 4. HTTP Status Codes Cheat Sheet
* **200 OK:** Success.
* **201 Created:** New resource created (e.g., successful user signup or entity creation).
* **400 Bad Request:** Client-side syntax error or missing payload parameters.
* **401 Unauthorized:** Missing or invalid authentication token / login session expired.
* **403 Forbidden:** Authenticated, but user lacks permission to access the resource.
* **404 Not Found:** Endpoint or resource does not exist.
* **429 Too Many Requests:** Client exceeded rate limits.
* **500 Internal Server Error:** Unexpected error on backend server code.
* **502 Bad Gateway / 503 Service Unavailable:** Backend service down or gateway timeout.

---

## Part 4: Ticket Triage & De-escalation Scenarios

### Scenario A: An angry enterprise customer submits a high-priority ticket saying: *"Your platform is down and we are losing money! Fix this immediately!"*
**Steps to Answer:**
1. **Acknowledge and Empathize immediately (in English):**  
   *"Thank you for reaching out and alerting us. I completely understand how critical this workflow is for your operations, and I'm actively investigating this right now."*
2. **Investigate Quickly:**
   - Check internal status dashboards (is it an outage or isolated to this account?).
   - Reproduce or verify the endpoint/URL.
   - Run a quick SQL or log check using their account ID or organization ID.
3. **Communicate Progress:**
   - If isolated: provide a workaround or gather exact steps/screenshots.
   - If platform-wide: reassure them that Engineering is engaged, give ticket tracking ID, and update them within the promised SLA window (e.g., every 15-30 minutes).

### Scenario B: When do you escalate a ticket from L1 to L2 or Engineering?
**Your Answer:**
> "I escalate when:
> 1. I have gathered complete diagnostic evidence: customer account ID, steps to reproduce, timestamp, relevant log snippets or HTTP response codes, and network HAR files if applicable.
> 2. The issue points to a backend bug, database deadlocks, unhandled server exceptions (500 errors), or requires code changes outside L1 permission boundaries.
> 3. I always package the escalation ticket cleanly so the engineer doesn't have to re-ask the customer for basic info."

---

## Part 5: Key Strengths to Reiterate at the End of the Interview
1. **Bilingual fluency:** Ready to handle live chats, phone calls, or async tickets with enterprise customers worldwide.
2. **Technical versatility:** Not afraid of command lines, SQL, or Python scripts.
3. **High work ethic & grit ("สู้งาน"):** Passionate about doing the heavy lifting, exceeding SLA metrics, and learning rapidly on the job.
