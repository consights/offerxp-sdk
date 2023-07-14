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
Add the JitPack repository to your project's build.gradle file:
arduino
Copy code
allprojects {
    repositories {
        // other repositories
        maven { url "https://jitpack.io" }
    }
}
Add the SDK dependency
Add the SDK dependency to your app module's build.gradle file:
``` groovy
dependencies {
    implementation 'com.github.consights:offerxp-sdk:1.0.1'
}
``` 
Click here to get the latest release version [![](https://jitpack.io/v/consights/offerxp-sdk.svg)](https://jitpack.io/#consights/offerxp-sdk)

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
## SDK Initialization and Error Handling
### Creating a singleton instance
To create a singleton instance of the OfferXP class, use the following code:
``` kotlin
val offerxp = OfferXP.instance
```
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
