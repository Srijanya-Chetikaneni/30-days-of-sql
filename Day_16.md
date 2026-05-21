# Day 16 – SQL Practice

## Problem Statement
𝐌𝐮𝐬𝐭 𝐓𝐫𝐲: JP Morgan, Chase, Bloomberg (Medium Level) hashtag#SQL Interview Question — Solution

Bank of Ireland has requested that you detect invalid transactions in December 2022. An invalid transaction is one that occurs outside of the bank's normal business hours. The following are the hours of operation for all branches:

Monday - Friday 09:00 - 16:00
Saturday & Sunday Closed
Irish Public Holidays 25th and 26th December
Determine the transaction ids of all invalid transactions.

## My Thought Process
To solve this problem, I broke it down into three independent checks:

1️⃣ Filter only December 2022 transactions
```sql
strftime('%Y-%m', time_stamp) = '2022-12'
```
2️⃣ Identify invalid days
A transaction is invalid if it occurs on:

Weekends:

SQLite weekday mapping:
  - Sunday → 0
  - Saturday → 6

Public holidays:
  - December 25
  - December 26

3️⃣ Identify invalid times on weekdays
Even on weekdays, transactions are invalid if they occur:

Before 09:00
After 16:00


## SQL Solution
```sql
SELECT transaction_id, time_stamp
FROM boi_transactions
WHERE strftime('%Y-%m', time_stamp) = '2022-12'
  AND
  (
      -- Weekend transactions
      strftime('%w', time_stamp) IN ('0','6')

      OR

      -- Public holidays
      strftime('%d', time_stamp) IN ('25','26')

      OR

      -- Weekdays but outside business hours
      (
          strftime('%w', time_stamp) NOT IN ('0','6')
          AND (
              strftime('%H:%M:%S', time_stamp) < '09:00:00'
              OR strftime('%H:%M:%S', time_stamp) > '16:00:00'
          )
      )
  );


```

## Output

| transaction_id | time_stamp           |
|----------------|----------------------|
| 1051           | 2022-12-03 10:15     |
| 1052           | 2022-12-03 17:00     |
| 1053           | 2022-12-04 10:00     |
| 1054           | 2022-12-04 14:00     |
| 1055           | 2022-12-05 08:59     |
| 1056           | 2022-12-05 16:01     |
| 1062           | 2022-12-10 11:00     |
| 1063           | 2022-12-10 17:30     |
| 1064           | 2022-12-11 12:00     |
| 1065           | 2022-12-12 08:59     |
| 1066           | 2022-12-12 16:01     |
| 1067           | 2022-12-25 10:00     |
| 1068           | 2022-12-25 15:00     |
| 1069           | 2022-12-26 09:00     |
| 1070           | 2022-12-26 14:00     |
| 1071           | 2022-12-26 16:30     |
| 1073           | 2022-12-28 08:30     |
| 1074           | 2022-12-29 16:15     |
| 1076           | 2022-12-31 10:00     |

## 🎯 Pattern: Date/Time Filtering + Conditional Logic

Why this pattern matters

This problem is a classic Medium‑level SQL filter logic challenge.
It tests your ability to:

- Extract weekday numbers

- Compare time ranges

- Identify holidays

- Combine multiple conditions with OR + AND

- Use strftime() for date/time parsing (SQLite)

This is the exact type of logic used in:

- fraud detection

- transaction monitoring

- compliance checks

- log analysis
  
## ✨ Key Takeaways
- strftime() always returns TEXT — format carefully.

- ISO date/time strings (YYYY-MM-DD HH:MM:SS) are safe for comparisons.

- Always separate day logic from time logic.

- Small logical operators (IN vs NOT IN, AND vs OR) can completely change results.
