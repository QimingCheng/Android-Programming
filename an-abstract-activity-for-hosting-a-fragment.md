In a moment, you will create theCrimeListActivityclass that will host aCrimeListFragment. First, you are going to set up a view forCrimeListActivity.

### A GENERIC FRAGMENT-HOSTING LAYOUT

ForCrimeListActivity, you can simply reuse the layout defined inactivity\_crime.xml\(which is copied in[Listing 8.4](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s02.html#pl_activity_crime_layout)\). This layout provides aFrameLayoutas a container view for a fragment, which is then named in the activity’s code.



**Listing 8.4  `activity_crime.xml`is already generic**

```
<
?xml version="1.0" encoding="utf-8"?
>
<
FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    /
>
```

Becauseactivity\_crime.xmldoes not name a particular fragment, you can use it for any activity hosting a single fragment. Rename itactivity\_fragment.xmlto reflect its larger scope.

In the project tool window, right-clickres/layout/activity\_crime.xml. \(Be sure to right-clickactivity\_crime.xmland notfragment\_crime.xml.\)

From the context menu, selectRefactor→Rename.... Rename this layoutactivity\_fragment.xmland clickRefactor.

When you rename a resource, the references to it should be updated automatically. If you see an error inCrimeActivity.java, then you need to manually update the reference inCrimeActivity, as shown in[Listing 8.5](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s02.html#fix_up_crimeactivity).



**Listing 8.5  Updating layout file for`CrimeActivity`\(`CrimeActivity.java`\)**

```
public class CrimeActivity extends AppCompatActivity {
    /** Called when the activity is first created. */
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
setContentView(R.layout.activity_crime);
setContentView(R.layout.activity_fragment);


        FragmentManager fm = getSupportFragmentManager();
        Fragment fragment = fm.findFragmentById(R.id.fragment_container);

        if (fragment == null) {
            fragment = new CrimeFragment();
            fm.beginTransaction()
                .add(R.id.fragment_container, fragment)
                .commit();
        }
    }
}

```

### AN ABSTRACT ACTIVITY CLASS

To create theCrimeListActivityclass, you could reuseCrimeActivity’s code. Look back at the code you wrote forCrimeActivity\(which is copied in[Listing 8.6](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s02.html#pl_criminalintent_crimeactivity_review)\). It is simple and almost generic. In fact, the only nongeneric code is the instantiation of theCrimeFragmentbefore it is added to theFragmentManager.



**Listing 8.6  `CrimeActivity`is almost generic \(`CrimeActivity.java`\)**

```
public class CrimeActivity extends AppCompatActivity {
    /** Called when the activity is first created. */
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_fragment);

        FragmentManager fm = getSupportFragmentManager();
        Fragment fragment = fm.findFragmentById(R.id.fragment_container);

        if (fragment == null) {
            fragment = 
new CrimeFragment();

            fm.beginTransaction()
                .add(R.id.fragment_container, fragment)
                .commit();
        }
    }
}

```

Nearly every activity you will create in this book will require the same code. To avoid typing it again and again, you are going to stash it in an abstract class.

Right-click on thecom.bignerdranch.android.criminalintentpackage, selectNew→Java Class, and name the new classSingleFragmentActivity. Make this class a subclass ofAppCompatActivityand make it an abstract class. Your generated file should look like this:



**Listing 8.7  Creating an abstract Activity \(`SingleFragmentActivity.java`\)**

```
public abstract class SingleFragmentActivity extends AppCompatActivity {

}

```

Now, add the following code toSingleFragmentActivity.java. Except for the highlighted portions, it is identical to your oldCrimeActivitycode.



**Listing 8.8  Adding a generic superclass \(`SingleFragmentActivity.java`\)**

```
public abstract class SingleFragmentActivity extends AppCompatActivity {

    
protected abstract Fragment createFragment();
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_fragment);
FragmentManager fm = getSupportFragmentManager();
Fragment fragment = fm.findFragmentById(R.id.fragment_container);
if (fragment == null) {
fragment =
createFragment();
fm.beginTransaction()
.add(R.id.fragment_container, fragment)
.commit();
}
}

}

```

In this code, you set the activity’s view to be inflated fromactivity\_fragment.xml. Then you look for the fragment in theFragmentManagerin that container, creating and adding it if it does not exist.

The only difference between the code in[Listing 8.8](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s02.html#pl_criminalintent_singlefragmentactivity)and the code inCrimeActivityis an abstract method namedcreateFragment\(\)that you use to instantiate the fragment. Subclasses ofSingleFragmentActivitywill implement this method to return an instance of the fragment that the activity is hosting.

#### Using an abstract class

Try it out withCrimeActivity. ChangeCrimeActivity’s superclass toSingleFragmentActivity, remove the implementation ofonCreate\(Bundle\), and implement thecreateFragment\(\)method as shown in[Listing 8.9](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s02.html#pl_criminalintent_crimeactivity_revisited).



**Listing 8.9  Cleaning up`CrimeActivity`\(`CrimeActivity.java`\)**

```
public class CrimeActivity extends 
AppCompatActivity
SingleFragmentActivity
 {
    
/** Called when the activity is first created. */
@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_fragment);
FragmentManager fm = getSupportFragmentManager();
Fragment fragment = fm.findFragmentById(R.id.fragment_container);
if (fragment == null) {
fragment = new CrimeFragment();
fm.beginTransaction()
.add(R.id.fragment_container, fragment)
.commit();
}
}
@Override
protected Fragment createFragment() {
return new CrimeFragment();
}

}

```

#### Creating the new controllers

Now, you will create the two new controller classes:CrimeListActivityandCrimeListFragment.

Right-click on thecom.bignerdranch.android.criminalintentpackage, selectNew→Java Class, and name the classCrimeListActivity.

Modify the newCrimeListActivityclass to also subclassSingleFragmentActivityand implement thecreateFragment\(\)method.



**Listing 8.10  Implementing`CrimeListActivity`\(`CrimeListActivity.java`\)**

```
public class CrimeListActivity 
extends SingleFragmentActivity
 {

    
@Override
protected Fragment createFragment() {
return new CrimeListFragment();
}

}

```

If you have other methods in yourCrimeListActivity, such asonCreate, remove them. LetSingleFragmentActivitydo its job and keepCrimeListActivitysimple.

TheCrimeListFragmentclass has not yet been created. Let’s remedy that.

Right-click on thecom.bignerdranch.android.criminalintentpackage again, selectNew→Java Class, and name the classCrimeListFragment.



**Listing 8.11  Implementing`CrimeListFragment`\(`CrimeListFragment.java`\)**

```
public class CrimeListFragment 
extends Fragment
 {

      
// Nothing yet


}

```

For now,CrimeListFragmentwill be an empty shell of a fragment. You will work with this fragment later in the chapter.

Now your activity code is nice and tidy. AndSingleFragmentActivitywill save you a lot of typing and time as you proceed through the book.

#### Declaring CrimeListActivity

Now that you have createdCrimeListActivity, you must declare it in the manifest. In addition, you want the list of crimes to be the first screen that the user sees when CriminalIntent is launched, soCrimeListActivityshould be the launcher activity.

In the manifest, declareCrimeListActivityand move the launcher intent filter fromCrimeActivity’s declaration toCrimeListActivity’s declaration.



**Listing 8.12  Declaring`CrimeListActivity`as the launcher activity \(`AndroidManifest.xml`\)**

```
<
application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme" 
>
<
activity android:name=".CrimeListActivity"
>
<
intent-filter
>
<
action android:name="android.intent.action.MAIN" /
>
<
category android:name="android.intent.category.LAUNCHER" /
>
<
/intent-filter
>
<
/activity
>
<
activity android:name=".CrimeActivity"
>
<
intent-filter
>
<
action android:name="android.intent.action.MAIN" /
>
<
category android:name="android.intent.category.LAUNCHER" /
>
<
/intent-filter
>
<
/activity
>
<
/application
>
```

CrimeListActivityis now the launcher activity. Run CriminalIntent and you will seeCrimeListActivity’sFrameLayouthosting an emptyCrimeListFragment, as shown in[Figure 8.3](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s02.html#fig_blank_list_activity).



**Figure 8.3  Blank`CrimeListActivity`screen**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/ci_blank_list_activity.png "Blank CrimeListActivity screen")

