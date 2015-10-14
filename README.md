![Xavier Logo](./readme_images/passport_scanning_simplified.jpg)  
![Xavier Logo](./readme_images/xavier_logo.jpg)  

### Xavier Android Integration Manual  
<br>
####For Xavier Android SDK 1.0, September 2015   
####By SimonComputing Inc.  5350 Shawnee Road, Suite 200  Alexandria, VA 22312    
<br>
**Description**  

The Xavier SDK contains a demo application that demonstrates the API calls you can use to interact with the Xavier Library. The Xavier SDK is an Android SDK that enables the developers to integrate the ability to scan International Civil Aviation Organization (ICAO) compliant two-line passport traveldocuments and three-line ID cards. Some sample documents that Xavier SDK can process are:
<br>

* Passport  
* Refugee Travel Document  
* Visa, Resident Alien, Commuter  
* Re-Entry Permit  

The Xavier SDK is capable of scanning the travel document via the native camera to extract all the Machine Readable Zone (MRZ) fields from the travel documents. Xavier SDK performs auto capture when the quality threshold is reached or timeout occurred. The resulting  data are returned as key-value pair elements.  

To test out the Xavier application on your Android phone, you can follow the link below to download it from Google Play:

&nbsp;&nbsp;&nbsp;&nbsp;
[https://play.google.com/store/apps/details?id=xavier.simoncomputing.com.xavierlibrary](https://play.google.com/store/apps/details?id=xavier.simoncomputing.com.xavierlibrary) 


To integrate the Xavier SDK into your project, you need two Android libraries located in the app libs folder:  

* xavier-release.aar&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This is the Xavier library
* tess-two-release.aar&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This is the Google Leptonica library  

The provided demo project was created using <b>Android Studio IDE version 1.2.2</b>. Please follow the instructions below on setting up and running the Xavier SDK demo application in Android Studio IDE. Android Studio IDE version 1.2.2 was used to develop the Xavier Evaluation SDK and the demo application. The project is configured to compile at Android API level 22 (Lollipop) andthe minimum Android API level is set to 14 (Ice Scream Sandwich).  

The Xavier Evaluation SDK has been successfully tested on both Lollipop and KitKat Android operating systems. The Xavier Evaluation SDK has been tested on the following Android phones  

* HTC One
* Samsung Galaxy (S4, S5, S6)
* Google Nexus 6.  

The Xavier Evaluation SDK will require a key and the email address registered to that key to operate.  You may go to the below link below to request for your key  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[http://www.simoncomputing.com/main/xavier](http://www.simoncomputing.com/main/xavier)   

You need to specify an email address to receive the generated key.  There is no obligation to purchase the Xavier SDK.  We invite you to explore and try it out for free.  

The Xavier Evaluation SDK displays a random pop-up screen to indicate that this is an evaluation version. Please contact SimonComputing Inc. email address xavier@simoncomputing.com for a production license version of Xavier  

####Getting the latest Xavier Evaluation SDK from GitHub  

1. On the right hand corner of the GitHub Xavier Evaluation SDK page, click the &quot;Clone In Desktop&quot; button to download the Xavier Evaluation SDK project. This is a self contained Xavier Evaluation project which has all the components for you to run the demo application on the Android phone. It also contains the two dependency libraries (xavier-release.aar and tess-two-release.aar).  
2. After the Xavier Evaluation SDK cloning is completed, open Android Studio IDE. Go to File menu and select Open and go to the cloned Xavier Evaluation SDK folder to open the Xavier Evaluation SDK project.   

####Running Xavier Evaluation SDK application on the Android phone  
1. Connect the Android phone to your laptop via the USB connection. 
2.  Before running the Xavier Evaluation SDK demo application from the Android Studio IDE, make sure the Android phone USB debugging option checkbox option in Settings is selected.   
![USB Debugging Option](./readme_images/developer_option.jpg)   
3. Run Xavier Evaluation application from Android Studio IDE.  
4. Once Xavier Evaluation application is running on the Android phone. You should see the following screen:  
5. Click &quot;Start&quot; button to initiate the MRZ capturing process. The capturing screen should display as below:    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**For two-line MRZ Document  (Figure 1)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![two-line MRZ](./readme_images/two_line_capture.jpg)   

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**For three-line MRZ document (Figure 2)**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![three-line MRZ](./readme_images/three_line_capture.jpg)   
<br>
6. To capture MRZ data accurately, hold the document as close as possible to the camera and make sure the MRZ lines (either two-line or three-line document) fall within the rectangular box on the phonescreen.   
7. The capturing screen displays a rectangular box in blue color (Figure 1) when it is not detecting any MRZ lines. The rectangular box turns to green (Figure 2) with a plus mark when it detects the MRZ lines.   
8. The capturing screen automatically goes away under one of these three conditions:  
    1. When the MRZ lines are successfully captured.  Result code value is RESULT_OK.  
    2. When timeout has occurred and it returns the MRZ lines to the onActivityResult callback.  Timeout is currently set to 5 seconds. Result code value is RESULT_OK.  
    3. When an error occurred, it returns an error message in the onActivityResult callback. Result code value is RESULT_CANCELED.   
 
 <br>
####Xavier Library Integration Code    
#####1. Starting up Xavier capturing screen  
<pre><code>
Intentintent = new Intent(MainActivity.this, XavierActivity.class);    
intent.setFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);    

// Please go to http://www.simoncomputing.com/main/xavier to request for
// an evaluation or production license key.
// ----------------------------------------------------------------------
intent.putExtra(XavierActivity.LICENSE_KEY, "E000000000");
intent.putExtra(XavierActivity.EMAIL_ADDRESS, "your_email_address@gmail.com");

startActivityForResult(intent,MRZ_REQUEST)   
</code></pre>
#####2. The MainActivity receives MRZ results in the onActivityResult callback.   
<pre><code>
protected void onActivityResult(int requestCode,int resultCode,Intent data{    
		super.onActivityResult(requestCode, resultCode,data);    
		if(requestCode == MRZ_REQUEST) {      
				if(resultCode == RESULT_OK) {     
					String mrzElements =(String)data.getSerializableExtra(XavierActivity.MRZ_LINES);    
					if(mrzElements == null||mrzElements.length()==0){    
						// No MRZ lines detected, request for another capture    
					} else {     
						// Successfully captured MRZ lines    
					}      
				}  
				else if(resultCode == RESULT_CANCELED) {  
						String errorMessage=(String)data.getSerializableExtra(XavierActivity.ERROR_MESSAGE);       
						if(errorMessage != null || !errorMessage.isEmpty()) {     
							// Log or display error message    
						}    
				}     
		}    		
} 
</code></pre>

####Sample MRZ result data   
Here is the value of the mrzElements string which contains all the parsed MRZ elements in the onActivityResult method demo code when the MRZ capture is completed and onActivityResult callback is called:  
{documentType=P,   
subtype=&lt;,  
org=USA,   
name=MOSS&lt;&lt;FRANK&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;,   
number=340000060,   
number_check=1,   
nationality=USA,  
birthdate=500101,  
birthdate_check=3,  
sex=M,  
expiration_date=060518,  
expiration_date_check=4,   
personal=700000001&lt;7866,   
personal_check=9,   
check=2}   

In our sample code, the mrzElements string result in the onActivityResult method is parsed and displayed on the Xavier Passport or ID result form.   

####Handling unknown document type   
For demoing purposes, when the MRZ result (mrzElements string) is returned, the documentType key may have an invalid Document Type value due to an OCRing error.  The demo sample code has modules that salvage the unknown document type.  In this case, the Document Type is selected from the dropdown list. This code is demonstrated in the processMrzResult method of the MainActivity.   

####Error Handling  
When an error occurrs, the errorMessage string in the onActivityResult callback method has the detailed description and stack trace of the error. The error message contains the following information:    

*  Stack trace
*  Error cause
*  Device: Brand, Device Name, Model, Id, and Product
*  Firmware: SDK, Release, and Incremental  

####Additional Information  
Please feel free to contact us at xavier@simoncomputing.com for any questions.

#####Release Notes 
1.0.0    
Initial release of Xavier Evaluation SDK  
<br>
<br>
<br>
© 2015 SimonComputing Inc. All Rights Reserved.


