# Stored XSS into HTML Context

**Hint:**

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

## Steps to Reproduce

### 1 - Access the Lab

When it opens, you can see a blog page with a blog post on it.

<img width="1257" height="899" alt="image" src="https://github.com/user-attachments/assets/5cbe2493-e10d-4937-8635-e3d376d2e722" />


### 2 - Find the Comment Section

I entered the first blog post, scrolled down, and found a comment section.

The hint indicated that there was a stored XSS vulnerability, so I decided to try a script.

### 3 - Try the XSS Payload

```html
<script>alert(1)</script>
```

I filled in the name and email fields with test information, then submitted the comment.

<img width="992" height="651" alt="image" src="https://github.com/user-attachments/assets/c15cf6a3-a0ea-4915-9f0d-6a52e0958400" />


### 4 - Check the Result

After that, I went back to the home page, and the alert appeared.

<img width="726" height="372" alt="image" src="https://github.com/user-attachments/assets/6cb40367-a481-453d-ac42-fe017ff8c534" />


And that's how I solved the lab!
