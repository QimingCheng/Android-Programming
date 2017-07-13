Now, you wantCrimeListFragmentto display a list of crimes to the user. To do this, you will use aRecyclerView.

RecyclerViewis a subclass ofViewGroup. It displays a list of childViewobjects, one for each item in your list of items. Depending on the complexity of what you need to display, these childViews can be complex or very simple.

For your first implementation, each item in the list will display the title and date of aCrime. TheViewobject on each row will be aLinearLayoutcontaining twoTextViews, as shown in[Figure 8.4](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_crime_list_showing_views).



**Figure 8.4  A`RecyclerView`with child`View`s**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/ci_with_text_views.png "A RecyclerView with child Views")

[Figure 8.4](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_crime_list_showing_views)shows 12 rows ofViews. Later you will be able to run CriminalIntent and swipe to scroll through 100Views to see all of yourCrimes. Does that mean that you have 100Viewobjects in memory? Thanks to yourRecyclerView, no.

Creating aViewfor every item in the list all at once could easily become unworkable. As you can imagine, a list can have far more than 100 items, and your list items can be much more involved than your simple implementation here. Also, aCrimeonly needs aViewwhen it is onscreen, so there is no need to have 100Views ready and waiting. It would make far more sense to create view objects only as you need them.

RecyclerViewdoes just that. Instead of creating 100Views, it creates 12 – enough to fill the screen. When a view is scrolled off the screen,RecyclerViewreuses it rather than throwing it away. In short, it lives up to its name: It recycles views over and over.

### VIEWHOLDERS AND ADAPTERS

TheRecyclerView’s only responsibilities are recyclingTextViews and positioning them onscreen. To get theTextViews in the first place, it works with two classes that you will build in a moment: anAdaptersubclass and aViewHoldersubclass.

TheViewHolder’s job is small, so let’s talk about it first. TheViewHolderdoes one thing: It holds on to aView\([Figure 8.5](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_view_holder)\).



**Figure 8.5  The lowly`ViewHolder`**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/view_holder.png "The lowly ViewHolder")

A small job, but that is whatViewHolders do. A typicalViewHoldersubclass looks like this:



**Listing 8.13  A typical`ViewHolder`subclass**

```
public class ListRow extends RecyclerView.ViewHolder {
    public ImageView mThumbnail;

    public ListRow(View view) {
        super(view);

        mThumbnail = (ImageView) view.findViewById(R.id.thumbnail);
    }
}

```

You can then create aListRowand access bothmThumbnail, which you created yourself, anditemView, a field that your superclassRecyclerView.ViewHolderassigns for you. TheitemViewfield is yourViewHolder’s reason for existing: It holds a reference to the entireViewyou passed intosuper\(view\).



**Listing 8.14  Typical usage of a`ViewHolder`**

```
ListRow row = new ListRow(inflater.inflate(R.layout.list_row, parent, false));
View view = row.itemView;
ImageView thumbnailView = row.mThumbnail;

```

ARecyclerViewnever createsViews by themselves. It always createsViewHolders, which bring theiritemViews along for the ride \([Figure 8.6](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_view_holders_and_recyclerview)\).



**Figure 8.6  A`RecyclerView`with its`ViewHolder`s**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/view_holders_and_recyclerview.png "A RecyclerView with its ViewHolders")

When theViewis simple,ViewHolderhas few responsibilities. For more complicatedViews, theViewHoldermakes wiring up the different parts ofitemViewto aCrimesimpler and more efficient. You will see how this works later on in this chapter, when you build a complexViewyourself.

#### Adapters

[Figure 8.6](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_view_holders_and_recyclerview)is somewhat simplified.RecyclerViewdoes not createViewHolders itself. Instead, it asks anadapter. An adapter is a controller object that sits between theRecyclerViewand the data set that theRecyclerViewshould display.

The adapter is responsible for:

* creating the necessaryViewHolders

* bindingViewHolders to data from the model layer

To build an adapter, you first define a subclass ofRecyclerView.Adapter. Your adapter subclass will wrap the list of crimes you get fromCrimeLab.

When theRecyclerViewneeds a view object to display, it will have a conversation with its adapter.[Figure 8.7](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#array_adapter_interaction)shows an example of a conversation that aRecyclerViewmight initiate.



**Figure 8.7  A scintillating`RecyclerView`-`Adapter`conversation**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/array_adapter_interaction.png "A scintillating RecyclerView-Adapter conversation")

First, theRecyclerViewasks how many objects are in the list by calling the adapter’sgetItemCount\(\)method.

Then theRecyclerViewcalls the adapter’sonCreateViewHolder\(ViewGroup, int\)method to create a newViewHolder, along with its juicy payload: aViewto display.

Finally, theRecyclerViewcallsonBindViewHolder\(ViewHolder, int\). TheRecyclerViewwill pass aViewHolderinto this method along with the position. The adapter will look up the model data for that position andbindit to theViewHolder’sView. To bind it, the adapter fills in theViewto reflect the data in the model object.

After this process is complete,RecyclerViewwill place a list item on the screen. Note thatonCreateViewHolder\(ViewGroup, int\)will happen a lot less often thanonBindViewHolder\(ViewHolder, int\). Once enoughViewHolders have been created,RecyclerViewstops callingonCreateViewHolder\(…\). Instead, it saves time and memory by recycling oldViewHolders.

### USING A RECYCLERVIEW

Enough talk; time for the implementation. TheRecyclerViewclass lives in one of Google’s many support libraries. The first step to using aRecyclerViewis to add the RecyclerView library as a dependency.

Navigate to your project structure window withFile→Project Structure.... Select theappmodule on the left, then theDependenciestab. Use the+button and chooseLibrary dependencyto add a dependency.

Find and select therecyclerview-v7library and clickOKto add the library as a dependency, as shown in[Figure 8.8](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#recyclerview_dependency).



**Figure 8.8  Adding the RecyclerView dependency**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/ci_add_recyclerview_dependency.png "Adding the RecyclerView dependency")

YourRecyclerViewwill live inCrimeListFragment’s layout file. First, you must create the layout file. Right-click on theres/layoutdirectory and selectNew→Layout resource file. Name the filefragment\_crime\_listand clickOKto create the file.

Open the newfragment\_crime\_listfile and modify the root view to be aRecyclerViewand to give it an ID attribute.



**Listing 8.15  Adding`RecyclerView`to a layout file \(`fragment_crime_list.xml`\)**

```
<
LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="match_parent"
android:layout_height="match_parent"
>
<
/LinearLayout
>
<
android.support.v7.widget.RecyclerView
xmlns:android="http://schemas.android.com/apk/res/android"
android:id="@+id/crime_recycler_view"
android:layout_width="match_parent"
android:layout_height="match_parent"/
>
```

Now thatCrimeListFragment’s view is set up, hook up the view to the fragment. ModifyCrimeListFragmentto use this layout file and to find theRecyclerViewin the layout file, as shown in[Listing 8.16](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#pl_crime_list_fragmnet_view_creation).



**Listing 8.16  Setting up the view for`CrimeListFragment`\(`CrimeListFragment.java`\)**

```
public class CrimeListFragment extends Fragment {

    
// Nothing yet
private RecyclerView mCrimeRecyclerView;
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container,
Bundle savedInstanceState) {
View view = inflater.inflate(R.layout.fragment_crime_list, container, false);
mCrimeRecyclerView = (RecyclerView) view
.findViewById(R.id.crime_recycler_view);
mCrimeRecyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));
return view;
}


}

```

Note that as soon as you create yourRecyclerView, you give it another object called aLayoutManager.RecyclerViewrequires aLayoutManagerto work. If you forget to give it one, it will crash.

RecyclerViewdoes not position items on the screen itself. It delegates that job to theLayoutManager. TheLayoutManagerpositions every item and also defines how scrolling works. So ifRecyclerViewtries to do those things when theLayoutManageris not there, theRecyclerViewwill immediately fall over and die.

There are a few built-inLayoutManagers to choose from, and you can find more as third-party libraries. You are using theLinearLayoutManager, which will position the items in the list vertically. Later on in this book, you will useGridLayoutManagerto arrange items in a grid instead.

Run the app. You should again see a blank screen, but now you are looking at an emptyRecyclerView. You will not see anyCrimes represented on the screen until theAdapterandViewHolderimplementations are defined.

### A VIEW TO DISPLAY

Each item displayed on theRecyclerViewwill have its own view hierarchy, exactly the wayCrimeFragmenthas a view hierarchy for the entire screen. You create a new layout for a list item view the same way you do for the view of an activity or a fragment. In the project tool window, right-click theres/layoutdirectory and chooseNew→Layout resource file. In the dialog that appears, name the filelist\_item\_crimeand clickOK.

Update your layout file to add the twoTextViews as shown in[Figure 8.9](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_list_item_crime_linearlayout).



**Figure 8.9  Updating the list item layout file \(`list_item_crime.xml`\)**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/list_item_crime.png "Updating the list item layout file \(list\_item\_crime.xml\)")

Take a look at the design preview, and you will see that you have created exactly one row of the completed product. In a moment, you will see howRecyclerViewwill create those rows for you.

### IMPLEMENTING A VIEWHOLDER AND AN ADAPTER

The next job is to define theViewHolderthat will inflate and own your layout. Define it as an inner class inCrimeListFragment.



**Listing 8.17  The beginnings of a`ViewHolder`\(`CrimeListFragment.java`\)**

```
public class CrimeListFragment extends Fragment {
    ...
     
private class CrimeHolder extends RecyclerView.ViewHolder {
public CrimeHolder(LayoutInflater inflater, ViewGroup parent) {
super(inflater.inflate(R.layout.list_item_crime, parent, false));
}
}

}

```

InCrimeHolder’s constructor, you inflatelist\_item\_crime.xml. Immediately you pass it intosuper\(…\),ViewHolder’s constructor. The baseViewHolderclass will then hold on to thefragment\_crime\_list.xmlview hierarchy. If you need that view hierarchy, you can find it inViewHolder’sitemViewfield.

CrimeHolderis all skin and bones right now. Later in the chapter,CrimeHolderwill beef up as you give it more work to do.

With theViewHolderdefined, create the adapter.



**Listing 8.18  The beginnings of an adapter \(`CrimeListFragment.java`\)**

```
public class CrimeListFragment extends Fragment {
    ...
    
private class CrimeAdapter extends RecyclerView.Adapter
<
CrimeHolder
>
 {
private List
<
Crime
>
 mCrimes;
public CrimeAdapter(List
<
Crime
>
 crimes) {
mCrimes = crimes;
}
}

}

```

\(The code in[Listing 8.18](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#pl_crime_list_fragment_adapter_creation)will not compile. You will fix this in a moment.\)

When theRecyclerViewneeds to display a newViewHolderor connect aCrimeobject to an existingViewHolder, it will ask this adapter for help by calling a method on it. TheRecyclerViewitself will not know anything about theCrimeobject, but theAdapterwill know all ofCrime’s intimate and personal details.

Next, implement three method overrides inCrimeAdapter. \(You can automatically generate these overrides by putting your cursor on top of`extends`, keying in Option-Return \(Alt+Enter\), selectingImplement methods, and clicking OK. Then you only need to fill in the bodies.\)



**Listing 8.19  Filling out`CrimeAdapter`\(`CrimeListFragment.java`\)**

```
    private class CrimeAdapter extends RecyclerView.Adapter
<
CrimeHolder
>
 {
        ...
        
@Override
public CrimeHolder onCreateViewHolder(ViewGroup parent, int viewType) {
LayoutInflater layoutInflater = LayoutInflater.from(getActivity());
return new CrimeHolder(layoutInflater, parent);
}
@Override
public void onBindViewHolder(CrimeHolder holder, int position) {
}
@Override
public int getItemCount() {
return mCrimes.size();
}

    }

```

onCreateViewHolderis called by theRecyclerViewwhen it needs a newViewHolderto display an item with. In this method, you create aLayoutInflaterand use it to construct a newCrimeHolder.

Your adapter must have an override foronBindViewHolder\(…\), but for now you can leave it empty. In a moment, you will return to it.

Now that you have anAdapter, connect it to yourRecyclerView. Implement a method calledupdateUIthat sets upCrimeListFragment’s UI. For now it will create aCrimeAdapterand set it on theRecyclerView.



**Listing 8.20  Setting an`Adapter`\(`CrimeListFragment.java`\)**

```
public class CrimeListFragment extends Fragment {

    private RecyclerView mCrimeRecyclerView;
    
private CrimeAdapter mAdapter;


    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_crime_list, container, false);

        mCrimeRecyclerView = (RecyclerView) view
                .findViewById(R.id.crime_recycler_view);
        mCrimeRecyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));

        
updateUI();


        return view;
    }

    
private void updateUI() {
CrimeLab crimeLab = CrimeLab.get(getActivity());
List
<
Crime
>
 crimes = crimeLab.getCrimes();
mAdapter = new CrimeAdapter(crimes);
mCrimeRecyclerView.setAdapter(mAdapter);
}

    ...
}

```

In later chapters, you will add more toupdateUI\(\)as configuring your UI gets more involved.

Run CriminalIntent and scroll through your newRecyclerView, which should look like[Figure 8.10](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s03.html#fig_crime_list_no_data).



**Figure 8.10  A beautiful list of… beautiful, beautiful beautifuls**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/without_binding.png "A beautiful list of… beautiful, beautiful beautifuls")

Hmm. Looking a little repetitive there, Mr.RecyclerView. Swipe or drag down, and you will see even more identical views scroll across your screen.

In the screenshot above, there are 11 rows, which means thatonCreateViewHolder\(…\)was called 11 times. If you scroll down, a few moreCrimeHolders may be created, but at a certain pointRecyclerViewwill stop creating newCrimeHolders. Instead, it will recycle oldCrimeHolders as they scroll off the top of the screen.RecyclerView, you were named well indeed.

For the moment, every row is identical. In your next step, you will fill eachCrimeHolderwith fresh information as it is recycled by binding to it.

