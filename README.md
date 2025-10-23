# 🏦 Bank Data Analytics Project (SQL + Power BI)
<img width="1416" height="798" alt="Screenshot 2025-10-23 133530" src="https://github.com/user-attachments/assets/aafb9f95-5791-4b1c-bb41-01e9cd703c0f" />

## 📘 Overview
This project focuses on analyzing a bank’s customer and financial data using **MySQL** for data management and **Power BI** for visualization.  
It involves creating multiple relational tables, importing CSV data, running SQL analysis queries, and visualizing key financial insights in Power BI.

---

## 🗄️ Database Structure

**Database Name:** `bank_analytics`

### **Tables Created**
1. **customers** — Stores customer information  
   Columns: `customer_id`, `full_name`, `gender`, `dob`, `city`, `state`, `income`, `occupation`, `join_date`

2. **accounts** — Holds account details for each customer  
   Columns: `account_id`, `customer_id`, `branch_name`, `account_type`, `balance`

3. **transactions** — Records transaction details  
   Columns: `transaction_id`, `account_id`, `transaction_date`, `transaction_type`, `amount`

4. **loans** — Contains loan-related data  
   Columns: `loan_id`, `customer_id`, `loan_type`, `loan_amount`, `interest_rate`, `tenure_years`, `default_status`

5. **credit_cards** — Stores credit card information  
   Columns: `card_id`, `customer_id`, `card_type`, `credit_limit`, `current_balance`, `payment_due_date`

Each table is connected using **foreign key relationships**, ensuring referential integrity between customers, accounts, loans, and credit cards.

---

## 🧠 SQL Operations Performed

### **1️⃣ Table Creation & Data Import**
- Created all tables using proper datatypes and foreign key constraints.
- Imported CSV files into MySQL using:
  ```sql
  LOAD DATA LOCAL INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/customers.csv'
  INTO TABLE bank_analytics.customers
  FIELDS TERMINATED BY ',' ENCLOSED BY '"' 
  LINES TERMINATED BY '\n' 
  IGNORE 1 ROWS;

  | # | Query Description                                 | SQL Query                                                                                                                                                                                                     |
| - | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | Count customers by gender                         | `SELECT gender, COUNT(*) AS total FROM customers GROUP BY gender;`                                                                                                                                            |
| 2 | Average account balance by city                   | `SELECT c.city, ROUND(AVG(a.balance), 2) AS avg_balance FROM customers c JOIN accounts a ON c.customer_id = a.customer_id GROUP BY c.city ORDER BY avg_balance DESC;`                                         |
| 3 | Find customers with credit limit above 100000     | `SELECT c.full_name, c.city, cc.card_type, cc.credit_limit FROM customers c JOIN credit_cards cc ON c.customer_id = cc.customer_id WHERE cc.credit_limit > 100000;`                                           |
| 4 | Total transaction amount per account              | `SELECT account_id, SUM(amount) AS total_spent FROM transactions GROUP BY account_id ORDER BY total_spent DESC;`                                                                                              |
| 5 | Loan default summary                              | `SELECT default_status, COUNT(*) AS total_loan FROM loans GROUP BY default_status;`                                                                                                                           |
| 6 | Customers with both credit cards and active loans | `SELECT DISTINCT c.customer_id, c.full_name, c.city FROM customers c JOIN credit_cards cc ON c.customer_id = cc.customer_id JOIN loans l ON c.customer_id = l.customer_id WHERE l.default_status = 'active';` |

