
![ekyc-logo](https://user-images.githubusercontent.com/81238558/175898689-22d5a275-45ec-4a5e-89c7-988dbbb073b0.png)

<h1>EkycID SDK for Android</h1><br>

![](https://img.shields.io/badge/platform-android-blue) ![](https://img.shields.io/github/v/tag/EKYCSolutions/ekyc-id-android?label=version)


The EkycID Android SDK lets you build a fantastic OCR and Face Recognition experienced in your Android app.

With one quick scan, your users will be able to extract information from thier identity cards, passports, driver licenses, license plate, vehicle registration, covid-19 vaccinate card, and any other document by government-issued.


EkycID is:
* Easy to integrate into existing ecosystems and solutions through the use of SDKs that supported both native and hybrid applications.
* Better for user experience being that the document detections and liveness checks are done directly offline on the device.
* Great for cutting down operations cost and increasing efficiency by decreasing reliance on human labor and time needed for manual data entry. 


EkycID can:
* Extract information from identity documents through document recognition & OCR.
* Verify whether an individual is real or fake through liveness detection, and face recognition. 
* Verify the authenticity of the identity documents by combining the power of document detection, OCR, liveness detection, and face recognition. 


To see all of these features at work download our free demo app:

<a href='https://play.google.com/store/apps/details?id=com.ekyc.demo_app&pcampaignid=pcampaignidMKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1'><img alt='Get it on Google Play' src='https://play.google.com/intl/en_us/badges/static/images/badges/en_badge_web_generic.png' width="300" 
     height="100"/>
</a>

# 1. Requirements
- minSdkVersion: 21
- targetSdkVersion: 33
- compileSdkVersion: 33

# 2. Installation
**Step 1:** Add the JitPack repository to your root build.gradle at the end of repositories.

```gradle
allprojects {
     repositories {
          ...
          maven { url 'https://jitpack.io' }
     }
}
```
**Step 2:** Add the dependency.
```gradle
dependencies {
     implementation 'com.github.EKYCSolutions:ekyc-id-android:1.0.21'
}
```

# 3. Usage

## 3.1. Document Scanner

**Step 1:** Initialize `Initializer` class and called `initializer.start()` in onResume.
```kotlin
class MainActivity : AppCompatActivity() {
    val initializer = Initializer(this)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    @RequiresApi(Build.VERSION_CODES.N)
    override fun onResume() {
        super.onResume()
        
        // Start initializer here
        initializer.start {
            
        }
    }
}
```

**Step 2:** Add DocumentScannerCameraView to your `layout.xml`
```xml

<com.ekycsolutions.ekycid.documentscanner.DocumentScannerCameraView
      android:id="@+id/documentScannerCameraView
      android:layout_width="match_parent"
      android:layout_height="match_parent" />

```

**Step 3:** Initialize `DocumentScannerCameraView` and implements `DocumentScannerEventListener`.
```kotlin
class MainActivity : AppCompatActivity(), DocumentScannerEventListener {
    val initializer = Initializer(this)

    lateinit var documentScannerView: DocumentScannerCameraView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    @RequiresApi(Build.VERSION_CODES.N)
    override fun onResume() {
        super.onResume()
        initializer.start {
            runOnUiThread {
                setContentView(R.layout.activity_main)
                documentScannerView = findViewById(R.id.documentScannerView)
                documentScannerView.addListener(this)
                documentScannerView.setWhiteList(arrayListOf(ObjectDetectionObjectType.NATIONAL_ID_0.name))
                documentScannerView.start()
            }
        }
    }

    override fun onInitialize() {
        Log.d(TAG, "onInitialize")
    }

    override fun onFrame(frameStatus: FrameStatus) {
        if (frameStatus != FrameStatus.PROCESSING) {
            Log.d(TAG, "onFrame: $frameStatus")
        }
    }

    override fun onDetection(detection: DocumentScannerResult) {
        Log.d(TAG, "onDetection")
    }
}
```

## 3.2. Liveness Detection

**Step 1:** Initialize `Initializer` class and called `initializer.start()` in onResume.
```kotlin
class MainActivity : AppCompatActivity() {
    val initializer = Initializer(this)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    @RequiresApi(Build.VERSION_CODES.N)
    override fun onResume() {
        super.onResume()
        
        // Start initializer here
        initializer.start {
            
        }
    }
}
```

**Step 2:** Add DocumentScannerCameraView to your `layout.xml`
```xml

<com.ekycsolutions.ekycid.livenessDetection.LivenessDetectionCameraView
      android:id="@+id/livenessDetectionCameraView
      android:layout_width="match_parent"
      android:layout_height="match_parent" />

```

**Step 3:** Initialize `LivenessDetectionCameraView` and implements `LivenessDetectionEventListener`.
```kotlin
class MainActivity : AppCompatActivity(), LivenessDetectionEventListener {
    val initializer = Initializer(this)

    lateinit var livenessDetectionCameraView: LivenessDetectionCameraView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    @RequiresApi(Build.VERSION_CODES.N)
    override fun onResume() {
        super.onResume()
        initializer.start {
            runOnUiThread {
                setContentView(R.layout.activity_main)
                livenessDetectionCameraView = findViewById(R.id.documentScannerView)
                livenessDetectionCameraView.addListener(this)
                livenessDetectionCameraView.setOptions(
                    LivenessDetectionOptions(
                         arrayListOf(LivenessPrompt.LOOK_LEFT, LivenessPrompt.LOOK_RIGHT, LivenessPrompt.BLINKING)
                    )
                )
                livenessDetectionCameraView.start()
            }
        }
    }

    override fun onInitialize() {
        Log.d(TAG, "onInitialize")
    }

    override fun onFrame(frameStatus: FrameStatus) {
        if (frameStatus != FrameStatus.PROCESSING) {
            Log.d(TAG, "onFrame: $frameStatus")
        }
    }

    override fun onPromptCompleted(completedPromptIndex: Int, success: Boolean: progress: Float) {
        Log.d(TAG, "onPromptCompleted")
    }
    
    override fun onAllPromptsCompleted(detection: LivenessDetectionResult) {
        Log.d(TAG, "onAllPromptsCompleted")
    }
    
    override fun onFocus() {
        Log.d(TAG, "onFocus")
    }
    
    override fun onFocusDropped() {
        Log.d(TAG, "onFocusDropped")
    }
    
    override fun onCountDownChanged(current: Int, max: Int) {
        Log.d(TAG, "onCountDownChanged")
    }
}
```

## 3.3. Perform Face Compare and OCR

To perform face compare and ocr, you can call your server that integrated with our [NodeSDK]() to get face compare score and ocr response respectively.

## 3.4 Blur Detection 

This section outlines the Canny Edge Detection algorithm that is used to detect bluriness in scanning the documents. 

The Canny Edge Detection algorithm is a popular method for detecting edges in digital images.
An edge is a boundary or transition region between two different regions or objects in an image. It represents a significant change in pixel intensity or color values.
Less pronounced or fewer detected edges in an image can indicate blurriness or a lack of sharp transitions between objects or regions, hence why this algorithm was chosen.

Given an image, here are the steps performed on it to determine blurriness: 

Original Image: 
![Original Image](https://github.com/EKYCSolutions/ekyc-id-android/assets/86823472/1a35c5f2-e001-4620-bc7f-b8ac45cc4f34)


1. Warp the image using the perspective transformation matrix to obtain a cropped and transformed image.

Warped Image:
![warped](https://github.com/EKYCSolutions/ekyc-id-android/assets/86823472/0c025544-3b26-4bff-9996-884fa169a9c3)


2. Convert the warped image to grayscale.
3. Apply Gaussian blur to the grayscale image.

Grayscale Image:
![grey](https://github.com/EKYCSolutions/ekyc-id-android/assets/86823472/0405e3b2-f619-41e6-a4d5-a0d1d2a5e012)


   
4. Perform Canny Edge Detection on the original image. This will produce an output which we will call the edge image.
   
Edge Image:
![edges](https://github.com/EKYCSolutions/ekyc-id-android/assets/86823472/c4463939-417c-42db-91ae-4380226a6af3)


5. Calculate the height and width of the edge image.
6. Count the number of white pixels in the edge image. 
7. Calculate the ratio of white pixels to the total number of pixels of the image ( width * height )
8. If the calculated ratio is less than or equal to the set ratio, the image is determined to be blurry.

# 4. Contact
<p>For any other questions, feel free to contact us at 
  <a href="https://ekycsolutions.com/">ekycsolutions.com</a>
</p>

# 5. License
© 2022 EKYC Solutions Co, Ltd. All rights reserved.
