# RecyclerView ClickHandler -- Add functionality for a detail view in the Sleep Tracker App.
- Made with Google Code Labs!

#### ClickHandler
- Receiving clicks and handling them is a two-part task: First, you need to listen to and receive the click and determine which item has been clicked. Then, you need to respond to the click with an action.

- The best pace to get information about one clicked item is in the ViewHolder object, since it represents one list item. 

- While the ViewHolder is a great place to listen for clicks, it's not usually the right place to handle them. So, what is the best place for handling the clicks?

The Adapter displays data items in views, so you could handle clicks in the adapter. However, the adapter's job is to adapt data for display, not deal with app logic.
You should usually handle clicks in the ViewModel, because the ViewModel has access to the data and logic for determining what needs to happen in response to the click.

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
Back


