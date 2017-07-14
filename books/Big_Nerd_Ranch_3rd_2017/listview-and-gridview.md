The core Android OS includesListView,GridView, andAdapterclasses. Until the release of Android 5.0, these were the preferred ways to create lists or grids of items.

The API for these components is very similar to that of aRecyclerView. TheListVieworGridViewclass is responsible for scrolling a collection of items, but it does not know much about each of those items. TheAdapteris responsible for creating each of theViews in the list. However,ListViewandGridViewdo not enforce that you use theViewHolderpattern \(though you can – and should – use it\).

These old implementations are replaced by theRecyclerViewimplementation because of the complexity required to alter the behavior of aListVieworGridView.

Creating a horizontally scrollingListView, for example, is not included in theListViewAPI and requires a lot of work. Creating custom layout and scrolling behavior with aRecyclerViewis still a lot of work, butRecyclerViewwas built to be extended, so it is not quite so bad.

Another key feature ofRecyclerViewis the animation of items in the list. Animating the addition or removal of items in aListVieworGridViewis a complex and error-prone task.RecyclerViewmakes this much easier, includes a few built-in animations, and allows for easy customization of these animations.

For example, if you found out that the crime at position 0 moved to position 5, you could animate that change like so:

```
mRecyclerView.getAdapter().notifyItemMoved(0, 5);
```



