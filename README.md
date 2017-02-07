#RecyclerView Demo

First of all, we need to import the RecyclerView from the support library.
```
compile 'com.android.support:recyclerview-v7:+'
```
This will go into your gradle build file under dependencies. You should already see other things being compiled, so just put it after those.

Next, we set up the layout file, so go ahead and put this in your MainActivity layout:
```
<android.support.v7.widget.RecyclerView
        android:id="@+id/recycle_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
```
Here comes the hard part. RecyclerView has 3 components; the adapter, the layoutmanager, and the viewholder.
The adapter is simple enough as we've worked with ListViews before, but unlike ListViews, there are no built-in adapters for you to use. Here's what mine looks like.
```
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {

    private String[] mDataset;

    public MyAdapter(String[] myDataset) {
        mDataset = myDataset;
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        // create a new view
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.my_list_item, parent, false);
        ViewHolder vh = new ViewHolder(v);
        return vh;
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        final String text = mDataset[position];
        holder.textview.setText(text);
        holder.button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(v.getContext(), text, Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    public int getItemCount() {
        return mDataset.length;
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        // each data item is just a string in this case
        public TextView textview;
        public Button button;
        public ViewHolder(View v) {
            super(v);
            textview = (TextView) v.findViewById(R.id.textview);
            button = (Button) v.findViewById(R.id.button);
        }
    }
}
```
###Let's break it down!
In the class declaration, you can see I extend the RecyclerView Adapter. This will provide the 3 basic functions we need to make this adapter work. You will also see that it takes a generic object MyAdapter.ViewHolder. This is another component that we will cover later, but just keep in mind that the Adapter needs to know what ViewHolder you are using.
```
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
```
You will see that we have a private field holding our dataset. As well as a constructor to populate that field. 
```
private String[] mDataset;
public MyAdapter(String[] myDataset) {
        mDataset = myDataset;
    }
```
The following are the three functions from RecyclerView.Adapter that you will be overriding. By default, onCreateViewHolder will return null, onBindViewHolder will do nothing, and getItemCount will return 0.
```
	@Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        // create a new view
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.my_list_item, parent, false);
        ViewHolder vh = new ViewHolder(v);
        return vh;
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        final String text = mDataset[position];
        holder.textview.setText(text);
        holder.button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(v.getContext(), text, Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    public int getItemCount() {
        return mDataset.length;
    }
```
By now, you should be familiar with the view inflating as we covered this in Fragments. In the onCreateViewHolder, we do exactly this. We will inflate our list_item layout, which consists of one Button and one TextView. This will then be passed into the ViewHolder and then returned. This function is basically to set up the view and OnBindViewHolder will handle the populating of information. OnBindViewHolder will only be called once the view is attached to the UI and is about to be displayed. getItemCount is pretty self explanatory, just remember to return the amount of things you intend to show.

We didn't cover this much in class, but you can always create a custom list_item but creating a separate layout file and specifying what you what in it. Think fragments, but simpler. Here is my layout file for one list item:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp"
    android:orientation="horizontal">

    <TextView
        android:id="@+id/textview"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="hello" />

    <Button
        android:id="@+id/button"
        android:text="push me"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
</LinearLayout>
```
###ViewHolder
```
 public static class ViewHolder extends RecyclerView.ViewHolder {
        // each data item is just a string in this case
        public TextView textview;
        public Button button;
        public ViewHolder(View v) {
            super(v);
            textview = (TextView) v.findViewById(R.id.textview);
            button = (Button) v.findViewById(R.id.button);
        }
    }
```
And now we arrive at the ViewHolder. A ViewHolder is just a nice way to manage your UI elements. In this case, we retrieve the TextView and Button from the view we inflated earlier in OnCreateViewHolder. Notice this is a static class. What this means is that Android won't have to keep reinflating or reloading the view every time the item is called up.

###Adding It to Our Activity
mRecyclerView = (RecyclerView) findViewById(R.id.recycleView);
```
RecyclerView mRecyclerView;
RecyclerView.Adapter mAdapter;
RecyclerView.LayoutManager mLayoutManager;

mRecyclerView = (RecyclerView) findViewById(R.id.recycle_view);

mLayoutManager = new LinearLayoutManager(this);
mRecyclerView.setLayoutManager(mLayoutManager);

mAdapter = new MyAdapter(myDataset);
mRecyclerView.setAdapter(mAdapter);
```
Here it is, the thing that makes it all work. Like always, we grab the reference to the RecycleView in our layout file. Next, we set the LayoutManager. RecycleView has a few like GridLayoutManager and LinearLayoutManager. This will determine how it will be displayed. LinearLayoutManager will give us something like a ListView. Now, we set up our custom Adapter and pass in our dataset, in this case it is a String array.

Check out my code, play around with it, make sure you get it. If you can understand this and you didn't quite understand how ListViews worked, hopefully this gave you a good idea on how they both work!
