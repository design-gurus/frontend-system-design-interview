# Security

Frontend security in an interview centers on three topics: cross-site scripting, cross-site request forgery, and content security policy. A fourth topic, where to keep the sign-in token, connects them. A strong answer names the attack, the main defense, and the second defense that limits the damage if the first one fails.

## Cross-site scripting (XSS)

Cross-site scripting is an attack in which a page runs JavaScript supplied by an attacker. The script runs with the victim's session, so it can read the page, send requests, and steal data.

| Type | How the script arrives |
|---|---|
| Stored | It is saved on the server, for example in a comment, and served to every reader |
| Reflected | It is placed in a URL, and the server echoes it into the page |
| DOM-based | Client code writes untrusted input into the page, for example through `innerHTML` |

Defenses, in order:

1. Escape on output. Modern frameworks escape text by default, so `<script>` in a comment is shown as text.
2. Avoid raw HTML sinks such as `innerHTML` and `dangerouslySetInnerHTML`.
3. Where rich text is required, sanitize it with an allowlist of tags and attributes.
4. Add a content security policy as the second layer.

## Content security policy (CSP)

A content security policy is an HTTP response header that tells the browser which sources of script, style, and other content the page may load.

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-r4nd0m'; object-src 'none'; base-uri 'none'
```

A nonce is a random value generated for each response. Only script tags that carry the same nonce are allowed to run. An injected script has no nonce, so the browser blocks it. A policy can first be deployed in report-only mode, which records violations without blocking anything.

## Cross-site request forgery (CSRF)

Cross-site request forgery is an attack in which another site causes the victim's browser to send a request to your site. The browser attaches the victim's cookies automatically, so the server sees a valid session.

| Defense | How it works |
|---|---|
| `SameSite` cookies | `Lax` or `Strict` stops the browser from sending the cookie on cross-site requests |
| CSRF token | The server issues a secret value that the page must send back in a header or form field |
| Origin check | The server rejects state-changing requests whose `Origin` header is not its own |

Read requests must never change state, because `SameSite=Lax` still sends cookies on top-level navigations.

## Where to keep the token

| Location | Readable by injected script | Sent automatically | Main risk |
|---|---|---|---|
| `localStorage` | Yes | No | XSS steals the token |
| Cookie with `HttpOnly`, `Secure`, `SameSite` | No | Yes | CSRF, reduced by `SameSite` and a token |
| Memory only | Only while the page runs | No | Lost on reload; needs a refresh flow |

The common recommendation is a session or refresh token in an `HttpOnly` cookie, with a short-lived access token held in memory. `HttpOnly` means that JavaScript cannot read the cookie.

## Other points worth naming

- Third-party scripts run with full access to the page. Load few of them, and use subresource integrity, a hash that the browser checks before running a file.
- `frame-ancestors` in the policy prevents clickjacking, where another site shows your page in a hidden frame.
- The client is never the authority. Every permission check must be repeated on the server.

## A worked example

A product review field accepts text. A user submits `<img src=x onerror="fetch('/api/account')">`. With output escaping, the review is displayed as literal text and nothing runs. If one template uses raw HTML by mistake, the policy still blocks the inline handler. The two layers fail independently, which is the point of having both.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-security) covers XSS, content security policy, and token storage with worked attacks.
- Lesson: [Cross Site Scripting and Content Security Policy](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/cross-site-scripting-and-content-security-policy?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-security), in Grokking the Modern Frontend System Design Interview
- Lesson: [Authentication Tokens in the Browser](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/authentication-tokens-in-the-browser?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-security), in Grokking the Modern Frontend System Design Interview
- [Grokking JavaScript Fundamentals](https://www.designgurus.io/course/grokking-javascript-fundamentals?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-security) covers the language and DOM basics these attacks rely on.
- [Offline and storage](offline-and-storage.md) lists the storage options in full.
- Free article: [Cookies vs Sessions: How Web Apps Keep Track of Users](https://www.designgurus.io/blog/cookies-vs-sessions?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-security), background for the token storage decision.
