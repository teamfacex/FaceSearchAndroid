# FaceSearchAndroid

Application can run either on device or emulator only in front camera

### Prerequisites

* You need an Android device and Android development environment with minimum API 21.
* Android Studio 3.2 or later.

### Building or Download
* Open Android Studio, and from the Welcome screen, select Open an existing Android Studio project.

* add  face_search.aar file in libs folder.
* add following dependencies app level


dependencies { implementation (name:'face_search', ext:'aar') }
Add following project level

allprojects {
   repositories {
            
       flatDir {
           dirs 'libs'
       }

   }
}
* If it asks you to do a Gradle Sync, click OK.
* Add file.json file in assets folder
*  You may also need to install various dependencies and tools, if you get errors like "Failed to find target with hash string 'android-21'" and similar.
 Click the Run button (the green arrow) or select Run > Run 'android' from the top menu. You may need to rebuild the project using Build > Rebuild Project.

##  implementation 'org.tensorflow:tensorflow-lite:0.0.0-nightly'
##  implementation 'com.squareup.retrofit2:converter-gson:2.1.0'
##  implementation 'com.squareup.retrofit2:retrofit:2.1.0'
##  implementation 'com.quickbirdstudios:opencv:3.4.1'

* And add this below lines in prject gradle
  aaptOptions {
       noCompress "tflite"
   }

## Additional Note
_Please do not delete or rename  file.json file

## How do I use FaceSearch?

## Java:

public class MainActivity extends AppCompatActivity {

FaceDetection faceDetection;
 @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        faceDetection = new FaceDetection(MainActivity.this);
        
 // For Automatic FaceSearch 

      Intent intent = new Intent(MainActivity.this, DetectorActivity.class);
      startActivityForResult(intent, 123);
 // For Manual FaceSearch
 
       faceDetection.getFaceresult(MainActivity.this, selectedImage, new FaceDetection.ManualSearchResponse() {
                @Override
                public void onResponse(FaceSearch faceSearch) {
                     User user = faceSearch.getData().getUser().get(0);
                }
            });
        }   
            
 // For Manual Face Add
          
          faceDetection.addUser(MainActivity.this, new File(filePath), new FaceDetection.ManualUserAddInterface() {
                @Override
                public void onResponse(boolean b, String s) {
                   
                }
            });
          }  
            
  //For Manual Face with Approval
          
          faceDetection.addUserWithApproval(MainActivity.this, filePath, name, new FaceDetection.ManualUserWithApproval() {                                              
                @Override
                public void onUserApprovalResponse(boolean b, String s) {
                    Log.e("boolean", b + "/" + s);
                }
            }); 
     }
    
    
    
      @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        
        if (requestCode == 123 && resultCode == Activity.RESULT_OK) {

            String jsonObject = data.getStringExtra("faceSearch");
            try {
                JSONObject jsonObj = new JSONObject(jsonObject);
                Log.e("jsonobject", jsonObject.toString());
            } catch (JSONException e) {
                e.printStackTrace();
            }

        }
        
    }    
    
}

## For Kotlin 

## Initialize 
   class KotlinActivity : AppCompatActivity() {

     var faceDetection: FaceDetection? = null
     
      override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_kotlin3)

        faceDetection = FaceDetection(this@KotlinActivity)
        
        }
      
  // For Automatic FaceSearch
    
    val intent = Intent(this@KotlinActivity, DetectorActivity::class.java)
        startActivityForResult(intent, 123)

// For Manual FaceSearch
    
    faceDetection!!.getFaceresult(this@KotlinActivity, selectedImage) { faceSearch ->
                Log.e("faceSearch", Gson().toJson(faceSearch))
               
            } 

    }
//For Manual FaceAdd

    faceDetection!!.addUser(this@KotlinActivity, File(filePath)) { b, s -> }
 
//For Manual FaceAdd with approval
 
    faceDetection!!.addUserWithApproval(this@KotlinActivity, filePath, name!!.text.toString()) { b, s ->  }


 
 


