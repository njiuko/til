#Different Build Types with Gradle


##app#build.gradle

###Declare Constants

~~~android
def varVersionName = "1.1.1"
def varVersionNameLetters = "one.one.one"
def varVersionCode = 26
~~~

then inside the **android{}** tag:

For the **default configuration**:

~~~android
defaultConfig {
        applicationId "de.example.app"
        minSdkVersion 14
        targetSdkVersion 23
        versionCode varVersionCode
        versionName "${varVersionName}"
        manifestPlaceholders = [nameApplicationLabel: "@string/app_name"]
    }
~~~

Then we play with our different **build Types**:

~~~android
buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            signingConfig signingConfigs.appReleaseKey
            resValue "string", "content_provider", "com.njiuko.dbb.providers.FavoriteContentProvider"
        }
        debug {
        	versionNameSuffix ".debug"
            applicationIdSuffix ".debug." + "${varVersionNameLetters}"
            manifestPlaceholders = [nameApplicationLabel: "AppName" + "${varVersionName}"]
            resValue "string", "content_provider", "com.njiuko.dbb.providers.debug.FavoriteContentProvider"
        }
}
~~~

##AndroidManifest.xml

Inside the **application** tag:

~~~android
tools:replace="android:label"
android:label="${nameApplicationLabel}"
~~~

##Build Variants

In **Android Studio** in the left side we can see the **Build Variants** window where we can select the build type we want or if we have flavours, then the flavour also.

##Results

###Build Variant Release:

Our app will be created with the following:


* Application ID:
	1. 
* App name: 
	1. Will go to the **AndroidManifest.xml** 
	2. For the label it will have to check the value of the variable **nameApplicationLabel** 
	3. Which in this case comes from the **defaultConfig**
		* Result: "@string/app_name"


