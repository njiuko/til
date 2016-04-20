#How to get Activity instance inside Test.

###Declare Field

~~~ java
private Activity currentActivity;
~~~

###Get the activity at any point in the Test

~~~ java
getActivityInstance();
~~~

###Declare Private Method

~~~ java
public Activity getActivityInstance(){
        getInstrumentation().runOnMainSync(new Runnable() {
            public void run() {
                Collection resumedActivities = ActivityLifecycleMonitorRegistry.getInstance().getActivitiesInStage(RESUMED);
                if (resumedActivities.iterator().hasNext()){
                    currentActivity = (Activity) resumedActivities.iterator().next();
                }
            }
        });

        return currentActivity;
}
~~~

##Use Case

It is useful for modifying views on the main thread while testing:

~~~ java
private void setPin(final String pin) {
        currentActivity.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                ((PinEntryView)currentActivity.findViewById(R.id.pin_view)).setText(pin);
            }
        });
    }
~~~
