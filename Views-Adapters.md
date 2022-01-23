### Adapters

Adapters in Android are basically a bridge between the UI components(list) and the data source that fill data into the
UI Component.

Adapters basically get element from the array, and it binds to the item view and show in the RecyclerView, ListView etc.

<br/>

### ViewHolder Design Pattern:

This pattern is used to recycle the view object again and again and bind those data to the view object when we scroll.

When we scroll down RecyclerView, it destroys top item which is out screen, take the old view object, reuse it and bind
data to it at the bottom. Same process is perform when we user scrolls up.

<br/>

### ListView & GridView

These both by default does not use ViewHolder pattern and hence they are not that memory efficient when it comes to
large list data. To overcome this issue we usually use RecyclerView which supports both list and grid layout managers
and supports ViewHolder pattern by default.

> ListView and GridView are deprecated.

<br/>

### RecyclerView

It is a more flexible and advanced form of ListView and GridView, and it supports VieHolder pattern by default.

**Example:**

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              android:gravity="center"
              android:orientation="vertical">

    <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerview_city"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>

</LinearLayout>
```

`list_item.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:tools="http://schemas.android.com/tools"
              android:layout_width="match_parent"
              android:layout_height="wrap_content"
              android:orientation="vertical"
              android:padding="10dp">

    <TextView
            android:id="@+id/tv_city_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            tools:text="City Name"/>

</LinearLayout>
```

`City.kt`

```kotlin
// this class is POJO to hold city objects
data class City(val name: String)
```

`MyAdapter.kt`

```kotlin
class MyAdapter(private val context: Context, private val cityList: ArrayList<City>) :
    RecyclerView.Adapter<MyAdapter.MyViewHolder>() {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val itemView = LayoutInflater.from(context).inflate(R.layout.list_item, parent, false)
        return MyViewHolder(itemView)
    }

    override fun onBindViewHolder(myViewHolder: MyViewHolder, position: Int) {
        val city = cityList[position]
        myViewHolder.setData(city, position)
    }

    override fun getItemCount(): Int {
        return cityList.size
    }

    inner class MyViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {

        private val tvCityName: TextView = itemView.findViewById(R.id.tv_city_name)

        fun setData(city: City, position: Int) {
            tvCityName.text = city.name
        }
    }
}
```

`MainActivity.kt`

```kotlin
class MainActivity : AppCompatActivity() {

    private val cityList = arrayListOf(
        City("Mumbai"),
        City("Banglore"),
        City("Chennai"),
        City("Nashik"),
        City("Pune"),
        City("Nagpur"),
        City("New Delhi"),
        City("Manali"),
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // creating adapter class object and passing context and list as a parameters
        val myAdapter = MyAdapter(this, cityList)

        // initialize recyclerview
        val recyclerView: RecyclerView = findViewById(R.id.recyclerview_city)

        // creating layout manager and setting orientation to linear
        val layoutManager = LinearLayoutManager(this)
        layoutManager.orientation = RecyclerView.VERTICAL

        // setting layout manager to the recyclerview
        recyclerView.layoutManager = layoutManager

        // setting adapter to the RecyclerView
        recyclerView.adapter = myAdapter
    }
}
```

<br/>

### Adapter Class Explained:

`onCreateViewHolder()` :

The purpose of this method is to create required view object which will be used to display items on screen.

> LayoutInflater is used to convert layout of list item into the view object.

`getItemCount()` :

Which returns item count of the list.

`onBindViewHolder()` :

this method is responsible to attach data to the ViewHolder and make them appear in from of the user. It will call each
time when there is need to bind data.

`RecyclerView.Adapter<T>` :

Adapter is the abstract class inside RecyclerView class and contains the above methods. This accepts ViewHolder as a
type parameter.

### Layout Managers:

There are various layout managers available in RecyclerView:

1. **LinearLayoutManager:** Used to show list in linear fashion.
2. **GridLayoutManager:** Used to show list in Grid manner.
3. **StaggeredGridLayoutManager:** Used to show list in grid manner but instead arranging views in staggered manner.