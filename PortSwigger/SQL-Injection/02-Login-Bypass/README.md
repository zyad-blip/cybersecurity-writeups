# SQL Injection — Login Bypass

**Hint:**

This lab contains a SQL injection vulnerability in the login functionality.

<img width="1200" height="568" alt="image" src="https://github.com/user-attachments/assets/6a5fb756-6103-4677-8522-24dd34fbd354" />


## Steps to Reproduce

### 1 - Access the Lab

When the lab opens, you can see a shop page with a **My account** button in the top-right corner.

<img width="1385" height="787" alt="image" src="https://github.com/user-attachments/assets/7102fe83-4f1b-442c-975a-cc3230876194" />


### 2 - Access the Login Page

When you click on the **My account** button, you will see the login page.

<img width="1306" height="580" alt="image" src="https://github.com/user-attachments/assets/ca7e061b-6f9c-402f-89b6-c2b09ee7fbaa" />


The hint indicated that the lab contains a SQL injection vulnerability in the login functionality, so I decided to try an SQL query.

The lab provides the username `administrator`.

### 3 - Try the SQL Query

```sql
administrator'--
```

<img width="847" height="425" alt="image" src="https://github.com/user-attachments/assets/6fcfa582-7a88-425f-94d3-787a35c124e5" />


Then, I entered any value in the password field and clicked **Log in**.

<img width="1311" height="639" alt="image" src="https://github.com/user-attachments/assets/68485ad4-8b41-45f0-bed5-1b5eac5af650" />


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
