---
tags:
  - labnote/herrerasoft/dealer
---
- [!] I am trying to load the app, when entering `Navigated_Shared.cs` I get a `NullReferenceException` for `Global.oNavigationWindow.RemoveBackEntry()` 
- I will check `Global.oNavigationWindow.RemoveBackEntry()`
	- It is a native function of `NavigationWindow`, I will try commenting it
- [!] The problem is that is trying to load `App_Exception_View`, but shouldn't. It is also still not in the app because it should load `Welcome_View.
- I will try debugging the app step by step to see where it tries to load the exception view.
- Use a try catch to see the exception thrown.
```exception
Unable to resolve service for type 'Dealer.Desktop.ViewModels.Dependency_ViewModel' while attempting to activate 'Dealer.Desktop.ViewModels.Welcome_ViewModel'.
```
-  Add the `service.AddTransient<Dependency_ViewModel>()` to `App.xaml.cs`
```exception
No suitable constructor was found for entity type 'Exception_Log'. The following constructors had parameters that could not be bound to properties of the entity type: 
    Cannot bind 'appInfoService' in 'Exception_Log(IAppInfoService appInfoService, ExceptionLogId id)'
Note that only mapped properties can be bound to constructor parameters. Navigations to related entities, including references to owned types, cannot be bound.
```
-  I will try stopping in the `Exception_Log` constructor
	- Since this is an abstraction it does not work. I have no idea why the constructor is wrong or if the one calling it are wrong
- [?] I will try temporary removing the `appInfoService` dependency. Worked, but shouldn't be an issue.
```exception
The 'DateOnly?' property 'Company.Business_Start_Date' could not be mapped to the database type 'date' because the database provider does not support mapping 'DateOnly?' properties to 'date' columns. Consider mapping to a different database type or converting the property value to a type supported by the database using a value converter. See https://aka.ms/efcore-docs-value-converters for more information. Alternately, exclude the property from the model using the '[NotMapped]' attribute or by using 'EntityTypeBuilder.Ignore' in 'OnModelCreating'.
```
- Since it's not added in the original `CompanyConfiguration` I will try to find a way of ignoring it.
- [[Ignoring property in EF builder]]
- This worked, but it is still trying to load the `App_Exception_View`
- I am adding the `App_Exception_View`, might give me a clue on why it's failing
- [!] Needed to add `App_Exception_ViewModel` and also `Base_ViewModel2` which brings many errors and requirements
- I commented out almost all of `Base_ViewModel2` since it seems to be legacy
- Had to comment `Global.oNavigationWindow.RemoveBackEntry()`
- [!] Since the Uri changed, the view can't load
	- This assumption might be wrong
- [!] `Global.oNavigationWindow` is null, that's the issue