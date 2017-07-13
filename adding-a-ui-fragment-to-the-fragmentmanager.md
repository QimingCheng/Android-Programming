When theFragmentclass was introduced in Honeycomb, theActivityclass was changed to include a piece called theFragmentManager. TheFragmentManageris responsible for managing your fragments and adding their views to the activity’s view hierarchy\([Figure 7.18](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s06.html#fig_fragmentmanager)\).

TheFragmentManagerhandles two things: a list of fragments and a back stack of fragment transactions \(which you will learn about shortly\).



**Figure 7.18  The`FragmentManager`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/fragmentmanager.png "The FragmentManager")

For CriminalIntent, you will only be concerned with theFragmentManager’s list of fragments.

To add a fragment to an activity in code, you make explicit calls to the activity’sFragmentManager. The first step is to get theFragmentManageritself. Do so inonCreate\(Bundle\)inCrimeActivity.java.



**Listing 7.15  Getting the`FragmentManager`\(`CrimeActivity.java`\)**

```
public class CrimeActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_crime);

        
FragmentManager fm = getSupportFragmentManager();

    }
}
```

If you see an error after adding this line of code, check the import statements to make sure that the support version of theFragmentManagerclass was imported.

You callgetSupportFragmentManager\(\)because you are using the support library and theAppCompatActivityclass. If you were not interested in using the support library, then you would subclassActivityand callgetFragmentManager\(\).

### FRAGMENT TRANSACTIONS

Now that you have theFragmentManager, add the following code to give it a fragment to manage. \(We will step through this code afterward. Just get it in for now.\)



**Listing 7.16  Adding a`CrimeFragment`\(`CrimeActivity.java`\)**

```
public class CrimeActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_crime);

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

The best place to start understanding the code you just added is not at the beginning. Instead, find theadd\(…\)operation and the code around it. This code creates and commits afragment transaction.



```
        if (fragment == null) {
            fragment = new CrimeFragment();
            
fm.beginTransaction()
.add(R.id.fragment_container, fragment)
.commit();
```

Fragment transactions are used to add, remove, attach, detach, or replace fragments in the fragment list. They are the heart of how you use fragments to compose and recompose screens at runtime. TheFragmentManagermaintains a back stack of fragment transactions that you can navigate.

TheFragmentManager.beginTransaction\(\)method creates and returns an instance ofFragmentTransaction. TheFragmentTransactionclass uses afluent interface– methods that configureFragmentTransactionreturn aFragmentTransactioninstead of`void`, which allows you to chain them together. So the code highlighted above says,“Create a new fragment transaction, include one add operation in it, and then commit it.”

Theadd\(…\)method is the meat of the transaction. It has two parameters: a container view ID and the newly createdCrimeFragment. The container view ID should look familiar. It is the resource ID of theFrameLayoutthat you defined inactivity\_crime.xml.

A container view ID serves two purposes:

* It tells theFragmentManagerwhere in the activity’s view the fragment’s view should appear.

* It is used as a unique identifier for a fragment in theFragmentManager’s list.

When you need to retrieve theCrimeFragmentfrom theFragmentManager, you ask for it by container view ID:



```
        FragmentManager fm = getSupportFragmentManager();
        
Fragment fragment = fm.findFragmentById(R.id.fragment_container);


        if (fragment == null) {
            fragment = new CrimeFragment();
            fm.beginTransaction()
                .add(R.id.fragment_container, fragment)
                .commit();
        }

```

It may seem odd that theFragmentManageridentifies theCrimeFragmentusing the resource ID of aFrameLayout. But identifying a UI fragment by the resource ID of its container view is built into how theFragmentManageroperates.If you are adding multiple fragments to an activity, you would typically create separate containers with separate IDs for each of those fragments.

Now we can summarize the code you added in[Listing 7.16](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s06.html#hook_up_fragment)from start to finish.

First, you ask theFragmentManagerfor the fragment with a container view ID of`R.id.fragment_container`. If this fragment is already in the list, theFragmentManagerwill return it.

Why would a fragment already be in the list? The call toCrimeActivity.onCreate\(Bundle\)could be in response toCrimeActivitybeingre-createdafter being destroyed on rotation or to reclaim memory. When an activity is destroyed, itsFragmentManagersaves out its list of fragments. When the activity is re-created, the newFragmentManagerretrieves the list and re-creates the listed fragments to make everything as it was before.

On the other hand, if there is no fragment with the given container view ID, thenfragmentwill be null. In this case, you create a newCrimeFragmentand a new fragment transaction that adds the fragment to the list.

CrimeActivityis now hosting aCrimeFragment. Run CriminalIntent to prove it. You should see the view defined infragment\_crime.xml, as shown in[Figure 7.19](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s06.html#empty_episode_1).



**Figure 7.19  `CrimeFragment`’s view hosted by`CrimeActivity`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/ci_end_of_first_chapter.png "CrimeFragment’s view hosted by CrimeActivity")

### THE FRAGMENTMANAGER AND THE FRAGMENT LIFECYCLE

Now that you know about theFragmentManager, let’s take another look at the fragment lifecycle \([Figure 7.20](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch07s06.html#fragment_lifecycle)\).



**Figure 7.20  The fragment lifecycle, again**



![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciUIFragments/fragment_lifecycle_typical.png "The fragment lifecycle, again")

TheFragmentManagerof an activity is responsible for calling the lifecycle methods of the fragments in its list. TheonAttach\(Context\),onCreate\(Bundle\), andonCreateView\(…\)methods are called when you add the fragment to theFragmentManager.

TheonActivityCreated\(Bundle\)method is called after the hosting activity’sonCreate\(Bundle\)method has executed. You are adding theCrimeFragmentinCrimeActivity.onCreate\(Bundle\), so this method will be called after the fragment has been added.

What happens if you add a fragment while the activity is already resumed? In that case, theFragmentManagerimmediately walks the fragment through whatever steps are necessary to get it caught up to the activity’s state. For example, as a fragment is added to an activity that is already resumed, that fragment gets calls toonAttach\(Context\),onCreate\(Bundle\),onCreateView\(…\),onActivityCreated\(Bundle\),onStart\(\), and thenonResume\(\).

Once the fragment’s state is caught up to the activity’s state, the hosting activity’sFragmentManagerwill call further lifecycle methods around the same time it receives the corresponding calls from the OS to keep the fragment’s state aligned with that of the activity.

