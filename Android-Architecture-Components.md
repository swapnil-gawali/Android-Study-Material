# Android Architecture Components

App architecture design is an important consideration for ensuring that your apps are robust, testable, and
maintainable. Android provides a set of libraries and components to help you put together your app according to best
practices.

Android Architecture Components are a part of Android Jetpack.

Architecture components help in managing our UI component lifecycle and handling data persistence.

All the Android Architecture Components are as follows:

1. **Data Binding**: It helps in declaratively binding UI elements to in our layout to data sources of our app.
2. **Lifecycle Aware Components**: It manages activity and fragment lifecycles of our app, survives configuration changes,
   avoids memory leaks and easily loads data into our UI.
3. **LiveData**: It notifies views of any database changes. Use LiveData to build data objects that notify views when the
   underlying database changes.
4. **Navigation**: It handles everything needed for in-app navigation in Android application.
5. **Paging**: It helps in gradually loading information on demand from our data source.
6. [**Room**](Room.md): It is a SQLite object mapping library. Use it to Avoid boilerplate code and easily convert SQLite
   table data to Java objects. Room provides compile time checks of SQLite statements and can return RxJava, Flowable
   and LiveData observables.
7. **ViewModel**: It manages UI-related data in a lifecycle-conscious way. It stores UI-related data that isn't destroyed on
   app rotations.
8. **WorkManager**: It manages every background jobs in Android with the circumstances we choose.