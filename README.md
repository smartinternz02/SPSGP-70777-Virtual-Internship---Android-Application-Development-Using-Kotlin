# SPSGP-70777-Virtual-Internship---Android-Application-Development-Using-Kotlin
 Virtual Internship - Android Application Development Using Kotlin
Category: Kotlin
Skills Required:Android App

Project Description:

As demand for skilled Android developers increases in the job marketplace, there is an greater need for the next generation to develop the skills of Android developement.
This program will give to complete hands-on experience on android application development using Kotlin also you will develop an application on your own.
 
 *SELF LEARNING COURSES*
 Unit 1: Android Basics In Kotlin
Duration: 10 Hrs
Skill Tags: Build your first Android apps with the Kotlin programming language. Add images and text to your Android apps, and learn how to use classes, objects, and conditionals to create an interactive app for your users.

Unit 2:Layouts
Duration: 6 Hrs
Skill Tags: Improve the user interface of your app by learning about layouts, Material Design guidelines, and best practices for UI development.

Unit 3: Navigation
Duration: 6 Hrs
Skill Tags: Enhance your users’ ability to navigate across, into and back out from the various screens within your app for a consistent and predictable user experience.

Unit 4:Connect To The Internet
Duration: 6 Hrs
Skill Tags: Write coroutines for complex code, and learn about HTTP and REST to get data from the internet.

Unit 5:Data Persistence
Duration: 6 Hrs
Skill Tags: Keep your apps working through any disruptions to essential networks or processes for a smooth and consistent user experience.

Unit 6:Work Manager
Duration: 6 Hrs
Skill Tags: Use Android Jetpack’s WorkManager API to schedule necessary background work, like backing up data or downloading fresh content, that keeps running even if the app exits or the device restarts.


I want to search for nearby places (ex: hospitals, schools, general clinics, etc.) in a certain radius of the user's current location. I have successfully managed to get the users current location, but don't know how to look for nearby places in the radius. I have managed to set up the html-attributions to get the nearby places, which looks like the following: https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=${location.latitude},${location.longitude}&radius=1500&type=restaurant&key=YOUR_API_KEY (I have put my key)

But I don't seem to know where and how do I implement this Url. Only if someone could tell me how could I use this URL. That too could be of great help! Thank You!

This is my code currently:

 

import android.content.pm.PackageManager
import android.location.Address
import android.location.Geocoder
import android.location.Location
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.ActivityCompat
import com.google.android.gms.location.FusedLocationProviderClient
import com.google.android.gms.location.LocationServices
import com.google.android.gms.maps.CameraUpdateFactory
import com.google.android.gms.maps.GoogleMap
import com.google.android.gms.maps.OnMapReadyCallback
import com.google.android.gms.maps.SupportMapFragment
import com.google.android.gms.maps.model.LatLng
import com.google.android.gms.maps.model.Marker
import com.google.android.gms.maps.model.MarkerOptions
import java.io.IOException

class MapsActivity : AppCompatActivity(), OnMapReadyCallback, GoogleMap.OnMarkerClickListener {

 private lateinit var map: GoogleMap
 private lateinit var fusedLocationProviderClient: FusedLocationProviderClient
 private lateinit var lastLocation: Location

private fun placeMarkerOnMap(location: LatLng){
     val markerOptions = MarkerOptions().position(location)
     val titleString = getAddress(location)
     markerOptions.title(titleString)
     map.addMarker(markerOptions)
 }

 private fun getAddress(latLng: LatLng): String {
     val geocoder = Geocoder (this)
     val addresses: List<Address>
     val address: Address?
     var addressText = ""

     try {
         addresses = geocoder.getFromLocation(latLng.latitude, latLng.longitude, 1)

         if (null != addresses && addresses.isNotEmpty()){
             address = addresses[0]
             for (i in 0 until address.maxAddressLineIndex){
                 addressText += if (i==0) address.getAddressLine(i) else
                     "\n" + address.getAddressLine(i)
             }
         }
     } catch (e: IOException){
         Log.e("MapsActivity", e.localizedMessage)
     }
     return  addressText
 }

 companion object {
     private const val LOCATION_PERMISSION_REQUEST_CODE = 1
 }

 private fun setUpMap() {
     if(ActivityCompat.checkSelfPermission(this, android.Manifest.permission.ACCESS_FINE_LOCATION) !=
             PackageManager.PERMISSION_GRANTED) {
         ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.ACCESS_FINE_LOCATION),
         LOCATION_PERMISSION_REQUEST_CODE)
         return
     }

     map.isMyLocationEnabled = true
     fusedLocationProviderClient.lastLocation.addOnSuccessListener(this) { location ->
         if (location != null){
             lastLocation = location
             val currentLatLng = LatLng(location.latitude, location.longitude)
             placeMarkerOnMap(currentLatLng)
             map.animateCamera(CameraUpdateFactory.newLatLngZoom(currentLatLng, 12f))
         }
     }
 }
 override fun onCreate(savedInstanceState: Bundle?) {
     super.onCreate(savedInstanceState)
     setContentView(R.layout.activity_maps)
     // Obtain the SupportMapFragment and get notified when the map is ready to be used.
     val mapFragment = supportFragmentManager
         .findFragmentById(R.id.map) as SupportMapFragment
     mapFragment.getMapAsync(this)

     fusedLocationProviderClient = LocationServices.getFusedLocationProviderClient(this)

 }

 /**
  * Manipulates the map once available.
  * This callback is triggered when the map is ready to be used.
  * This is where we can add markers or lines, add listeners or move the camera. In this case,
  * we just add a marker near Sydney, Australia.
  * If Google Play services is not installed on the device, the user will be prompted to install
  * it inside the SupportMapFragment. This method will only be triggered once the user has
  * installed Google Play services and returned to the app.
  */
 override fun onMapReady(googleMap: GoogleMap) {
     map = googleMap

     map.uiSettings.isZoomControlsEnabled = true
     map.setOnMarkerClickListener(this)
     setUpMap()
 }

 override fun onMarkerClick(p0: Marker?) = false
} 
fun makeApiCall(location:Location){
    val request = Request.Builder().url("https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=${location.latitude},${location.longitude}&radius=1500&type=restaurant&key=YOUR_API_KEY")
                 .build()

    val response = OkHttpClient().newCall(request).execute().body().string()
    val jsonObject = JSONObject(response) // This will make the json below as an object for you

    // You can access all the attributes , nested ones using JSONArray and JSONObject here


}
