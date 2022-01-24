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

`getItemViewType()`

Using this method we can define multiple item layout and set to the RecyclerView based on the position.

### Layout Managers:

There are various layout managers available in RecyclerView:

1. **LinearLayoutManager:** Used to show list in linear fashion.
2. **GridLayoutManager:** Used to show list in Grid manner.
3. **StaggeredGridLayoutManager:** Used to show list in grid manner but instead arranging views in staggered manner.

#### LinearLayoutManager

```kotlin
val layoutManager = LinearLayoutManager(this)

// vertically
layoutManager.orientation = RecyclerView.VERTICAL

// OR horizontally
layoutManager.orientation = RecyclerView.HORIZONTAL
```

#### GridLayoutManager

```kotlin
val layoutManager = GridLayoutManager(this, 2) // 2 is span count i.e. number of items in a row 

// vertically
layoutManager.orientation = RecyclerView.VERTICAL

// OR horizontally
layoutManager.orientation = RecyclerView.HORIZONTAL
```

#### StaggeredGridLayoutManager

```kotlin
val layoutManager = StaggeredGridLayoutManager(2, RecyclerView.VERTICAL)
```

In the above example, 2 is span count and second param is orientation of RecyclerView.

<br/>

### Paging 2 - PagedListAdapter

When we use `RecyclerView.Adapter`, it loads all the data at once. Loading all the data once can affect application
performance but also consumes unnecessary resources and memory.

To overcome this situation, android provides **Paging** library which contains the class known as `PagedListAdapter`.
This class is nothing but a subclass of `RecyclerView.Adapter`, and it is a more optimized and advanced version of its
legacy `RecyclerView.Adapter`.

`PagedListAdapter` used to load small chunks of data at a time. Loading partial data on demand reduces resource usage
and increase application performance.

We can load data from network as well as local database using **Paging** library.

For using paging library, first we need to add it to the app level `build.gradle`

```kotlin
implementation "androidx.paging:paging-runtime-ktx:2.1.2"
```

// TODO - pending

<br/>

### ViewPager 2

ViewPager is used to create swipable items. We can use `RecyclerView.Adapter` for ViewPager.

<br/>

#### Creating Swipable Views

`pager_item.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/root_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    <TextView
            android:id="@+id/tv_page_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            tools:text="Item Name"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical">

    <androidx.viewpager2.widget.ViewPager2
            android:id="@+id/view_pager_2"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>

</LinearLayout>
```

`Page.kt`

```kotlin
data class Page(val title: String, val color: String)
```

`MyPagerAdapter.kt`

```kotlin
class MyPagerAdapter(private val context: Context, private val pageList: ArrayList<Page>) :
    RecyclerView.Adapter<MyPagerAdapter.MyViewHolder>() {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val itemView = LayoutInflater.from(context).inflate(R.layout.pager_item, parent, false)
        return MyViewHolder(itemView)
    }

    override fun onBindViewHolder(myViewHolder: MyViewHolder, position: Int) {
        val page = pageList[position]
        myViewHolder.setData(page, position)
    }

    override fun getItemCount(): Int {
        return pageList.size
    }

    inner class MyViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {

        private val rootLayout: ConstraintLayout = itemView.findViewById(R.id.root_layout)
        private val tvPageName: TextView = itemView.findViewById(R.id.tv_page_name)

        fun setData(page: Page, position: Int) {
            tvPageName.text = page.title
            rootLayout.setBackgroundColor(Color.parseColor(page.color))
        }
    }
}
```

`MainActivity.kt`

```kotlin
class MainActivity : AppCompatActivity() {

    private val pageList = arrayListOf(
        Page("Yellow", "#DFFF00"),
        Page("Orange", "#FFBF00"),
        Page("Red", "#FF7F50"),
        Page("Pink", "#DE3163"),
        Page("Green", "#9FE2BF"),
        Page("Light Blue", "#40E0D0"),
        Page("Blue", "#6495ED"),
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // creating adapter class object and passing context and list as a parameters
        val myPagerAdapter = MyPagerAdapter(this, pageList)

        // initializing viewpager
        val viewPager: ViewPager2 = findViewById(R.id.view_pager_2)

        // setting up adapter
        viewPager.adapter = myPagerAdapter

        // setting up orientation
        viewPager.orientation = ViewPager2.ORIENTATION_HORIZONTAL

    }
}
```

We can change the ViewPager orientation using following:

```kotlin
// vertical
viewPager.orientation = ViewPager2.ORIENTATION_VERTICAL

// horizontal
viewPager.orientation = ViewPager2.ORIENTATION_HORIZONTAL
```

Viewpager supports `onPageChangedListener` which gets invoked whenever we change or select page.

```kotlin
viewPager.registerOnPageChangeCallback(object : ViewPager2.OnPageChangeCallback() {
    override fun onPageSelected(position: Int) {
        super.onPageSelected(position)
        Log.wtf("ViewPager2", "Selected page is $position")
    }
})
```

<br/>

#### Creating Swipable Fragments

For swipable fragments, we need to use `FragmentStateAdapter` which is the subclass of `RecyclerView.Adapter`. The
advantage of using this adapter is, we can manage fragments within ViewPager.

`fragment_page.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/root_layout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    <TextView
            android:id="@+id/tv_page_name"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="18sp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent"
            tools:text="Item Name"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

`PagerFragment.kt`

```kotlin
class PagerFragment(private val page: Page) : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_page, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        val rootLayout: ConstraintLayout = view.findViewById(R.id.root_layout)
        val tvPageName: TextView = view.findViewById(R.id.tv_page_name)

        tvPageName.text = page.title
        rootLayout.setBackgroundColor(Color.parseColor(page.color))
    }
}
```

`MyFragmentPagerAdapter.kt`

```kotlin
class MyFragmentPagerAdapter(private val context: Context, private val pageList: ArrayList<Page>) :
    FragmentStateAdapter(context as FragmentActivity) {
    override fun getItemCount(): Int {
        return pageList.size
    }

    override fun createFragment(position: Int): Fragment {
        val page = pageList[position]
        return PagerFragment(page)
    }
}
```

In the above `main_activity.xml` and `MainActivity.kt` implementation will remain same.

> `FragmentStateAdapter` stores state of the fragment in memory and restore state whenever fragment is created for
> for second time.

<br/>

### Spinner

Spinner are basically just a dropdown menus, and it can be implemented using `ArrayAdapter`.

<br/>

#### Creating Spinner from the Resource (string.xml)

`strings.xml`

```xml

<resources>
    <string-array name="countries">
        <item>All Countries</item>
        <item>India</item>
        <item>UK</item>
        <item>USA</item>
        <item>France</item>
        <item>Japan</item>
        <item>Germany</item>
        <item>Russia</item>
    </string-array>
</resources>
```

`activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="10dp">

    <androidx.appcompat.widget.AppCompatSpinner
            android:id="@+id/spinner"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/>

</LinearLayout>
```

`MainActivity.kt`

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // initializing spinner
        val spinner: Spinner = findViewById(R.id.spinner)

        // create ArrayAdapter and set resource array to it
        val adapter = ArrayAdapter.createFromResource(this, R.array.countries, android.R.layout.simple_spinner_item)

        // set dropdown resource layout
        adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)

        // setting up adapter
        spinner.adapter = adapter

        // setting up onItemSelectListener
        spinner.onItemSelectedListener = object : AdapterView.OnItemSelectedListener {
            override fun onNothingSelected(parent: AdapterView<*>?) {
            }

            override fun onItemSelected(parent: AdapterView<*>?, view: View?, position: Int, id: Long) {
                val selectedItem = parent!!.getItemAtPosition(position)
                Toast.makeText(this@MainActivity, "$selectedItem Selected", Toast.LENGTH_SHORT).show()
            }
        }
    }
}
```
