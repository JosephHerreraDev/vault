---
date: 2025-09-03
tags:
  - note/webdev
---
# Explanation 

(Representational State Transfer) architectural style of building web services.

At its core, REST treats domain objects as resources (nouns) addressed by URIs.
Client manipulate these resources using a uniform interface (HTTP methods, headers, status codes) and exchange representations (JSON).

## HTTP Methods

| Method | Meaning                                   | Typical use                       |
| ------ | ----------------------------------------- | --------------------------------- |
| GET    | Retrieve a representation                 | Fetch a list or a single resource |
| POST   | Create a subordinate or process a command | Create a resource                 |
| PUT    | Replace entire resource                   | Full update                       |
| PATCH  | Apply partial modification                | Partial update                    |
| DELETE | Remove a resource                         | Delete completely                 |
