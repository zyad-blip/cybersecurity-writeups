# CyberHeroes

**Platform:** TryHackMe  
**Category:** Web / Authentication  
**Difficulty:** Easy  
**Status:** Completed

## Objective

Analyze the login page and find a way to authenticate to the CyberHeroes training machine.

## Approach

### 1. Inspect the page's JavaScript

Instead of guessing credentials, I inspected the page source and located the `authenticate()` function.

The function reads the username and password from the page:

```javascript
a = document.getElementById('uname')
b = document.getElementById('pass')
```

It then checks both values in a client-side condition:

```javascript
if (a.value=="h3ck3rBoi" & b.value==RevereString("54321@terceSrepuS"))
```

### 2. Understand the string reversal

The page defines a function that reverses a string:

```javascript
const RevereString = str => [...str].reverse().join('');
```

This works in three steps:

1. `[...str]` spreads the string into an array of characters.
2. `.reverse()` reverses the array.
3. `.join('')` joins the characters back into a string.

Reversing the hard-coded value reveals the password used by the condition.

The credentials identified from the JavaScript were:

- **Username:** `h3ck3rBoi`
- **Password:** `SuperSecret@12345`

### 3. Understand what happens after authentication

If the condition is true, the page creates an XMLHttpRequest and sends a GET request. The requested filename is assembled from a fixed path prefix, the username, and the password.

When the server responds with HTTP status 200, the page places the response text in the element with ID `flag`, clears another page element, and removes the login-related element.

This explains why inspecting the JavaScript was enough to understand the intended login path; brute force was unnecessary.

## Result

The room accepted the submitted flag:

```text
flag{edb0be532c540b1a150c3a7e85d2466e}
```

**Room completed: 100%.**

## Security Lessons

- **Client-side authentication is not suitable for protecting secrets.** JavaScript delivered to a browser can be inspected by the user.
- Reversing or obfuscating a hard-coded password does not make it secret. The transformation and its input are both visible to the client.
- Authentication and authorization decisions for protected resources must be enforced on the server.
- A client-side check can be bypassed or modified, so the server must independently validate every protected request.
- The JavaScript uses `&` (bitwise AND) where `&&` (logical AND) would be the conventional operator for combining boolean conditions. This is a code-quality issue; it is not the main security weakness in this challenge.

## Key Takeaway

The important step was reading the client-side JavaScript before attempting guesses. Source-code inspection exposed the hard-coded username and the reversible password check, demonstrating why security-sensitive logic must not rely on code running in the user's browser.
