<p align="center">
  <img src="https://camo.githubusercontent.com/4c4c8d90e242619972a11baa3c33acaaeb9bad00/687474703a2f2f692e696d6775722e636f6d2f666748314852742e706e67" />
</p>
<p align="center">
  <a href="http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels">
    <img src="https://img.shields.io/badge/MIN_SDK-15-B8D529.svg?style=flat" alt="Twitter">
    <img src="https://img.shields.io/badge/BUILD_SDK-23-92D230.svg?style=flat" alt="Twitter">
  </a>
  <a href="https://www.photoeditorsdk.com/documentation/android/getting-started">
    <img src="https://img.shields.io/badge/platform-android-2DC25C.svg?style=flat" alt="Twitter">
  </a>
  <a href="https://bintray.com/9elements/ly.img.android/photo-editor-sdk/_latestVersion">
    <img src="https://api.bintray.com/packages/9elements/ly.img.android/photo-editor-sdk/images/download.svg" alt="JCenter">
  </a>
  <a href="http://twitter.com/PhotoEditorSDK">
    <img src="https://img.shields.io/badge/twitter-@PhotoEditorSDK-8646E2.svg?style=flat" alt="Twitter">
  </a>
</p>
## img.ly SDK for Android

img.ly SDK for Android is for creating stunning images with a nice selection of premium filters.

<a href="https://play.google.com/store/apps/details?id=com.photoeditorsdk.android.app&utm_source=global_co&utm_medium=prtnr&utm_content=Mar2515&utm_campaign=PartBadge&pcampaignid=MKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1"><img height="60px" alt="Get it on Google Play" src="https://play.google.com/intl/en_us/badges/images/generic/en-play-badge-border.png" /></a>

### Overview

The img.ly Photo Editor SDK provides a variety of tools and functions for creating photo applications for Android. It is licensed under a proprietary license and intended to be used as better alternative for free software applications such as the [GPUImage](https://github.com/CyberAgent/android-gpuimage) or similiar libraries. If you are interested in our SDK, please [contact us](#author--contact).

#### SDK Core

__ACS Component__ <br/>
A generic Android Camera Stack library which is based on the [android.hardware.Camera](http://developer.android.com/reference/android/hardware/Camera.html) API. Supports front and rear cam, HDR, flash modes and much more.

__SDK Component__ <br/>
The img.ly core library for Android. Containing the OpenGL and toolset implementation.

__UI Component__ <br/>
The default UI components consisting of LivePreview and Editor Activity.

__PLUGIN Components__ <br/>
At the momment not implemented, it is for future use. For Example to embedding you Google Analytics.

#### Dependencies

No external non Google libraries needed or used by the SDK.

* com.android.support:recyclerview-v7:23.2.0'
* com.android.support:support-annotations:23.2.0'


### Features

* __Android API Level 15+__. Covers nearly 95% of all Android devices.
* __Default UI__ for camera preview and editing. Based on Intents and Activities.
* __Fast image export__. Even with large images or slow devices the export is done in adequate time with background processing.
* __Generic camera support__. Integrated and featureful on the most Android phones.
* __Crop__, __Rotate__, __Stickers__, __Text Placement__, and __Colorize__. All essential photo editing functions wrapped into a simple, beautiful and customizable UI.
* __57 Stunning filters__ which are builtin and work out of the box.
* __No native code__. Our backend is Renderscript based with highlevel OpenGL support, therefore we dodge all the nasty native library problems other frameworks face.
* __Tablet support__. Works great on tablets.
* __Photoshop LUT__. Design color filters in Photoshop!
With this feature it is possible to generate LUT (Look Up Table) color filters easily from different photo
editing tools. Export and integrate them in minutes!
* __Live Preview up to 21mpx__. Filters can be previewed in high quality realtime on full camera resolution.
* __Low memory footprint__ even with high resolution images.
* __Extensible and customizable toolset interface__. Add your own customized filters with [Renderscript](https://developer.android.com/guide/topics/renderscript/index.html) and modify tool properties yourself.

### License

img.ly SDK for Android is a licensed library which can be used for different purposes. <br/>
Please see:

 [LICENSE.PROPIETARY](https://github.com/imgly/imgly-sdk-android/blob/master/LICENSE.PROPIETARY) for PROPIETARY usage.

### Author & Contact

&copy; 9elements GmbH <br/>
[Email](mailto:eray.basar@9elements.com) <br/>
[Homepage](http://www.9elements.com) <br/>
[Follow us on Twitter](https://twitter.com/9elements)

## Installation

> Require a minimum deployment target of Android API 15 (4.0.4) and Device with HardwareLayer (for LivePreview) and LargeHeap Support (To operate and export large images)


##### 1. Configure your Module build.gradle to import the img.ly SDK into your project with jCenter.

There are few things we'll need to add here.
See comments in the example code below. 

__DO NOT FORGET TO ADD RENDERSCRIPT SUPPORT!__

```groovy
apply plugin: 'com.android.application'

android {
    /* Set the Compile SDK and the Build SDK min. at SDK 23 or grater. */
    compileSdkVersion 23
    buildToolsVersion '23.0.2'

    /* If you update from SDK 22 and below, the ApacheHttp-Library are removed by Google,
     * if you need the ApacheHttp-Library comment out the next this line */
    // useLibrary 'org.apache.http.legacy'

    defaultConfig {
        /* Replace with your App-ID @see http://tools.android.com/tech-docs/new-build-system/applicationid-vs-packagename */
        applicationId "my.domain.application"

        /* Set the minimum supported SDK Version to 15 (Android 4.0.3) or higher */
        minSdkVersion 15

        /* Set the target SDK Version at minimum to 23 or higher */
        targetSdkVersion 23

        /* Set your own Version Code and Version name */
        versionCode 1
        versionName "1.0"

        /* Don't forget this two lines, apart from that the app will crash! */
        renderscriptTargetApi 20
        renderscriptSupportModeEnabled true
    }

    /* Set Java Language level minimal to Java 1.7 or greater */
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_7
        targetCompatibility JavaVersion.VERSION_1_7
    }

    ...
}

dependencies {
    /* Make sure you are import the latest SDK version */
    compile 'ly.img.android:photo-editor-sdk:1.0.8'
}

...

```

Be also sure to sync your project with the Gradle files after making any edits.

For more information about gradle look at http://developer.android.com/tools/building/configuring-gradle.html

##### 2. Initialize SDK in an Application class.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.mymodule.app" >

    <application
        android:name=".Application"
        ... >

        ...
    </application>

</manifest>

```

```java
public class Application extends android.app.Application {
    @Override
    public void onCreate() {
        super.onCreate();

        ImgLySdk.init(this);
    }
}
```

##### 3.1. Start img.ly SDK default UI.

This is what your Activity should look like. Follow the steps below to understand the individual workflow:

```java
public class MainActivity extends Activity implements PermissionRequest.Response{

    public static int CAMERA_PREVIEW_RESULT = 1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        new CameraPreviewIntent(this)
                .setExportDir(CameraPreviewIntent.Directory.DCIM, "ImgLyExample")
                .setExportPrefix("example_")
                .setEditorIntent(
                        new PhotoEditorIntent(this)
                        .setExportDir(PhotoEditorIntent.Directory.DCIM, "ImgLyExample")
                        .setExportPrefix("result_")
                        .destroySourceAfterSave(true)
                )
                .startActivityForResult(CAMERA_PREVIEW_RESULT);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, android.content.Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK && requestCode == CAMERA_PREVIEW_RESULT) {
            String path = data.getStringExtra(CameraPreviewActivity.RESULT_IMAGE_PATH);

            Toast.makeText(this, "Image Save on: " + path, Toast.LENGTH_LONG).show();

        }
    }
    
    //Important for Android 6.0 permisstion request, don't forget this!
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        PermissionRequest.onRequestPermissionsResult(requestCode, permissions, grantResults);
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    }

    @Override
    public void permissionGranted() {

    }

    @Override
    public void permissionDenied() { 
        //The Permission whas rejected by the user, so the Editor was not opened because it can not save the result Image. 
        //TODO for you: Show a Hint to the User
    }
}
```

__Do not forget to delegate the onRequestPermissionsResult to PermissionRequest.onRequestPermissionsResult. Apart from that it will not work on Android 6.0 and above__

##### 3.1. Start Camera Preview Activity with editor backend.

```java
// Camera activity intent with customized editor access.
        new CameraPreviewIntent(this)
        .setExportPrefix("example_")
        .setEditorIntent(
            new PhotoEditorIntent(this)
            .setExportDir(PhotoEditorIntent.Directory.DCIM, "ImgLyExample")
            .setExportPrefix("result_")
            .destroySourceAfterSave(true)
        )
        .setExportDir(CameraPreviewIntent.Directory.DCIM, "ImgLyExample")
        .startActivityForResult(CAMERA_PREVIEW_RESULT);
```

##### 3.2. Start Editor Activity standalone.

```java
// Editor activity intent.
        new PhotoEditorIntent(this)
        .setSourceImagePath(imagePath)
        .setExportDir(PhotoEditorIntent.Directory.DCIM, "ImgLyExample")
        .setExportPrefix("result_")
        .destroySourceAfterSave(true)
        .startActivityForResult(CAMERA_PREVIEW_RESULT);
```

##### 4. Customize SDK tools for your own Android App.

```java
// Toolset modifications can be done easily inside the Application class.
public class Application extends android.app.Application {
    @Override
    public void onCreate() {
        super.onCreate();

        ImgLySdk.init(this);

        // Step1 get current configuration.
        ArrayList<CropAspectConfig> cropConfig = PhotoEditorSdkConfig.getCropConfig();

        // Step2 clear it.
        cropConfig.clear();

        // Step3 add the needed properties and filters for different tools.
        cropConfig.add(new CropAspectConfig(R.string.imgly_crop_name_custom, R.drawable.icon_crop_custom, -1));
        cropConfig.add(new CropAspectConfig(R.string.imgly_crop_name_4_3, R.drawable.icon_crop_4_3, 4/3));

        // Step4 done!

        // More examples:
        ArrayList<ColorFilter> colorFilterConfig = PhotoEditorSdkConfig.getFilterConfig();

        colorFilterConfig.clear();

        colorFilterConfig.add(new NoneColorFilter());
        colorFilterConfig.add(new ColorFilterBleached());
        colorFilterConfig.add(new ColorFilterChest());

        ArrayList<ImageStickerConfig> stickers = PhotoEditorSdkConfig.getStickerConfig();

        stickers.clear();

    }
}
```

## Troubleshooting

Some AndroidStudio decides to import `PermissionRequest.Response` from `android.webkit.PermissionRequest`. Make sure you import `ly.img.android.ui.utilities.PermissionRequest`.
