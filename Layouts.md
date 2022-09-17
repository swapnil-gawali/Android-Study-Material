# Layouts

### Linear Layout

LinearLayout used to align views in a single direction, vertically or horizontally. You can specify the layout direction
with the `android:orientation` attribute.
<br/>

### Relative Layout

Relative Layout used to align its views relative to each other.
<br/>

### Frame Layout

Frame Layout is the lightweight layout which puts views on top of each other.
<br/>

### Coordinator Layout

Coordinator Layout is super powered `FrameLayout`. It has additional level of control over it’s child views. It
coordinates the animations and transitions of child views with one another.

Using `CoordinatorLayout` we can customise behaviour of `Toolbar` as well as `BottomNavigationBar` while scrolling
`RecyclerView`. We can also customise Floating action button behaviour for e.g. when we show snackbar and
FloatingActionButton together the SnackBar will overlap FAB if not using Coordinator layout. But when using
CoordinatorLayout FAB will auto push upwards using animation while showing SnackBar.

CoordinatorLayout also useful to show Collapsing Toolbar.
<br/>

### `</merge>` & `</include>`

`<merge/>` is useful because it can get rid of unneeded ViewGroups, i.e. layouts that are simply used to wrap other
views and serve no purpose themselves.

For example, if you were to `<include/>` a layout from another file without using merge, the two files might look
something like this:

layout1.xml:

```xml

<FrameLayout>
    <include layout="@layout/layout2"/>
</FrameLayout>
```

layout2.xml:

```xml

<FrameLayout>
    <TextView/>
    <TextView/>
</FrameLayout>
```

which is functionally equivalent to this single layout:

```xml

<FrameLayout>
    <FrameLayout>
        <TextView/>
        <TextView/>
    </FrameLayout>
</FrameLayout>
```

That `FrameLayout` in layout2.xml may not be useful. <merge/> helps get rid of it. Here's what it looks like using
merge (
layout1.xml doesn't change):

layout2.xml:

```xml

<merge>
    <TextView/>
    <TextView/>
</merge>
```

This is functionally equivalent to this layout:

```xml

<FrameLayout>
    <TextView/>
    <TextView/>
</FrameLayout>
```

but since you are using `<include/>` you can reuse the layout elsewhere. It doesn't have to be used to replace only
FrameLayouts - you can use it to replace any layout that isn't adding something useful to the way your view
looks/behaves.
<br/>

### ViewStub

A `ViewStub` is an invisible, zero-sized View that can be used to lazily inflate layout resources at runtime. When a
`ViewStub` is made visible, or when `inflate()` is invoked, the layout resource is inflated.

```xml

<ViewStub android:id="@+id/stub"
          android:inflatedId="@+id/subTree"
          android:layout="@layout/mySubTree"
          android:layout_width="120dip"
          android:layout_height="40dip"/>
```

Inflating the ViewStub inside Fragment/Activity:

```kotlin
val stub: ViewStub = findViewById(R.id.stub)
val inflated: View = stub.inflate()
```