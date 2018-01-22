# Launcher

Steps
-----

1. Complete the manifest :

```groovy
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.HOME" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

2. Create CustomView class

```groovy
import android.content.Context;
import android.util.Log;
import android.view.MotionEvent;
import android.view.ViewGroup;

public class CustomViewGroup extends ViewGroup {
    public CustomViewGroup(Context context) {
        super(context);
    }
    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
    }
    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        Log.v("customViewGroup", "**********Intercepted");
        return true;
    }
}
```

3. Modify the MainActivity.java

```groovy
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        WindowManager manager = ((WindowManager) getApplicationContext().getSystemService(Context.WINDOW_SERVICE));
        WindowManager.LayoutParams localLayoutParams = new WindowManager.LayoutParams();
        localLayoutParams.type = WindowManager.LayoutParams.TYPE_SYSTEM_ERROR;
        localLayoutParams.gravity = Gravity.TOP;
        localLayoutParams.flags = WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE|
// this is to enable the notification to recieve touch events
                WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL |
// Draws over status bar
                WindowManager.LayoutParams.FLAG_LAYOUT_IN_SCREEN;
        localLayoutParams.width = WindowManager.LayoutParams.MATCH_PARENT;
        localLayoutParams.height = (int) (50 * getResources().getDisplayMetrics().scaledDensity);
        localLayoutParams.format = PixelFormat.TRANSPARENT;
        CustomViewGroup view = new CustomViewGroup(this);
        manager.addView(view, localLayoutParams);
    }
```

4. Change style for full screen (in styles.xml)

```groovy
<resources>

    <style name="Theme.AppCompat.Light.NoActionBar.FullScreen" parent="@style/Theme.AppCompat.Light">
        <item name="android:windowNoTitle">true</item>
        <item name="android:windowActionBar">false</item>
        <item name="android:windowFullscreen">true</item>
        <item name="android:windowContentOverlay">@null</item>
    </style>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar.FullScreen">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

</resources>
```

