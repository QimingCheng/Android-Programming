To host a UI fragment, an activity must:

* define a spot in its layout for the fragment’s view

* manage the lifecycle of the fragment instance

### THE FRAGMENT LIFECYCLE

[Figure 7.11](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s04.html#fragment_lifecycle_1)shows the fragment lifecycle. It is similar to the activity lifecycle: It has stopped, paused, and resumed states, and it has methods you can override to get things done at critical points – many of which correspond to activity lifecycle methods.



**Figure 7.11  Fragment lifecycle diagram**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/fragment_lifecycle_typical.png "Fragment lifecycle diagram")

The correspondence is important.Because a fragment works on behalf of an activity, its state should reflect the activity’s state. Thus, it needs corresponding lifecycle methods to handle the activity’s work.

One critical difference between the fragment lifecycle and the activity lifecycle is that fragment lifecycle methods arecalled by the hosting activity, not the OS. The OS knows nothing about the fragments that an activity is using to manage things. Fragments are the activity’s internal business.

You will see more of the fragment lifecycle methods as you continue building CriminalIntent.

### TWO APPROACHES TO HOSTING

You have two options when it comes to hosting a UI fragment in an activity:

* add the fragment to the activity’slayout

* add the fragment in the activity’scode

The first approach is known as using alayout fragment. It is straightforward but inflexible. If you add the fragment to the activity’s layout, you hardwire the fragment and its view to the activity’s view and cannot swap out that fragment during the activity’s lifetime.

The second approach, adding the fragment to the activity’s code, is more complex – but it is the only way to havecontrol at runtime over your fragments. You determine when the fragment is added to the activity and what happens to it after that. You can remove the fragment, replace it with another, and then add the first fragment back again.

Thus, to achieve real UI flexibility you must add your fragment in code. This is the approach you will use forCrimeActivity’s hosting of aCrimeFragment. The code details will come later in the chapter. First, you are going to defineCrimeActivity’s layout.

### DEFINING A CONTAINER VIEW

You will be adding a UI fragment in the hosting activity’s code, but you still need to make a spot for the fragment’s view in the activity’s view hierarchy. InCrimeActivity’s layout, this spot will be theFrameLayoutshown in[Figure 7.12](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s04.html#fig_fragment_activity_layout).



**Figure 7.12  Fragment-hosting layout for`CrimeActivity`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/activity_fragment_layout.png "Fragment-hosting layout for CrimeActivity")

ThisFrameLayoutwill be thecontainer viewfor aCrimeFragment. Notice that the container view is completely generic; it does not name theCrimeFragmentclass. You can and will use this same layout to host other fragments.

LocateCrimeActivity’s layout atres/layout/activity\_crime.xml. Open this file and replace the default layout with theFrameLayoutdiagrammed in[Figure 7.12](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s04.html#fig_fragment_activity_layout). Your XML should match[Listing 7.5](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s04.html#create_fragment_activity_xml).



**Listing 7.5  Creating the fragment container layout \(`activity_crime.xml`\)**

```
<
FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent" /
>
```

Note that whileactivity\_crime.xmlconsists solely of a container view for a single fragment, an activity’s layout can be more complex anddefine multiple container viewsas well as widgets of its own.

You can preview your layout file or run CriminalIntent to check your code. You will see an emptyFrameLayoutbelow a toolbar containing the text CriminalIntent \([Figure 7.13](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s04.html#fig_ci_empty_frame_layout)\). \(If the preview window does not render the screen correctly, or you see errors, build the project by selectingBuild→Rebuild Project. If that still does not work correctly, run the app on your emulator or device. As of this writing, the preview window can be finicky.\)



**Figure 7.13  An empty`FrameLayout`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_empty_frame_layout.png "An empty FrameLayout")

TheFrameLayoutis empty because theCrimeActivityis not yet hosting a fragment. Later, you will write code that puts a fragment’s view inside thisFrameLayout. But first, you need to create a fragment.

\(The toolbar at the top of your app is included automatically because of the way you configured your activity. You will learn more about the toolbar in[Chapter 13](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch13.html).\)

