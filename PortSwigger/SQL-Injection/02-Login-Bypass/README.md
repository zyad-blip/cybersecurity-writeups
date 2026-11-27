# SQL Injection — Login Bypass

**Hint:**

This lab contains a SQL injection vulnerability in the login functionality.

## Steps to Reproduce

### 1 - Access the Lab

When the lab opens, you can see a shop page with a **My account** button in the top-right corner.

### 2 - Access the Login Page

When you click on the **My account** button, you will see the login page.

The hint indicated that the lab contains a SQL injection vulnerability in the login functionality, so I decided to try an SQL query.

The lab provides the username `administrator`.

### 3 - Try the SQL Query

```sql
administrator'--
```

Then, I entered any value in the password field and clicked **Log in**.

## Technical Explanation

Let's understand the payload:

- `administrator`: The username provided by the lab.
- `'`: Closes the original SQL string.
- `--`: Starts an SQL comment, which comments out the remaining part of the query.

### Why It Works

The SQL comment causes the remaining part of the query to be ignored.

In this lab, this allows the login query to bypass the password condition, resulting in a successful login without knowing the administrator's password.

## Key Takeaways

- SQL Injection can occur in login functionality.
- SQL comments can ignore the remaining part of a query.
- Improper handling of user input can lead to authentication bypass.
