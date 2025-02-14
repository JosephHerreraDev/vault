---
date: 2025-02-04
tags:
  - config/entity-framework
---
# Create Migration

```
Add-Migration InitialCreate -p Herrerasoft.Infrastructure -s Herrerasoft.Api -c HerrerasoftDataContext
```

# Update Database

```
Update-database -p Herrerasoft.Infrastructure -s Herrerasoft.Api -context HerrerasoftDataContext
```