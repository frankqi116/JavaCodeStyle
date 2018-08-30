Fatal Exception: java.lang.IllegalArgumentException: You cannot start a load for a destroyed activity

...

com.bumptech.glide.Glide.with(Glide.java:653)

...

What will be effect of this on performance of application and memory?

Glide provides so many .with() methods for a reason: it follows lifecycle.

Imagine a Fragment that is dynamically added to an Activity. In its onCreateView method it starts a Glide load of a 3MB image. Now, what if the user presses the back button and the Fragment is removed or the whole activity is closed?

If you use with(getActivity().getApplicationContext()) nothing will happen, all 3MBs of data is downloaded and then decoded, cached, probably even set to the ImageView, which is then garbage collected, because the only reference to it was from Glide internals.

If you use with((Fragment)this) Glide subscribes to the Fragment's lifecycle events and as soon as the Fragment is stopped, the any outstanding request should be paused; and when destroyed, all pending requests be cleared. This means that the image download will stop midway and no more resources will be used by that dead Fragment.

If you use with(getActivity()) Glide subscribes to the Activity's lifecycle events and the same thing happens as above, but only when the Activity is stopped or destroyed.

*** So the best practice is to use the closest possible context/fragment to avoid unused request completions! (There's also a manual way to stop a load: Glide.clear(ImageView|Target).)

To apply this in practice try to use with(this) when possible, but when it's not, like in an adapter, or a centralized image loading method, pass in a RequestManager glide as an argument and use glide.load(..., for example:

	static loadImage(RequestManager glide, String url, ImageView view) {
		glide.load(url).into(view);
	}
  
or in adapter:

	class MyAdapter extends WhichEveryOneYouUse {
	  private final RequestManager glide;
	  MyAdapter(RequestManager glide, ...) {
		  this.glide = glide;
		  ...
	  }
	  void getView/onBindViewHolder(... int position) {
		  // ... holder magic, and get current item for position
		  glide.load... or even loadImage(glide, item.url, holder.image);
	  }
	}
  
and use these from Activity/Fragment:

  loadImage(Glide.with(this), url, findViewById(R.id.image));

// or

  list.setAdapter(new MyAdapter(Glide.with(this), data));


thanks to this

https://stackoverflow.com/questions/31964737/glide-image-loading-with-application-context/32887693#32887693
