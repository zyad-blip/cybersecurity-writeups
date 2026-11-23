# SQL Injection — Retrieving Hidden Data

**Lab:** SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

**Hint:**

This lab contains a SQL injection vulnerability in the product category filter.

<img width="1153" height="408" alt="image" src="https://github.com/user-attachments/assets/a31ca0e3-e881-408d-b1b2-c3aa4c5de420" />


## Steps to Reproduce

### 1 - Access the Lab

When it opens, you can see a shop page with a category filter on it.

<img width="1356" height="844" alt="image" src="https://github.com/user-attachments/assets/5fe77788-ecca-4446-b0c3-35804e7751e3" />


### 2 - Intercept the Traffic with Burp Suite

I opened Burp Suite and turned on Intercept. Then I clicked on **Corporate Gifts** and found the `category` parameter in the request.

The hint indicated that the lab contains a SQL injection vulnerability in the product category filter, so I decided to try an SQL query.

### 3 - Try the SQL Query

```sql
'+OR+1=1--
```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/345a6001-2f08-4f8b-ab49-1b64c3bdffa5" />


Then I forwarded the request, went back to the browser, and checked the results.

<img width="1437" height="861" alt="image" src="https://github.com/user-attachments/assets/d9b8030c-ba43-4a7d-85f6-cbc5a9297455" />


## Technical Explanation

The payload works by modifying the original SQL query.

- `'` : Closes the original string value.
- `OR` : Adds an alternative condition to the query.
- `1=1` : A Boolean condition that is always TRUE.
- `--` : Starts an SQL comment, which comments out the remaining part of the query.
- `+` : Represents a space in URL encoding.

### Why It Works

The original SQL query contains two conditions:

```sql
SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1;
```

After the injection, the query becomes conceptually:

```sql
SELECT * FROM products
WHERE category = 'Gifts' OR 1=1
-- AND released = 1;
```

Since `1=1` is always TRUE, the WHERE condition matches all products.

The `--` comment also prevents the remaining release restriction from being executed.

As a result, the application displays all products, including unreleased ones.

## Root Cause

The application directly incorporates user-controlled input into an SQL query without using parameterized queries.

## Impact

An attacker could retrieve products that are normally hidden by the application's category and release restrictions.

## Key Takeaways

- SQL Injection can occur in the WHERE clause.
- Boolean conditions can change the logic of SQL queries.
- SQL comments can neutralize the remaining part of a query.
- Always inspect HTTP requests to identify user-controlled parameters.
