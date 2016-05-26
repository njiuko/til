#Animation Drawable

With an Animation Drawable we can achieve an Animation from many drawable files.

Check: https://developer.android.com/guide/topics/graphics/drawable-animation.html

* First Step: Download all images into the Drawable folder of the application.

* Second Step: Create an XML file for the AnimationDrawable. You can set the order of the drawables and the duration of each.
~~~ xml
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">
    <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
</animation-list>
~~~
The oneshot variable says the animation is only happening once. When we desire a loop, we should set it to false.

* Third Step: Create and Start the AnimationDrawable in the Activity/Fragment.
~~~ java
AnimationDrawable rocketAnimation;

public void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.main);

  ImageView rocketImage = (ImageView) findViewById(R.id.rocket_image);
  rocketImage.setBackgroundResource(R.drawable.rocket_thrust);
  rocketAnimation = (AnimationDrawable) rocketImage.getBackground();
}

public boolean onTouchEvent(MotionEvent event) {
  if (event.getAction() == MotionEvent.ACTION_DOWN) {
    rocketAnimation.start();
    return true;
  }
  return super.onTouchEvent(event);
}
~~~
In case we have set oneshot to false, we should remember to stop the animation at some point.

##Important
The start method of the animation should not be called in the onCreate of the Activity because the AnimationDrawable is not yet fully attached to the window. If you want to play the animation immediately, without requiring interaction, then you might call it from the onWindowFocusChanged() method in your Activity.

For the case of a Fragment is okay to call it in the onCreateView method.
