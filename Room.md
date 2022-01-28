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

<br/>

### Entity

Entity is a class which is defined with the `@Entity` annotation. This class holds information about the database
schema(table).

```kotlin
@Entity(tableName = "user")
class UserEntity(
    @PrimaryKey(autoGenerate = true) // this will mark "id" field as primary key and auto increment it.
    val id: Int,

    @ColumnInfo(name = "first_name") // "name" is custom column name
    val firstName: String,

    @ColumnInfo(name = "last_name")
    val lastName: String,

    @ColumnInfo(
        name = "country",
        defaultValue = "India",
        index = true
    ) // this will provide default value to "country field make it index
    val country: Boolean,

    @Ignore // this field will be ignore and do not get included in the table
    val fullName: String = firstName + lastName
)
```

`@Entity` is used to annotate class, we can also provide custom table name using `tableName` property.

`@PrimaryKey` is used to define field as primary key.

`@ColumnInfo` is used to define different column name, index and to provide default value.

`@Ingore` is used to ignore field in the entity, so Room will not include it in the database schema.

`@ForeignKey` is used to define relation with the other table.

<br/>

### Data Access Objects (DAO)

This is an interface that contains methods to access DB, and it is annotated with `@Dao` annotation.

`@Dao` is used to annotate data access object interface.

`@Insert` is used to implement insert operation.

`@Update` is used to implement update operation.

`@Delete` is used to implement delete operation.

`@Query` is used for any custom database operation like insert, delete update etc. We need to provide custom query to
perform this operation. The Query provide is validated at compile time.

<br/>

### Database

This is an abstract database holder class and this is annotated with the `@Database` annotation. This class must
extends `RoomDatabase` class.

This class holds abstract method to get the DAO and creates instance of the database object.

```kotlin
@Database(version = 1, entities = [UserEntity::class])
abstract class MyDatabase : RoomDatabase() {

    abstract fun userDao(): UserDao

    companion object {
        // Singleton prevents multiple instances of database opening at the
        // same time.
        @Volatile
        private var INSTANCE: MyDatabase? = null

        fun getDatabase(context: Context): MyDatabase {
            // if the INSTANCE is not null, then return it,
            // if it is, then create the database
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    MyDatabase::class.java,
                    "my_database"
                ).build()
                INSTANCE = instance
                // return instance
                instance
            }
        }
    }
}
```

In the above example, we have created `abstract` function to retrieve `UserDao`. `companion object` is used to create
object of the database if not present, if object is already created then it will return same object. This is singleton
implementation.

`@Database` this is used to declare this class as a database holder class. This annotation expects multiple parameters
in which `version` and `entities` are mandatory. `version` is used in case we change database schema and perform
migrations. In the `entities` field, we can provide multiple entities associated with the database.

<br/>

### Inserting Data

For insert operation, we need to get DAO using database object and call the method associate with it.

```kotlin
@Dao
interface UserDao {
    @Insert
    fun insertUser(userEntity: UserEntity): Int // returns id of inserted user
}
```

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // first get database object
        val myDatabase = MyDatabase.getDatabase(this)

        // retrieving DAO using database object
        val userDao = myDatabase.userDao()

        val buttonInsertData: Button = findViewById(R.id.btn_insert_data)
        val tvMessage: TextView = findViewById(R.id.tv_message)

        buttonInsertData.setOnClickListener {
            // creating entity which needs to persist
            val userEntity = UserEntity(firstName = "Swapnil", lastName = "Gawali", country = "India")

            // inserting data into the database
            val insertedUserId = userDao.insertUser(userEntity)

            // set message that user is inserted with id
            tvMessage.text = "User inserted with id $insertedUserId"
        }
    }
}
```

<br/>

### Reading Data:

For reading data from the database, we need to use `@Query` annotation and pass the raw SQL query as per our
requirement.

`UserDao.kt`

```kotlin
@Query("SELECT * FROM user")
fun getAllUsers(): List<UserEntity> // fetching all users

@Query("SELECT * FROM user WHERE first_name = :firstName")
fun getUser(firstName: String): UserEntity // fetching single user by its name
```

`MainActivity.kt`

```kotlin
val userList = userDao.getAllUsers()
print(userList)

val user = userDao.getUser("Swapnil")
print(user)
```

<br/>

### Updating Data

The implementation of the method will update its parameters in the database if they already exists (checked by primary
keys). If they don't already exist, this option will not change the database.

`UserDao.kt`

```kotlin
@Update
fun updateUser(userEntity: UserEntity): Int // returns id of updated user
```

`MainActivity.kt`

```kotlin
val userEntity = UserEntity(firstName = "Rohit", lastName = "Sharma", country = "Australia")
val updatedUserId = userDao.updateUser(userEntity)
```

<br/>

### Deleting Data

The implementation of the method will delete record from the database.

`UserDao.kt`

```kotlin
@Delete
fun deleteUser(userEntity: UserEntity): Int // returns id of deleted user
```

`MainActivity.kt`

```kotlin
val userEntity = UserEntity(firstName = "Rohit", lastName = "Sharma", country = "Australia")
val updatedUserId = userDao.deleteUser(userEntity)
```

<br/>

### Migration

If we change any database schema, it has to perform migration. Migration preserves user data in case of change of entity
and database schema.

