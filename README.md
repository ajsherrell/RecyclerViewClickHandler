# RecyclerView ClickHandler -- Add functionality for a detail view in the Sleep Tracker App.
- Made with Google Code Labs!
- ClickHandler
- RecyclerView Header

#### ClickHandler
- Receiving clicks and handling them is a two-part task: First, you need to listen to and receive the click and determine which item has been clicked. Then, you need to respond to the click with an action.

- The best pace to get information about one clicked item is in the ViewHolder object, since it represents one list item. 

- While the ViewHolder is a great place to listen for clicks, it's not usually the right place to handle them. So, what is the best place for handling the clicks?

The Adapter displays data items in views, so you could handle clicks in the adapter. However, the adapter's job is to adapt data for display, not deal with app logic.
You should usually handle clicks in the ViewModel, because the ViewModel has access to the data and logic for determining what needs to happen in response to the click.

### Headers - RecyclerView
- A list can have a single header to describe the item content. A list can also have multiple headers to group and separate items in a single list.
- RecyclerView doesn't know anything about your data or what type of layout each item has. The LayoutManager arranges the items on the screen, but the adapter adapts the data to be displayed and passes view holders to the RecyclerView. So you will add the code to create headers in the adapter.
- One way to add headers to a list is to modify your adapter to use a different ViewHolder by checking indexes where your header needs to be shown. The Adapter will be responsible for keeping track of the header. For example, to show a header at the top of the table, you need to return a different ViewHolder for the header while laying out the zero-indexed item. Then all the other items would be mapped with the header offset.
- Another way to add headers is to modify the backing dataset for your data grid. Since all the data that needs to be displayed is stored in a list, you can modify the list to include items to represent a header. This is a bit simpler to understand, but it requires you to think about how you design your objects, so you can combine the different item types into a single list. Implemented this way, the adapter will display the items passed to it. So the item at position 0 is a header, and the item at position 1 is a SleepNight, which maps directly to what's on the screen.
- Each methodology has benefits and drawbacks. Changing the dataset doesn't introduce much change to the rest of the adapter code, and you can add header logic by manipulating the list of data. On the other hand, using a different ViewHolder by checking indexes for headers gives more freedom on the layout of the header. It also lets the adapter handle how data is adapted to the view without modifying the backing data.
- In this case, your app will use a different ViewHolder for the header than for data items. The app will check the index of the list to determine which ViewHolder to use.

#### Summary
- To make items in a RecyclerView respond to clicks, attach click listeners to list items in the ViewHolder, and handle clicks in the ViewModel.

To make items in a RecyclerView respond to clicks, you need to do the following:

Create a listener class that takes a lambda and assigns it to an onClick() function.
class SleepNightListener(val clickListener: (sleepId: Long) -> Unit) {
   fun onClick(night: SleepNight) = clickListener(night.nightId)
}
Set the click listener on the view.
android:onClick="@{() -> clickListener.onClick(sleep)}"
Pass the click listener to the adapter constructor, into the view holder, and add it to the binding object.
class SleepNightAdapter(val clickListener: SleepNightListener):
       ListAdapter<DataItem, RecyclerView.ViewHolder>(SleepNightDiffCallback()
holder.bind(getItem(position)!!, clickListener)
binding.clickListener = clickListener
In the fragment that shows the recycler view, where you create the adapter, define a click listener by passing a lambda to the adapter.
val adapter = SleepNightAdapter(SleepNightListener { nightId ->
      sleepTrackerViewModel.onSleepNightClicked(nightId)
})
Implement the click handler in the view model. For clicks on list items, this commonly triggers navigation to a detail fragment.

- Headers:
- A header is generally an item that spans the width of a list and acts as a title or separator. A list can have a single header to describe the item content, or multiple headers to group items and separate items from each other.
A RecyclerView can use multiple view holders to accommodate a heterogeneous set of items; for example, headers and list items.
One way to add headers is to modify your adapter to use a different ViewHolder by checking indexes where your header needs to be shown. The Adapter is responsible for keeping track of the header.
Another way to add headers is to modify the backing dataset (the list) for your data grid, which is what you did in this codelab.
- These are the major steps for adding a header:

Abstract the data in your list by creating a DataItem that can hold a header or data.
Create a view holder with a layout for the header in the adapter.
Update the adapter and its methods to use any kind of RecyclerView.ViewHolder.
In onCreateViewHolder(), return the correct type of view holder for the data item.
Update SleepNightDiffCallback to work with the DataItem class.
Create a addHeaderAndSubmitList() function that uses coroutines to add the header to the dataset and then calls submitList().
Implement GridLayoutManager.SpanSizeLookup() to make only the header three spans wide.


