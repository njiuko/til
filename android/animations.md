#How to build Animations.

First thing to check is that if we want to do an animation at the beginning and that depends on any view attribute, we should call OnGlobalLayoutListener to see when we can get the view details.

~~~ java
final LinearLayout llDayHeader = binding.llDayHeader;
llDayHeader.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
    @Override
    public void onGlobalLayout() {
        llDayHeader.animate().translationYBy(-llDayHeader.getHeight());
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
            llDayHeader.getViewTreeObserver().removeOnGlobalLayoutListener(this);
        } else {
            llDayHeader.getViewTreeObserver().removeGlobalOnLayoutListener(this);
        }
    }
});
~~~

The code for the animation is quite straight-forward, we just get the view and call animate(), where we can set some parameters like translationYBy, to move in the Y direction and say how much, also we could set the duration in milliseconds, and other things..

~~~ java
View llDayHeader = binding.llDayHeader;
llDayHeader.animate().translationYBy(llDayHeader.getHeight()).setDuration(200);
~~~
