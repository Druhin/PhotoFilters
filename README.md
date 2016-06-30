

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
