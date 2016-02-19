Overcoming INSTALL_FAILED_CONFLICTING_PROVIDER

http://gradlewhy.ghost.io/overcoming-install-failed-conflicting-provider/

Check this link, very useful. I will explain later.

I will explain also the whole process of having two builds (release/debug)

Also I had to change the app name in the manifest and the authorities attribute in the content provider

And for our project I also had to change the constant AUTHORITY

from

    public static final String AUTHORITY = "com.njiuko.dbb.providers.FavoriteContentProvider";

to

    public static final String AUTHORITY = BuildConfig.DEBUG ? "com.njiuko.dbb.providers.debug.FavoriteContentProvider" : "com.njiuko.dbb.providers.FavoriteContentProvider";
