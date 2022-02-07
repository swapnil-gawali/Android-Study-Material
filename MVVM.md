# MVVM Architecture Pattern

MVVM stands for Model-View-ViewModel. MVVM is way to structure our code.

* **Model:** Model classes can be Data classes, POJO, local or remote data sources or repositories.
* **View:** Views are Activity/Fragment, xml etc.Views are responsible to update UI related data which comes from the
  ViewModel.
* **ViewModel:** ViewModels are bridge between Views and repository. ViewModels job is to get data from the repository
  and pass to the Activity/Fragment using livedata. See [ViewModel](ViewModel.md).

<br/>

### Advantages of MVVM

* UI Components like Activity/Fragment are only responsible to update UI data.
* Business logic kept away from the database and API operation.
* Easy to manage and scale.
* Easy to manage lifecycle of the application i.e. lifecycle state of the application is maintained.
* It follows separation of concern.

<br/>

### MVVM Architecture Diagram

![MVVM Architecture](images/mvvm_architecture.png)

<br/>

### Repository Pattern

Repository pattern follows single truth of service i.e. all the operations related to database and API service handled
by the repository and return those data to the ViewModel.

> repository's job is to fetch data from database or API service, perform operations to it and return it to ViewModel.

<br/>

### MVC (Model-View-Controller) in Android

Whenever we create new android application, it basically follows MVC architecture.

* **Model:** Data classes or POJO etc or business logic.
* **View:** In android, View represents xml layouts.
* **Controller:** Controller represents Activity/Fragments.

When in comes to Android MVC works following ways:

Views can be Buttons, TextViews, EditText etc. User interacts with these views and the events corresponding to these
Views for e.g. click, text changed etc. gets registered in the Controller(Activity/Fragment). Controllers job is to
fetch data from Model class and update Views(xml). Models can have business logic or holds data classes or POJO.

<br/>

### MVP (Model-View-Presenter) in Android

* **Model:** It represents data classes, POJO, repository etc.
* **View:** It represents xml and Activity/Fragments.
* **Presenter:** It is basically like ViewModel where we write business logic and return that data to View using
  interfaces. We cannot write android related code inside presenter.