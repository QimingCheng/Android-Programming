Binding is taking Java code \(like model data in aCrime, or click listeners\) and hooking it up to a widget. So far, in all the exercises up until this point in the book, you bound each and every time you inflated a view. This meant there was no need to split that work into its own method. However, now that views are being recycled, it pays to have creation in one place and binding in another.

All the code that will do the real work of binding will go inside yourCrimeHolder. That work starts with pulling out all the widgets you are interested in. This only needs to happen one time, so write this code in your constructor.



**Listing 8.21  Pulling out views in the constructor \(`CrimeListFragment.java`\)**

```
private class CrimeHolder extends RecyclerView.ViewHolder {

    
private TextView mTitleTextView;
private TextView mDateTextView;


    public CrimeHolder(LayoutInflater inflater, ViewGroup parent) {
        super(inflater.inflate(R.layout.list_item_crime, parent, false));

        
mTitleTextView = (TextView) itemView.findViewById(R.id.crime_title);
mDateTextView = (TextView) itemView.findViewById(R.id.crime_date);

    }
}

```

YourCrimeHolderwill also need abind\(Crime\)method. This will be called each time a newCrimeshould be displayed in yourCrimeHolder. First, addbind\(Crime\).



**Listing 8.22  Writing a`bind(Crime)`method \(`CrimeListFragment.java`\)**

```
private class CrimeHolder extends RecyclerView.ViewHolder {

    
private Crime mCrime;

    ...
    
public void bind(Crime crime) {
mCrime = crime;
mTitleTextView.setText(mCrime.getTitle());
mDateTextView.setText(mCrime.getDate().toString());
}

}

```

When given aCrime,CrimeHolderwill now update the titleTextViewand dateTextViewto reflect the state of theCrime.

Next, call your newly mintedbind\(Crime\)method each time theRecyclerViewrequests that a givenCrimeHolderbe bound to a particular crime.



**Listing 8.23  Calling the`bind(Crime)`method \(`CrimeListFragment.java`\)**

```
private class CrimeAdapter extends RecyclerView.Adapter
<
CrimeHolder
>
 {
    ...
    @Override
    public void onBindViewHolder(CrimeHolder holder, int position) {
        
Crime crime = mCrimes.get(position);
holder.bind(crime);

    }
    ...
}

```

Run CriminalIntent one more time, and every visibleCrimeHoldershould now display a distinctCrime\([Figure 8.11](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s04.html#fig_custom_crimes2)\).



**Figure 8.11  All right, all right, all right**

![](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ciRecyclerView/ci_with_custom_list_items.png "All right, all right, all right")

When you fling the view up, the scrolling animation should feel as smooth as warm butter. This effect is a direct result of keepingonBindViewHolder\(…\)small and efficient, doing only the minimum amount of work necessary.

Take heed: Always be efficient in youronBindViewHolder\(…\). Otherwise, your scroll animation could feel as chunky as cold Parmesan cheese.

