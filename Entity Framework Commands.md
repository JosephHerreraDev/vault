---
date: 2025-02-04
tags:
  - note/csharp/entityframework
---
# Create Migration

```bash
Add-Migration InitialCreate -p Herrerasoft.Infrastructure -s Herrerasoft.Api -c ApplicationDbContext
```

# Update Database

```
Update-database -p Herrerasoft.Infrastructure -s Herrerasoft.Api -context ApplicationDbContext
```