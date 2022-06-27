
![image](https://user-images.githubusercontent.com/81238558/175767662-be4dc9ba-a6bd-459d-aaa3-f8ad0c96aa37.png)

<h1>EkycID SDK for Android</h1><br>

![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/EKYCSolutions/ekyc-id-android)


The EkycID Android SDK lets you build a factastic OCR and Face Recognition experienced in your Android app.

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
<br></br>

<h1>Quick Start</h1>
Let's start the quick start application within Android Studio.

<h2>SDK Integration</h2>
<h4>Adding EkycID dependency</h4>
<p>In your <code>build.gradle</code>, add EkycID as a dependency</p>

<pre class="notranslate">
<code>dependencies {
    implementation project(path: ':ekyc-id')
}
</code>
</pre>

<h4>Import JavaDoc</h4>
<p dir="auto">Android studio 3.0 should automatically import javadoc from dependency. If that doesn't happen, you can do that manually by following these steps:</p>
<ol dir="auto">
<li>In Android Studio project sidebar, ensure <a href="https://developer.android.com/sdk/installing/studio-androidview.html" rel="nofollow">project view is enabled</a></li>
<li>Expand <code>External Libraries</code> entry (usually this is the last entry in project view)</li>
<li>Locate <code>ekyc-id-android</code> entry, right click on it and select <code>Library Properties...</code></li>
<li>A <code>Library Properties</code> pop-up window will appear</li>
<li>Click the second <code>+</code> button in bottom left corner of the window (the one that contains <code>+</code> with little globe)</li>
<li>Window for defining documentation URL will appear</li>
<li>Enter following address: <code>https://github.com/EKYCSolutions/ekyc-id-android/</code></li>
<li>Click <code>OK</code></li>
</ol>

<h4>Performing your first scan</h4>
<ol dir="auto">
<li>
<p>In your main activity, EventListener suppose to be automatically import. If that doesn't happen, you can do that manually:</p>
<pre class="notranslate">
import com.ekycsolutions.ekycid.models.FrameStatus
import com.ekycsolutions.ekycid.documentscanner.DocumentScannerResult
import com.ekycsolutions.ekycid.documentscanner.DocumentScannerCameraView
import com.ekycsolutions.ekycid.documentscanner.DocumentScannerEventListener
import com.ekycsolutions.ekycid.livenessdetection.*
</code>
</pre>
</li>

<li>
<p>In your main activity, define event variable:</p>
<pre>
<code>class MainActivity : AppCompatActivity(), LivenessDetectionEventListener {
  val initializer = Initializer(this)
  <br></br>
  //define ekycid camera view event
  lateinit var livenessDetectionView: LivenessDetectionCameraView
  <br></br>
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
  }
}
</code>
</pre>
</li>

<li>
  <p>In <code>onResume</code> cycle call initializer:</p>
  <pre><code>
    @RequiresApi(Build.VERSION_CODES.N)
    override fun onResume() {
        super.onResume()
        initializer.start {
            runOnUiThread {
                setContentView(R.layout.activity_main)
                livenessDetectionView = findViewById(R.id.livenessDetectionView)
                livenessDetectionView.addListener(this)
                livenessDetectionView.setOptions(
                    LivenessDetectionOptions(
                        arrayListOf(LivenessPromptType.LOOK_LEFT, LivenessPromptType.LOOK_RIGHT, LivenessPromptType.BLINKING)
                    )
                )
                livenessDetectionView.start()
            }
        }
    }
</code>
</pre>
</li>

<li>
  <p>In while camera view recently focus, log the frame status:</p>
  <pre><code>
    override fun onFrame(frameStatus: FrameStatus) {
        if (frameStatus != FrameStatus.PROCESSING) {
            Log.d(TAG, "onFrame: $frameStatus")
        }
    }
</code>
</pre>
</li>

<li>
<p>In <code>onPromptCompleted</code> when front image success, call the next back image of ID card:
 <pre><code>
    override fun onPromptCompleted(completedPromptIndex: Int, succeed: Boolean, progress: Float) {
        Log.d(TAG, "onPromptCompleted")
        this.livenessDetectionView.nextImage()
    }
</code>
</pre>
</li>

<li>
  <p>Find out the full sample code here:</p>
  <pre><code>
package com.ekycsolutions.ekycid.example

import android.os.Build
import android.util.Log
import android.os.Bundle
import androidx.annotation.RequiresApi
import androidx.appcompat.app.AppCompatActivity
import com.ekycsolutions.ekycid.Initializer
import com.ekycsolutions.ekycid.example.R

import com.ekycsolutions.ekycid.models.FrameStatus
import com.ekycsolutions.ekycid.documentscanner.DocumentScannerResult
import com.ekycsolutions.ekycid.documentscanner.DocumentScannerCameraView
import com.ekycsolutions.ekycid.documentscanner.DocumentScannerEventListener
import com.ekycsolutions.ekycid.livenessdetection.*

class MainActivity : AppCompatActivity(), LivenessDetectionEventListener {
    val TAG = "MainActivity"
    val initializer = Initializer(this)

    lateinit var livenessDetectionView: LivenessDetectionCameraView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    @RequiresApi(Build.VERSION_CODES.N)
    override fun onResume() {
        super.onResume()
        initializer.start {
            runOnUiThread {
                setContentView(R.layout.activity_main)
                livenessDetectionView = findViewById(R.id.livenessDetectionView)
                livenessDetectionView.addListener(this)
                livenessDetectionView.setOptions(
                    LivenessDetectionOptions(
                        arrayListOf(LivenessPromptType.LOOK_LEFT, LivenessPromptType.LOOK_RIGHT, LivenessPromptType.BLINKING)
                    )
                )
                livenessDetectionView.start()
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

    override fun onPromptCompleted(completedPromptIndex: Int, success: Boolean, progress: Float) {
        Log.d(TAG, "onPromptCompleted")
        this.livenessDetectionView.nextImage()
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
        Log.d(TAG, "onCountDownChanged - current: $current, max: $max")
    }
}
</code>
</pre>
</li>
</ol>


<h1>Device Requirement</h1>
<h3 dir="auto">Android Version</h3>
<p>Currently EkycID require Android API level 21 or newer. For best performance and compatibility, we recommend at least Android 6.0.</p>

<h3 dir="auto">Cemera</h3>
<p>Camera video preview resolution also matters. In order to perform successful scans, camera preview resolution must be at least 720p. Note that camera preview resolution is not the same as video recording resolution.</p>

<h3 dir="auto">Processor architecture</h3>
<p>EkycID is distributed with ARMv7, ARM64, x86 and x86_64 native library binaries.

EkycID is a native library, available for multiple platforms. Because of this, EkycID cannot work on devices with obscure hardware architectures. We have compiled EkycID native code only for the most popular Android ABIs.

You should check if the EkycID is supported on the current device. Attempting to call any method from the SDK that relies on native code,  on a device with unsupported CPU architecture will crash your app.

If you are combining EkycID library with other libraries that contain native code into your application, make sure you match the architectures of all native libraries.</p>

<h2>Contact</h2>
<p>For any other questions, feel free to contact us at 
  <a href="https://ekycsolutions.com/">ekycsolutions.com</a>
</p>

