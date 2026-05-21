# Day 19 – SQL Practice

## Problem Statement

𝐌𝐮𝐬𝐭 𝐓𝐫𝐲: Walmart (HardLevel) hashtag#SQL Interview Question — Solution

Identify users who started a session and placed an order on the same day. For these users, calculate the total number of orders and the total order value for that day. Your output should include the user, the session date, the total number of orders, and the total order value for that day.

🔍By solving this, you'll learn how to use join, groupby and having. Give it a try and share the output! 👇

𝐒𝐜𝐡𝐞𝐦𝐚 𝐚𝐧𝐝 𝐃𝐚𝐭𝐚𝐬𝐞𝐭:
CREATE TABLE sessions(session_id INT PRIMARY KEY,user_id INT,session_date DATETIME);

INSERT INTO sessions(session_id, user_id, session_date) VALUES (1, 1, '2024-01-01 00:00:00'),(2, 2, '2024-01-02 00:00:00'),(3, 3, '2024-01-05 00:00:00'),(4, 3, '2024-01-05 00:00:00'),(5, 4, '2024-01-03 00:00:00'),(6, 4, '2024-01-03 00:00:00'),(7, 5, '2024-01-04 00:00:00'),(8, 5, '2024-01-04 00:00:00'),(9, 3, '2024-01-05 00:00:00'),(10, 5, '2024-01-04 00:00:00');

CREATE TABLE order_summary (order_id INT PRIMARY KEY,user_id INT,order_value INT,order_date DATETIME);

INSERT INTO order_summary (order_id, user_id, order_value, order_date) VALUES (1, 1, 152, '2024-01-01 00:00:00'),(2, 2, 485, '2024-01-02 00:00:00'),(3, 3, 398, '2024-01-05 00:00:00'),(4, 3, 320, '2024-01-05 00:00:00'),(5, 4, 156, '2024-01-03 00:00:00'),(6, 4, 121, '2024-01-03 00:00:00'),(7, 5, 238, '2024-01-04 00:00:00'),(8, 5, 70, '2024-01-04 00:00:00'),(9, 3, 152, '2024-01-05 00:00:00'),(10, 5, 171, '2024-01-04 00:00:00');

## My Thought Process
To find users who had a session and placed orders on the same day, I joined the sessions and order_summary tables on both user_id and the matching dates. This ensures I only keep records where the session day and order day line up. After the join, I grouped the results by user_id and session_date so each user‑day combination appears once. From there, I calculated the total number of orders and the total order value for that day.

## SQL Solution
```sql

SELECT s.user_id, s.session_date, count(*) AS num_of_orders, SUM(order_value) AS total_order_value
  FROM sessions AS s
  JOIN order_summary AS o
    ON s.user_id = o.user_id AND s.session_date = o.order_date
 GROUP BY s.user_id, s.session_date;
```

## Output

| user_id | session_date           | num_of_orders | total_order_value |
|---------|-------------------------|----------------|--------------------|
| 1       | 2024-01-01 00:00:00     | 1              | 152                |
| 2       | 2024-01-02 00:00:00     | 1              | 485                |
| 3       | 2024-01-05 00:00:00     | 9              | 2610               |
| 4       | 2024-01-03 00:00:00     | 4              | 554                |
| 5       | 2024-01-04 00:00:00     | 9              | 1437               |


## 🎯 Pattern: JOIN + GROUP BY

Why this pattern matters

This is a classic Hard‑level SQL pattern because it requires:

- Joining two event tables
- Matching on user_id AND date
- Grouping by user‑day
- Aggregating both count and sum
- Ensuring only user‑days with orders appear

This pattern appears in:

- sessionization problems
- funnel analysis
- daily active user metrics
- order‑session matching
- marketing attribution

It’s one of the most important SQL patterns for analytics interviews.

 
