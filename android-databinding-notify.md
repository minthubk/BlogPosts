title: Android databinding and notifying changes
tags: android,android-databinding

We've gone through how to setup and do various things with databinding.

However, we've not specified how we can update our POJO from another thread, and have the UI thread update itself based on the new values. 

(Note: Apparrently you may want to do the updates from the UI thread, especially when dealing with lists)

For this we need to tell the databinding that something has changed on each setter method. For instance:

    public void setTitle(String title) {
      this.title = title;
      notifyPropertyChanged(com.newfivefour.example.BR.title);
    }

The `BR` class here is generated by the databinding system so you can access the attribute name.

But for that `BR` class to generate its values you need a `@Bindable` annotation on the getter:

    @Bindable
    public String getTitle() {
      return title;
    }

You must also make the class extend `BaseObservable`.

If you don't want to extend you class, you can implement a particular data binding class instead. 

You can even use an `ObservableField` generic type to avoid the whole extending or implementing thing. But, apparently, this is mainly for quick implementations, not production.
