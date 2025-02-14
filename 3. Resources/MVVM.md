---
date: 2025-02-04
tags: 
source: "[[Bookmarks#[WPF MVVM](https //www.youtube.com/playlist?list=PLA8ZIAm2I03hS41Fy4vFpRw8AdYNBXmNm)|WPF MVVM]]"
---
Complex maintenance issues can arise as apps are modified and grow in size and scope. These issues include the tight coupling between the UI controls and the business logic, which increases the cost of making UI modifications, and the difficulty of unit testing such code. 

The MVVM pattern helps cleanly separate an application's business and presentation logic from its UI.

At a high level, the view "knows about" the view model, and the view model "knows about" the model, but the model is unaware of the view model, and the view model is unaware of the view.

* The view model acts as an adapter for the model classes and prevents you from making major changes to the model code.
* Developers can create unit tests for the view model and the model, without the view.
* The app UI can be redesigned without touching the view model and model code.

> [!note]
> The key to using MVVM effectively lies in understanding how to factor app code into the correct classes and how the classes interact

## View

The view is responsible for defining the structure, layout, and appearance of what the user sees on screen. The code-behind might contain UI logic that implements visual behavior that is difficult to express in XAML, such as animations
If a control supports commands, the control's Command property can be data-bound to an ICommand property on the view model. When the control's command is invoked, the code in the view model will be executed. 

## ViewModel

The view model implements properties and commands to which the view can data bind to, and notifies the view of any state changes through change notification events. In the view model, use asynchronously notify views of property changes.
The view model is also responsible for coordinating the view's interactions with any model classes that are required. The view model might choose to expose model classes directly to the view so that controls in the view can data bind directly to them.
Each view model provides data from a model in a from a model in a form that the view can easily consume, the view model sometimes performs data conversion. 
Use converters as a separate as a separate data conversion layer that sits between the view model and the view, properties must raise the PropertyChanged event. View models satisfy this requirement by implementing the INotifyPropertyChanged interfaces and raising the PropertyChanged event when a property is changed. 
For collections, the view-friendly `ObservableCollection<T>` is provided.

## Model

Model classes are non-visual classes that encapsulate the app's data. Model classes are typically used in conjunction with services or repositories that encapsulates data access and caching.