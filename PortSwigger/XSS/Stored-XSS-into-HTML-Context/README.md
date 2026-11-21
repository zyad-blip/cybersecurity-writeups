# Stored XSS into HTML Context

**Hint:**

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

## Steps to Reproduce

### 1 - Access the Lab

When it opens, you can see a blog page with a blog post on it.

### 2 - Find the Comment Section

I entered the first blog post, scrolled down, and found a comment section.

The hint indicated that there was a stored XSS vulnerability, so I decided to try a script.

### 3 - Try the XSS Payload

```html
<script>alert(1)</script>
```

I filled in the name and email fields with test information, then submitted the comment.

### 4 - Check the Result

After that, I went back to the home page, and the alert appeared.

And that's how I solved the lab!
