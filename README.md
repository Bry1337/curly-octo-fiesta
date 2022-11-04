## Java Based Android Database Sample (MVVM) Pattern
The diagrams below is the complete architecture diagram with the component that you are going to implement. Every part will have such a diagram to help you understand where the current component fits into the overall structure of the app, and to see how the components are connected.


## Entity
The data for this app is words, and each word is represented by an entity in the database.
![enter image description here](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/19206d185c60f245_1920.png)



## DAO 
![enter image description here](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/b55a278849b29b86_1920.png)

The data access object, or  [`Dao`](https://developer.android.com/reference/android/arch/persistence/room/Dao.html), is an annotated class where you specify SQL queries and associate them with method calls. The compiler checks the SQL for errors, then generates queries from the annotations. For common queries, the libraries provide convenience annotations such as  `@Insert`.

Note that:

-   The DAO must be an  `interface`  or  `abstract`  class.
-   Room uses the DAO to create a clean API for your code.
-   By default, all queries (`@Query`) must be executed on a thread other than the main thread. (You work on that later.) For operations such as inserting or deleting, if you use the provided convenience annotations, Room takes care of thread management for you.

## Live Data
When you display data or use data in other ways, you usually want to take some action when the data changes. This means you have to observe the data so that when it changes, you can react.

`LiveData`, which is a  [lifecycle library](https://developer.android.com/topic/libraries/architecture/lifecycle.html) class for data observation, can help your app respond to data changes. If you use a return value of type  [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html)  in your method description, Room generates all necessary code to update the  `LiveData`  when the database is updated.

![enter image description here](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/573a310cfc0e997c_1920.png)

## Room Database
Room is a database layer on top of an SQLite database. Room takes care of mundane tasks that you used to handle with a database helper class such as  [`SQLiteOpenHelper`](https://developer.android.com/reference/android/database/sqlite/SQLiteOpenHelper.html).

-   Room uses the DAO to issue queries to its database.
-   By default, to avoid poor UI performance, Room doesn't allow you to issue database queries on the main thread.  [`LiveData`](https://developer.android.com/reference/android/arch/lifecycle/LiveData.html)  applies this rule by automatically running the query asynchronously on a background thread, when needed.
-   Room provides compile-time checks of SQLite statements.
-   Your  `Room`  class must be abstract and extend  `RoomDatabase`.
-   Usually, you only need one instance of the Room database for the whole app.
![enter image description here](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/2cd876ee18559294_1920.png)

## Repository
![enter image description here](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/1ba7cd5b21eb6305_1920.png)

A  _Repository_  is a class that abstracts access to multiple data sources. The Repository is not part of the Architecture Components libraries, but is a suggested best practice for code separation and architecture. A  `Repository`  class handles data operations. It provides a clean API to the rest of the app for app data.

![Dao, Repo, network block diagram](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/2b9726b57b0d07f0.png)

A Repository manages query threads and allows you to use multiple backends. In the most common example, the Repository implements the logic for deciding whether to fetch data from a network or use results cached in the local database.

## ViewModel
![enter image description here](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/64f745b6848396e8_1920.png)

The  `ViewModel`  is a class whose role is to provide data to the UI and survive configuration changes. A  `ViewModel`  acts as a communication center between the Repository and the UI. The  `ViewModel`  is part of the  [lifecycle library](https://developer.android.com/topic/libraries/architecture/lifecycle.html). For an introductory guide to this topic, see  [`ViewModel`](https://developer.android.com/topic/libraries/architecture/viewmodel.html).

![b629bed12492374c.png](https://developer.android.com/static/codelabs/android-training-livedata-viewmodel/img/b629bed12492374c.png)

A  `ViewModel`  holds your app's UI data in a way that survives configuration changes. Separating your app's UI data from your  `Activity`  and  `Fragment`  classes lets you better follow the single responsibility principle: Your activities and fragments are responsible for drawing data to the screen, while your  `ViewModel`  is responsible for holding and processing all the data needed for the UI.

# LICENSE
MIT License
```
Copyright (c) 2022 Edward Bryan Abergas

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
