---
tags:
  - labnote/herrerasoft/dealer
---
- [!] `Global.oNavigationWindow` is null
- I am trying to figure out why it is failing, the only difference is that my version is missing a second singleton initialization
- [!] `.ico` was missing
- Added all missing images, I suspect the error with the navigation was caused by the error preventing all the interfaces to load properly
- I will try to load an exception to see why the `appinfoservice` is failing
```exception
No suitable constructor was found for entity type 'Exception_Log'. The following constructors had parameters that could not be bound to properties of the entity type: 
    Cannot bind 'appInfoService' in 'Exception_Log(IAppInfoService appInfoService, ExceptionLogId id)'
Note that only mapped properties can be bound to constructor parameters. Navigations to related entities, including references to owned types, cannot be bound.
```
- I added a blank constructor for EF, apparently that is the issue
- Worked, now I am going to add all the Domain Entities
- I left off in `Inventory`