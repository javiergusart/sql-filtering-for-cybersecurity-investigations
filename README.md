# SQL Filtering for Cybersecurity Investigations

## Table of Contents  
1. [Overview](#overview)  
2. [Scenario](#scenario)  
3. [Query 1: After-Hours Failed Login Attempts](#query-1-after-hours-failed-login-attempts)  
4. [Query 2: Login Attempts on Specific Dates](#query-2-login-attempts-on-specific-dates)  
5. [Query 3: Login Attempts Outside of Mexico](#query-3-login-attempts-outside-of-mexico)  
6. [Query 4: Employees in Marketing (East Offices)](#query-4-employees-in-marketing-east-offices)  
7. [Query 5: Employees in Sales or Finance](#query-5-employees-in-sales-or-finance)  
8. [Query 6: Employees Not in IT](#query-6-employees-not-in-it)  
9. [Summary](#summary)

---

## Overview

This portfolio project was completed as part of the Google Cybersecurity Professional Certificate and is designed to demonstrate practical SQL skills for cybersecurity use cases. In this scenario-based activity, I took on the role of a security professional within a large organization, tasked with investigating potential security incidents using SQL queries.

The project focuses on analyzing login attempt data and employee machine information from two internal tables: `log_in_attempts` and `employees`. The goal was to identify suspicious patterns—such as failed logins after business hours, logins on specific dates, or access from outside authorized regions—as well as to isolate department-based employee records for targeted machine updates.

Throughout the investigation, I applied SQL filtering techniques using `AND`, `OR`, `NOT`, and `LIKE` to extract only the most relevant information. These queries simulate real-world tasks security analysts perform when triaging logs, managing access, and maintaining secure systems.

---

## Scenario

As a cybersecurity analyst at a large organization, I was tasked with investigating login and access anomalies in the company’s internal systems. Two main tables were analyzed:

- `log_in_attempts` — stores records of login activity, including success/failure status, login time/date, and country of origin.
- `employees` — holds employee details such as department and office location.

My objective was to write SQL queries to detect after-hours activity, isolate logins from specific dates or locations, and filter employee data for machine updates based on departmental roles and physical office location.

---

## Query 1: After-Hours Failed Login Attempts

### Context
A potential security incident was reported that occurred outside regular working hours. To investigate this, I needed to identify all failed login attempts that took place after 6:00 PM. Failed attempts can indicate brute-force attacks or unauthorized access attempts.

```sql
SELECT *
FROM log_in_attempts
WHERE success = 0
  AND login_time > '18:00';
```

**Explanation:**  
This query filters for rows where `success = 0` (indicating a failed login) and where the login time occurred after 18:00. This helps identify suspicious login activity happening after hours when the office is closed.

---

## Query 2: Login Attempts on Specific Dates

### Context  
A suspicious event was reported on May 9, 2022, and I needed to investigate login activity not only on that day but also on the previous day for a complete picture of any potential lead-up activity.

```sql
SELECT *
FROM log_in_attempts
WHERE login_date = '2022-05-08'
   OR login_date = '2022-05-09';
```

**Explanation:**  
This query pulls all login attempts that occurred on May 8 or May 9. By using `OR`, I was able to extract records from both days to support a time-based analysis around the incident.

---

## Query 3: Login Attempts Outside of Mexico

### Context  
During the investigation, the team determined that suspicious login attempts were not originating from Mexico. To narrow the dataset and focus on external threats, I needed to exclude any login attempts where the country matched `MEX` or `MEXICO`.

```sql
SELECT *
FROM log_in_attempts
WHERE country NOT LIKE '%MEX%';
```

**Explanation:**  
This query uses the `NOT LIKE '%MEX%'` condition to exclude any country values containing `MEX`, ensuring both `MEX` and `MEXICO` are filtered out. It helps isolate potentially foreign access attempts for further review.

---

## Query 4: Employees in Marketing (East Offices)

### Context  
The IT team planned a security update rollout targeting machines in the Marketing department located in East building offices. I needed to provide a filtered list of employees that matched both criteria.

```sql
SELECT *
FROM employees
WHERE department = 'Marketing'
  AND office LIKE 'East-%';
```

**Explanation:**  
This query combines both conditions using `AND`: the department must be 'Marketing', and the office must start with 'East-'. The `LIKE` operator ensures flexibility in matching office numbers like `East-170` or `East-320`.

---

## Query 5: Employees in Sales or Finance

### Context  
Another security update was scheduled for the Sales and Finance departments. To assist with this deployment, I needed to identify all employees in either department.

```sql
SELECT *
FROM employees
WHERE department = 'Sales'
   OR department = 'Finance';
```

**Explanation:**  
This query uses `OR` to retrieve employees in either department. It’s useful when multiple business units require the same policy enforcement or IT action.

---

## Query 6: Employees Not in IT

### Context  
The Information Technology (IT) department had already received the necessary updates. My task was to identify all other employees who still required the rollout.

```sql
SELECT *
FROM employees
WHERE department != 'Information Technology';
```

**Explanation:**  
This query filters out rows where the department is 'Information Technology' using `!=`. The result includes everyone else still pending the update.

---

## Summary

This project simulates real-world security investigation tasks that require filtering large datasets with SQL. I used logical operators like `AND`, `OR`, `NOT`, and pattern matching with `LIKE` to extract records that helped support threat detection, access analysis, and system maintenance workflows.

By querying login attempts and employee records, I demonstrated how SQL can power internal audits, detect anomalies, and help coordinate technical responses across departments. These are foundational skills for any cybersecurity analyst working in data-driven environments.
