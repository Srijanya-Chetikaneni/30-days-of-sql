# Day 17 – SQL Practice

## Problem Statement
𝐌𝐮𝐬𝐭 𝐓𝐫𝐲: Uber (Medium Level) hashtag#SQL Interview Question — Solution

You’re given a table of Uber rides that contains the mileage and the purpose for the business expense. You’re asked to find business purposes that generate the most miles driven for passengers that use Uber for their business transportation. Find the top 3 business purpose categories by total mileage.

🔍 By solving this, you'll learn how to group by. Give it a try and share the output! 👇

𝐒𝐜𝐡𝐞𝐦𝐚 𝐚𝐧𝐝 𝐃𝐚𝐭𝐚𝐬𝐞𝐭:
CREATE TABLE my_uber_drives (start_date DATETIME,end_date DATETIME,category VARCHAR(50),start VARCHAR(50),stop VARCHAR(50),miles FLOAT,purpose VARCHAR(50));

INSERT INTO my_uber_drives (start_date, end_date, category, start, stop, miles, purpose) VALUES('2016-01-01 21:11', '2016-01-01 21:17', 'Business', 'Fort Pierce', 'Fort Pierce', 5.1, 'Meal/Entertain'),('2016-01-02 01:25', '2016-01-02 01:37', 'Business', 'Fort Pierce', 'Fort Pierce', 5, NULL),('2016-01-02 20:25', '2016-01-02 20:38', 'Business', 'Fort Pierce', 'Fort Pierce', 4.8, 'Errand/Supplies'),('2016-01-05 17:31', '2016-01-05 17:45', 'Business', 'Fort Pierce', 'Fort Pierce', 4.7, 'Meeting'),('2016-01-06 14:42', '2016-01-06 15:49', 'Business', 'Fort Pierce', 'West Palm Beach', 63.7, 'Customer Visit'),('2016-01-06 17:15', '2016-01-06 17:19', 'Business', 'West Palm Beach', 'West Palm Beach', 4.3, 'Meal/Entertain'),('2016-01-06 17:30', '2016-01-06 17:35', 'Business', 'West Palm Beach', 'Palm Beach', 7.1, 'Meeting');

## My Thought Process

To solve this problem, I first filtered the data to include only rides where the category is Business.
Then, I grouped the records by the purpose of the trip and calculated the total miles for each purpose using the SUM() aggregate function.
Finally, I sorted the results in descending order of total mileage and selected the top three purposes.

## SQL Solution
```sql
SELECT purpose, sum(miles) as total_miles
  FROM my_uber_drives
 WHERE category = 'Business'
 GROUP BY purpose
 ORDER BY total_miles DESC
 LIMIT 3;
```

## Output

| purpose          | total_miles |
|------------------|-------------|
| Customer Visit   | 63.7        |
| Meeting          | 11.8        |
| Meal/Entertain   | 9.4         |

## 🎯 Pattern: GROUP BY + SUM + ORDER BY + LIMIT

Why this pattern matters

This is one of the most common SQL interview patterns:

- Filter the dataset
- Group by a category
- Aggregate a numeric column
- Sort by the aggregated value
- Return the top N results

This pattern appears in:

- top‑selling products
- top revenue categories
- top customers by spend
- top drivers by distance
- top cities by orders

It’s simple but extremely powerful.

