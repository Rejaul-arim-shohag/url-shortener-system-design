# 🔗 URL Shortener

A simple URL Shortener system designed to demonstrate fundamental **System Design** concepts such as:

- Client-Server Architecture
- Load Balancing
- Stateless Application Servers
- Horizontal Scaling
- Database Design
- REST APIs
- HTTP Redirects
- Custom Aliases
- Base62 Short Codes

The project starts from a **whiteboard architecture**, converts it into a **system specification**, and then can be implemented as a working application.

---

## 📌 What is a URL Shortener?

A URL Shortener converts a long URL into a short URL.

For example:

```text
Long URL:

https://example.com/some/very/long/path?user=123&category=system-design



                    │    Client    │
                    │Browser / App │
                    └──────┬───────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │  Load Balancer  │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   App Server    │
                  │   Stateless     │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │    Database     │
                  └─────────────────┘


#Components
Component 1: Client (Browser / Phone)
Component 2: Load Balancer
Component 3: App Server (Backend)
Component 4: Database

#Data Flow (How a Request Travels)
Flow A: Shorten a URL (Write)
Flow B: Visit a Short Link (Redirect / Read)


#APIs (The Doors Into the System)
API 1: Shorten a URL
  POST /api/shorten
  Body:  { "long_url": "https://example.com/very/long/path", "custom_alias": "my-link" }
  Note:  custom_alias is optional. If provided, it becomes the `code` in the database.
  Auth:  none (public)
  {
  "short_url": "https://shortener.com/my-link",
  "code": "my-link",
  "long_url": "https://example.com/very/long/path",
  "created_at": "2026-08-16T10:30:00Z"
}

API 2: Visit a Short Link (the redirect)

API 3: Get Link Metadata (Stretch)
  GET /api/links/:code
Returns the link record without following it. Useful for dashboards.

Response on success (200 OK):
{
  "code": "aB3xd9z",
  "short_url": "https://shortener.com/aB3xd9z",
  "long_url": "https://example.com/very/long/path",
  "created_at": "2026-08-16T10:30:00Z",
  "clicks": 142
}
Response when code does not exist (404 Not Found):
{ "error": "link not found" }

API 4: Click Analytics (Stretch)
  GET /api/links/:code/analytics
Returns click counts and timestamps. Out of scope for v1, listed here so later labs know where it fits.

#Data Model (What the Database Stores)

One table. Three columns. That is the whole data model.

The columns are:

code (VARCHAR(32) PRIMARY KEY) - the short code. Auto-generated is 7 chars; custom aliases can be up to 32 chars.
long_url (TEXT NOT NULL) - the original long URL.
created_at (TIMESTAMP NOT NULL) - when the link was created, set by the database.


Out of Scope (v1)
These are not part of this spec. We will cut them.

User login / accounts
Custom domains for short URLs (go.mycompany.com/x)
Link expiry / auto-delete after N days
QR-code generation
Password-protected short links
Spam / abuse detection / CAPTCHA
A/B testing the redirect target




