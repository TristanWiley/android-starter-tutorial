# Part 3
## Layouts

Let's look inside the `activity_main.xml` layout. When we open it in Android Studio, we see two views. "Design" and "Text"

First let's open the "Design" section, we see something like this.

![Layouts, bam](https://i.imgur.com/Llk5BQP.jpg)

> Note: If you don't see the layout preview and only see the blueprint, you may need to click the "blue stacks" icon on top top of the view and change it to "Design + Blueprint"

Let's break this apart. On the left side we see a Palette. These contain all the different types of inputs, text, views, layouts etc. Right now, we're defaulted with a TextView. Let's start with that and go from there.

Below the Pallete we see a "Component Tree", this shows us what's in our current Activity. We have a `TextView` inside a `ConstraintLayout`. The ConstraintLayout is a new layout from Google that allows all elements to be constrained in the view somewhat relative to their surroundings.

## XML view
Now let's switch to the XML view by clicking the "Text" button in the bottom toolbar.

You should see the following code (or something very similar)

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</android.support.constraint.ConstraintLayout>
```

I'll break down this code really quickly. The parent element is a `ConstraintLayout`. This layout type makes it easy to create apps that look good on all sized devices by setting constraints on child elements. 

The `TextView` functions exactly as you'd expect, it's purpose is to display text (in our case, "Hello World"). Each element has different parameters such as `layout_width` and `layout_height` (where `wrap_content` and `match_parent` are the most common values).

> Note: The `ConstraintLayout` is an example of a ViewGroup, while the `TextView` is a view. You can think of a ViewGroup as a type of container, it can have child views. [This image demonstrates the concept pretty well](https://i.stack.imgur.com/KSCNf.png).

[*GO TO PART 4 ->*](part4.html)