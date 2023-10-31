# OfferXP SDK Installation Guide
The OfferXP SDK is a software development kit that enables developers to integrate rewards and loyalty features into their Android applications. This guide will walk you through the installation process for the OfferXP SDK.

## Prerequisites
Before proceeding with the installation, make sure that you have the following prerequisites:

* Android Studio 4.1 or higher
* Minimum SDK version: 23
* Kotlin version: 1.7.10 or higher
* Gradle version: 7.3.3 or higher
## Installation
### Add the JitPack repository
Add the JitPack repository to your project's build.gradle file
```groovy
allprojects {
    repositories {
        // other repositories
        maven { url "https://jitpack.io" }
    }
}
```
Add the JitPack repository to your ```settings.gradle``` ```dependencyResolutionManagement``` instead of ```build.gradle``` if you are using gradle build tools 7.x.x or heigher as shown below
```groovey
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url "https://jitpack.io" }
    }
}
```
Add the SDK dependency
Add the SDK dependency to your app module's build.gradle file:
``` groovy
implementation('com.github.consights:offerxp-sdk:1.0.11@aar') {
    transitive = true
}
``` 
Click here to get the latest release version [![](https://jitpack.io/v/consights/offerxp-sdk.svg)](https://jitpack.io/#consights/offerxp-sdk)

Setting `transitive = true` is essential because it ensures that all the dependencies required by the SDK are automatically included in your project. This simplifies dependency management, reduces compatibility issues, and guarantees that the SDK functions correctly.

Use the Material app theme
To ensure that your app looks consistent with other Material Design apps, use the Material app theme. Add the following line to your app module's AndroidManifest.xml file, inside the application tag:
``` xml
<application ... android:theme="@style/Theme.MaterialComponents.Light" ...> 
    ...
</application>
```
Usage
After completing the installation steps, you can start using the SDK in your app. Please refer to the SDK documentation for more information on how to use it.

### Troubleshooting
If you encounter any issues with the SDK integration, please check the following:

* Make sure that your app's minimum SDK version is not less than 23.
* Make sure that your Kotlin version is at least 1.7.10.
* Make sure that your Gradle version is at least 7.3.3.
* Make sure that you have added the JitPack repository to your project's build.gradle file.
* Make sure that you have added the SDK dependency to your app module's build.gradle file.
* If you encounter problems like missing classes, packages, or other build errors, double-check that you've included `transitive = true` in your SDK dependency. This ensures that all necessary components are pulled into your project, potentially resolving such issues."
## SDK Initialization and Error Handling
Initialization of the SDK is a necessary first step. Attempting to obtain an instance of the SDK before it has been initialized may lead to a crash. To prevent such situations, please ensure that you have correctly initialized the SDK. In many cases, you can accomplish this by initializing the SDK within the Application class itself.
### Initialize the sdk
Use the following code to initilize the sdk
```kotlin
OfferXP.initialize(context)
```
### Creating a singleton instance
To get a singleton instance of the OfferXP class, use the following code:
``` kotlin
val offerxp = OfferXP.getInstance()
```
Note: Calling this method before initialization may result in a crash.
### Checking Authentication
To check if the OfferXP instance is already authenticated, use the following code:
``` kotlin
if (offerxp.isAuthenticated()) {
    // Token is already present in the device
} else {
    // Token is not present in the device
}
```
### Setting Tokens
If the authentication check returns false, set the token using the following code:
``` kotlin
offerxp.setTokens(authToken, refreshToken)
```
Setting Callback Listeners
To set a callback listener for authentication errors and other issues, use the following code: 
``` kotlin
offerxp.setRequestListener(object : OfferXPRequestListener {
    override fun onReceivedUnauthorizedRequest() {
        // Handle unauthorized case here
    }

    override fun onReceivedSuccessRequest() {
        // Handle API success cases if needed
    }

    override fun onError() {
        // Handle API errors if needed
    }

    override fun onSettingsError() {
        // Handle Settings related errors if needed
    }
})
```
### UI Customization Settings
To set custom settings using the OfferXPSettingsBuilder class, use the following code:
``` kotlin
val settings = OfferXpSettingsBuilder()
    .enableActionBar() // or .enableActionBar(false)
    .enableDarkMode() // or .enableDarkMode(false)
    .setActionBarText("My Rewards")
    .setActionBarColor("#C8C8C8","#323CA") // or .setActionBarColor(Color.WHITE, Color.BLACK)
    .setStatusBarColor("#C8C8C8","#323CA") // or .setStatusBarColor(Color.WHITE, Color.BLACK)
    .setPrimaryActionBackgroundColor("#faf9eb","#a8a232") // or .setPrimaryActionBackgroundColor(Color.WHITE, Color.BLACK)
    .setWindowBackgroundColor("#ffffff","#800000") // or .setWindowBackgroundColor(Color.WHITE, Color.BLACK)
    .setPopupWindowBackgroundColor("#ffffff","#800000") // or .setPopupWindowBackgroundColor(Color.WHITE, Color.BLACK)
    .setIconTint("#ffffff","#000000") // or .setIconTint(Color.WHITE, Color.BLACK)
    .build()

offerxp.settings = settings
```
### Requesting a Reward Coupon
To request a reward coupon, you can call the ``` offerxp.requestReward()``` method and pass in the campaign slug and a lambda function that will be called once the reward fetching is complete. The lambda function will receive a CampaignReward object. Then you can call ``` showCampaignRewardPopup(context,campaignReward) ``` to show a build in popup message. 
``` kotlin
OfferXP.getInstance().requestReward("K04HJK20") {reward->
               OfferXP.getInstance().showCampaignRewardPopup(this,reward)
           }
           
```
To retrieve a list of all active campaigns and access their details in the Android SDK, you can use the following code snippet:
``` kotlin
/**
 * Retrieve a list of all active campaigns.
 */
OfferXP.getInstance().getAllActiveCampaigns { campaigns ->
    // Handle successful retrieval of active campaigns
    for (campaign in campaigns.campaigns) {
        // Access campaign details
        val slug = campaign.slug
        // Process other campaign details as needed
    }
}
```
In this code, the ```getAllActiveCampaigns()``` method is called on the ```OfferXP``` instance, and a lambda function is passed as the parameter. The lambda function takes a single argument ```campaigns```, which represents the response object containing the list of active campaigns.
Inside the lambda function, you can access the ```campaigns``` field and iterate over the campaigns list. The campaign slug is retrieved using ```campaign.slug```. You can add additional code to process other campaign details according to your requirements.
### Quick Reward
To quickly retrieve a random reward from any of your active campaigns without diving into the implementation details of specific campaigns, you can use the following Kotlin code snippet:
``` kotlin
/**
 * Request a random reward from any active campaign.
 */
OfferXP.getInstance().requestRandomReward { reward ->
    OfferXP.getInstance().showCampaignRewardPopup(this, reward)
}
```
In the code snippet above, the ```requestRandomReward()``` method is called on the ```OfferXP``` instance. It takes a lambda function as a parameter, which receives the reward object as its argument.
Inside the lambda function, you can handle the ```reward``` as needed. In this example, the ```showCampaignRewardPopup()``` method is called on the ```OfferXP``` instance to display a reward popup. The ```this``` parameter represents the context or activity where the popup should be displayed, and reward is the retrieved random reward from any active campaign.
### Displaying User Rewards
To showcase a user's rewards within your app, simply call the following method:
```kotlin
OfferXP.getInstance().launchRewardsActivity(this)
```
Where 'this' represents an activity context. This function launches the built-in screen designed to display user rewards. It automatically fetches and presents all user rewards upon activation. Should you encounter any API errors during this process, you can monitor and handle them through the respective methods of the requestListener that you've configured with the OfferXP instance.
This feature provides a seamless way to engage users and showcase their earned rewards within your application.
## Workarounds to handling the Crashes Due to non-Material theme in your app
In some cases, your app may not use a Material theme or its descendant, leading to potential crashes when utilizing functions like `showCampaignRewardPopup()` or `launchRewardsActivity()`. To resolve this issue, consider the following workarounds:
### Setting the Reward Activity Theme
To ensure the `RewardsActivity` utilizes a Material theme without affecting the rest of your app's theme, follow these steps:
Add the Material theme dependency to your `build.gradle` file:
```gradle
implementation 'com.google.android.material:material:1.9.0'
```
In your AndroidManifest.xml file, specify the Material theme for the RewardsActivity as follows:
```xml
<activity
    android:name="com.offerxp.reward.offerxp_sdk.core.ui.RewardsActivity"
    android:label="My Rewards"
    android:theme="@style/Theme.MaterialComponents.Light.NoActionBar" />
```
This configuration applies the Theme.MaterialComponents.Light.NoActionBar theme specifically to the RewardsActivity, allowing it to function smoothly without affecting your app's overall theme.
### Handling showCampaignRewardPopup() Crashes
If you experience crashes with showCampaignRewardPopup(), it may be due to passing a non-Material theme activity context to the method. To resolve this, create a new context wrapper with the Material theme:
```kotlin
import android.content.ContextWrapper
import android.view.ContextThemeWrapper

// Obtain your original context
val originalContext = this // Replace 'this' with your current context

// Define the Material theme you want to use
val materialThemeResId = R.style.Theme.MaterialComponents

// Create a ContextThemeWrapper with the Material theme
val materialContext = ContextThemeWrapper(originalContext, materialThemeResId)

// Use 'materialContext' when calling the method
OfferXP.getInstance().showCampaignRewardPopup(materialContext, reward)
```
In the provided code, we first obtain the original context (replace 'this' with your current context). Then, we create a ContextThemeWrapper with the desired Material theme resource ID (R.style.Theme.MaterialComponents). Finally, you can safely use materialContext when calling the showCampaignRewardPopup() method.
By following these workarounds, you can ensure that the context passed to showCampaignRewardPopup() uses the Material theme, even if your app's overall theme is not a descendant of the Material theme.
