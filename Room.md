# Room

Room ORM is an abstraction layer top of SQLite to allow easy database access, database operations and so on.

<br/>

### Room vs SQLite

|                          SQLite                          |                            Room                             |
|:--------------------------------------------------------:|:-----------------------------------------------------------:|
|                  Deal with Raw Queries                   |           No Raw queries for basic DB operations            |
|        No compile time check for raw SQL queries         |       Provides compile time check for SQL statements        |
| Lots of boilerplate code to convert DB data into objects | Maps DB objects and kotlin objects without boilerplate code |
|      Low level and more complex to write large apps      | Room with ViewModel, LiveData and Coroutines makes it easy  |
