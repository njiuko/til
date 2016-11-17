#Properly hide release signature data

* Keystore:
	1. Create keystore like in https://developer.android.com/studio/publish/app-signing.html
	2. Add keystore to project release build like stated there
  3. Put keystore in project **app/** folder

##Add **keystore.properties**. file in root directory

~~~ java
keyAlias=AliasName
keyPassword=KeyPassword
storeFile=keystore.jks
storePassword=StorePassword
~~~

##Add to **app#build.gradle**.

~~~ java
// Create a variable called keystorePropertiesFile, and initialize it to your
// keystore.properties file, in the rootProject folder.
def keystorePropertiesFile = rootProject.file("keystore.properties")

// Initialize a new Properties() object called keystoreProperties.
def keystoreProperties = new Properties()

// Load your keystore.properties file into the keystoreProperties object.
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

android {
    **signingConfigs {
        config {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }**
    compileSdkVersion 25
    buildToolsVersion "24.0.2"
    defaultConfig {
        applicationId "com.easyinvoice.app"
        minSdkVersion 19
        targetSdkVersion 25
        versionCode rootProject.ext.varVersionCode
        versionName "$rootProject.ext.varVersionName"
        manifestPlaceholders = [adventInternationalApplicationLabel: "@string/app_name"]
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            **signingConfig signingConfigs.config**
        }
        debug {
            versionNameSuffix ".debug"
            applicationIdSuffix ".debug." + "$rootProject.ext.varVersionNameLetters"
            manifestPlaceholders = [adventInternationalApplicationLabel: "Advent International" + "$rootProject.ext.varVersionName"]
            testCoverageEnabled = true
        }
    }
    dataBinding {
        enabled = true
    }
}
~~~


##Add to **.gitignore**

~~~ java
keystore.properties
app/keystore.jks
~~~
