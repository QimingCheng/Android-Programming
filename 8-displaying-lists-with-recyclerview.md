CriminalIntent’s model layer currently consists of a single instance ofCrime. In this chapter, you will update CriminalIntent to work with a list of crimes. The list will display eachCrime’s title and date, as shown in[Figure 8.1](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08.html#fig_crime_list).



**Figure 8.1  A list of crimes**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/ci_with_custom_list_items.png "A list of crimes")

[Figure 8.2](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08.html#ci_object_diagram_4_abbrev)shows the overall plan for CriminalIntent in this chapter.



**Figure 8.2  CriminalIntent with a list of crimes**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/ci_object_diagram_4_abbrev.png "CriminalIntent with a list of crimes")

In the model layer, you have a new object,CrimeLab, that will be a centralized data stash forCrimeobjects.

Displaying a list of crimes requires a new activity and a new fragment in CriminalIntent’s controller layer:CrimeListActivityandCrimeListFragment.

\(Where areCrimeActivityandCrimeFragmentin[Figure 8.2](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08.html#ci_object_diagram_4_abbrev)? They are part of the detail view, so we are not showing them here. In[Chapter 10](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch10.html), you will connect the list and the detail parts of CriminalIntent.\)

In[Figure 8.2](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08.html#ci_object_diagram_4_abbrev), you can also see the view objects associated withCrimeListActivityandCrimeListFragment. The activity’s view will consist of a fragment-containingFrameLayout. The fragment’s view will consist of aRecyclerView. You will learn more about theRecyclerViewclass later in the chapter.

## Updating CriminalIntent’s Model Layer

The first step is to upgrade CriminalIntent’s model layer from a singleCrimeobject to aListofCrimeobjects.

### SINGLETONS AND CENTRALIZED DATA STORAGE

You are going to store theListof crimes in asingleton.A singleton is a class that allows only one instance of itself to be created.

A singleton exists as long as the application stays in memory, so storing the list in a singleton will keep the crime data available throughout any lifecycle changes in your activities and fragments. Be careful with singleton classes, as they will be destroyed when Android removes your application from memory. TheCrimeLabsingleton is not a solution for long-term storage of data, but it does allow the app to have one owner of the crime data and provides a way to easily pass that data between controller classes. \(You will learn more about long-term data storage in[Chapter 14](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch14.html).\)

\(See the For the More Curious section at the end of this chapter for more about singleton classes.\)

To create a singleton, you create a class with a private constructor and aget\(\)method. If the instance already exists, thenget\(\)simply returns the instance. If the instance does not exist yet, thenget\(\)will call the constructor to create it.

Right-click thecom.bignerdranch.android.criminalintentpackage and chooseNew→Java Class. Name this classCrimeLaband clickOK.

InCrimeLab.java, implementCrimeLabas a singleton with a private constructor and aget\(\)method.



**Listing 8.1  Setting up the singleton \(`CrimeLab.java`\)**

```
public class CrimeLab {
    
private static CrimeLab sCrimeLab;
public static CrimeLab get(Context context) {
if (sCrimeLab == null) {
sCrimeLab = new CrimeLab(context);
}
return sCrimeLab;
}
private CrimeLab(Context context) {
}

}

```

There are a few interesting things in thisCrimeLabimplementation. First, notice thesprefix on thesCrimeLabvariable. You are using this Android convention to make it clear thatsCrimeLabis a static variable.

Also, notice the private constructor on theCrimeLab. Other classes will not be able to create aCrimeLab, bypassing theget\(\)method.

Finally, in theget\(\)method onCrimeLab, you pass in aContextobject. You will make use of thisContextobject in[Chapter 14](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch14.html).

Let’s giveCrimeLabsomeCrimeobjects to store. InCrimeLab’s constructor, create an emptyListofCrimes. Also, add two methods: agetCrimes\(\)method that returns theListand agetCrime\(UUID\)that returns theCrimewith the given ID.



**Listing 8.2  Setting up the`List`of`Crime`objects \(`CrimeLab.java`\)**

```
public class CrimeLab {
    private static CrimeLab sCrimeLab;

    
private List
<
Crime
>
 mCrimes;


    public static CrimeLab get(Context context) {
        ...
    }

    private CrimeLab(Context context) {
        
mCrimes = new ArrayList
<
>
();

    }

    
public List
<
Crime
>
 getCrimes() {
return mCrimes;
}
public Crime getCrime(UUID id) {
for (Crime crime : mCrimes) {
if (crime.getId().equals(id)) {
return crime;
}
}
return null;
}

}

```

List&lt;E&gt;is an interface that supports an ordered list of objects of a given type. It defines methods for retrieving, adding, and deleting elements. A commonly used implementation ofListisArrayList, which uses a regular Java array to store the list elements.

BecausemCrimesholds anArrayList– andArrayListis also aList– bothArrayListandListare valid types formCrimes. In situations like this, we recommend using the interface type for the variable declaration:List. That way, if you ever need to use a different kind ofListimplementation – likeLinkedList, for example – you can do so easily.

ThemCrimesinstantiation line usesdiamond notation,`<>`, which was introduced in Java 7. This shorthand notation tells the compiler to infer the type of items theListwill contain based on the generic argument passed in the variable declaration. Here, the compiler will infer that theArrayListcontainsCrimes because the variable declaration`private List<Crime> mCrimes;`specifiesCrimefor the generic argument. \(The more verbose equivalent, which developers were required to use prior to Java 7, is`mCrimes = new ArrayList<Crime>();`.\)

Eventually, theListwill contain user-createdCrimes that can be saved and reloaded. For now, populate theListwith 100 boringCrimeobjects.



**Listing 8.3  Generating crimes \(`CrimeLab.java`\)**

```
private CrimeLab(Context context) {
    mCrimes = new ArrayList
<
>
();
    
for (int i = 0; i 
<
 100; i++) {
Crime crime = new Crime();
crime.setTitle("Crime #" + i);
crime.setSolved(i % 2 == 0); // Every other one
mCrimes.add(crime);
}

}

```

Now you have a fully loaded model layer with 100 crimes.

