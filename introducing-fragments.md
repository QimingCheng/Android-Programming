You can get around the letter of the Android law by moving the app’s UI management from the activity to one or morefragments.

Afragmentis a controller object that an activity can deputize to perform tasks. Most commonly, the task is managing a UI. The UI can be an entire screen or just one part of the screen.

A fragment managing a UI is known as aUI fragment.A UI fragment has a view of its ownthat is inflated from a layout file. The fragment’s view contains the interesting UI elements that the user wants to see and interact with.

The activity’s view contains a spotwhere the fragment’s view will be inserted. In fact, while in this chapter the activity will host a single fragment,an activity can have several spots for the views of several fragments.

You can use the fragment\(s\) associated with the activity to compose and recompose the screen as your app and users require. The activity’s view technically stays the same throughout its lifetime, and no laws of Android are violated.

Let’s see how this would work in a list-detail application to display the list and detail together. You would compose the activity’s view from a list fragment and a detail fragment. The detail view would show the details of the selected list item.

Selecting another item should display a new detail view. This is easy with fragments; the activity will replace the detail fragment with another detail fragment \([Figure 7.3](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s02.html#dig_os_vague_fluffy_fragment_diagram)\). No activities need to die for this major view change to happen.



**Figure 7.3  Detail fragment is swapped out**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/fragment_view_changes.png "Detail fragment is swapped out")

Using UI fragments separates the UI of your app into building blocks, which is useful for more than just list-detail applications. Working with individual blocks, it is easy to build tab interfaces, tack on animated sidebars, and more.

Achieving this UI flexibility comes at a cost: more complexity, more moving parts, and more code. You will reap the benefits of using fragments in[Chapter 11](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch11.html)and[Chapter 17](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch17.html). The complexity, however, starts now.

