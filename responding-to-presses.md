As icing on theRecyclerViewcake, CriminalIntent should also respond to a press on these list items. In[Chapter 10](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch10.html), you will launch the detail view for aCrimewhen the user presses on thatCrimein the list. For now, show aToastwhen the user takes action on aCrime.

As you may have noticed,RecyclerView, while powerful and capable, has precious few real responsibilities. \(May it be an example to us all.\) The same goes here: Handling touch events is mostly up to you. If you need them,RecyclerViewcan forward along raw touch events. But most of the time this is not necessary.

Instead, you can handle them like you normally do: by setting anOnClickListener. Since eachViewhas an associatedViewHolder, you can make yourViewHoldertheOnClickListenerfor itsView.

Modify theCrimeHolderto handle presses for the entire row.



**Listing 8.24  Detecting presses in`CrimeHolder`\(`CrimeListFragment.java`\)**

```
private class CrimeHolder extends RecyclerView.ViewHolder
        
implements View.OnClickListener
 {
    ...
    public CrimeHolder(LayoutInflater inflater, ViewGroup parent) {
        super(inflater.inflate(R.layout.list_item_crime, parent, false));
        
itemView.setOnClickListener(this);

        ...
    }
    ...
    
@Override
public void onClick(View view) {
Toast.makeText(getActivity(),
mCrime.getTitle() + " clicked!", Toast.LENGTH_SHORT)
.show();
}

}

```

In[Listing 8.24](https://www.safaribooksonline.com/library/view/android-programming-the/9780134706061/ch08s05.html#pl_crime_list_fragment_crimeholder_on_click), theCrimeHolderitself is implementing theOnClickListenerinterface. On theitemView, which is theViewfor the entire row, theCrimeHolderis set as the receiver of click events.

Run CriminalIntent and press on an item in the list. You should see aToastindicating that the item was clicked.

