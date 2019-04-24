# Dow AR Model Management Platform Documentation

## Go To

* [How To Use](#how-to-use)
* [Roles](#roles)
  * [Adding New Roles](#adding-new-roles)
  * [Adding People To Groups](#adding-people-to-groups)
  * [Editing Roles](#editing-roles)
  * [Giving Access to Models Based On Role](#giving-access-to-models-based-on-role)
* [Authorization Process](#authorization-process)
  * [Authorization Code Interceptor](#authorizationcodeinterceptor)
  * [Microsoft Graph Service](#microsoftgraphservice)
  * [SharePointFolderMakeGraphCall](#sharepointfoldermakegraphcall)
* [Tutorial](#tutorial)
* [Model Selection](#model-selection)
* [AR Screen](#ar-screen)
  * [iOS](#ios)
  * [Android](#android)
  * [Plane Visualization](#plane-visualization)
  * [Resetting AR Session](#resetting-ar-session)
* [Creating an AssetBundle](#creating-an-assetbundle)

## How To Use

App Center is being used to distribute the latest version of the application for both Android and iOS. The application can be installed onto a users device directly from App Center by using their Dow login.

Once the application is open a user can login to the application using their normal Dow employee login. After login has been completed a user returns to the application and will be shown all the models they have access to. A login will be shown for first time users to acclimate them to the application. From there a user will be able to operate the application normally. Models will be automatically updated from the database whenever a user navigates back to the model selection screen.

[Return To Top](#go-to)

## Roles

In order for different teams to access different models, you first have to create groups inside of the [Sharepoint instance](https://workspaces.bsnconnect.com/sites/Ext_MSU_Capstone_2019/SitePages/Home.aspx) we have set up. 

**Not Entirely Azure AD(Unfortunately)**

Right now in Azure AD, you are(or at least with our access level we were) only able to give access to the site as a whole and not individual elements of the site. To do that, you have to go in and manage permissions on the sharepoint site itself. 

### Adding New Roles

When adding new roles, the best way to make a new role is to [create a group](https://workspaces.bsnconnect.com/sites/Ext_MSU_Capstone_2019/_layouts/15/newgrp.aspx?Source=https%3A%2F%2Fworkspaces%2Ebsnconnect%2Ecom%2Fsites%2FExt%5FMSU%5FCapstone%5F2019%2F%5Flayouts%2F15%2Fuser%2Easpx%3FSource%3Dhttps%253A%252F%252Fworkspaces%252Ebsnconnect%252Ecom%252Fsites%252FExt%255FMSU%255FCapstone%255F2019%252F%255Flayouts%252F15%252FUser%252Easpx%253Fobj%253D%25257B6acab2a9%25252D9b39%25252D4783%25252Da837%25252De4535d218838%25257D%25252C1%25252CLISTITEM%2526List%253D%25257B6acab2a9%25252D9b39%25252D4783%25252Da837%25252De4535d218838%25257D%2526Sourcehttps%25253A%25252F%25252Fworkspaces%25252Ebsnconnect%25252Ecom%25252Fsites%25252FExt%25255FMSU%25255FCapstone%25255F2019%25252FSitePages%25252FHome%25252Easpx) which you can get to by clicking that link or:

* Go to the *Home* List
* Click the Gear Icon in the top-right, then choose *Site Settings*
* Under *Users and Permissions*, there should be a link called *Site Permissions*
* If you are an admin, you should have access to Create a Group. Do that while adding the initial role  you have a new role created. Now you can go and give it permissions by going into the places you want and granting permissions. 

[Return To Top](#go-to)

### Adding People to Groups

Adding people to groups is a similar process as making new roles. If you go [here](https://workspaces.bsnconnect.com/sites/Ext_MSU_Capstone_2019/_layouts/15/user.aspx) This should have all the roles listed there because it's home and they need to at least have read access to be able to see the home menu. If you want to edit a group permission, given you have the correct access, you can click on the name of the group, then either add users with the _New_ button or remove users by checking them and then click _Actions_ -> _Remove Users From Group_


### Editing Roles

Editing Groups(roles) is similar to the process of adding people to groups. If you go [here](https://workspaces.bsnconnect.com/sites/Ext_MSU_Capstone_2019/_layouts/15/user.aspx) This should have all the roles listed there because it's home and they need to at least have read access to be able to see the home menu. If you want to edit a group permission, given you have the correct access, you can click on one of the checkboxes and then go to _Edit User Permissions_ to change the whole group's general access level. Or you can click on the group itself, go to the settings tab and change other things about the group. 

[Return To Top](#go-to)
### Giving Access To Models Based On Role

If you want to give access to models based on their role and what team they are on, there are a few different approaches you can take:

   [1. Create a new list on the side for that team and give the group access to that list specifically](#create-a-new-list) 
 
   [2. Give access to models on a user-by-user basis(not really recommended)](#give-access-per-user) 
 
   [3. Give Groups access to models on a model-per-model basis(not recommended either) ](#giving-groups-access-per-model) 

 #### Create A New List
 You can create a list pretty easily and as long as the user has access to the list, the app will find the model and load it in. Go into site contents on the sidebar and click New -> App Then Search For _ModelListTemplate_. Click add, give it a name and description and you're new list is created! 
 
 You can get back to this list by either adding it to the sidebar or go into Site Contents and click the link on the side. In order to give the group access to that library:
 
 #### Adding Groups to Lists 
 
 * Head over to the list and click the gear in the top-right
 * Select Library Settings
 * Under Permissions and Management click Permissions for this document library
 * Click Grant Permissions
 * Search for the Group(s) that you want to give list access to and add them(make sure to uncheck "give access to everything" if you plan on giving access to specific models within that folder(again not recommended)
 
 
 
 #### Give Access Per User
 This option is the same as [adding groups to lists](#adding-groups-to-lists) but when you go to add people, just add individual emails instead.  
 
 #### Giving Groups Access Per-Model
 If you go and check the model you want to give access to(either one at a time or none for the entire folder), you click the _i_ with the circle around it click manage access and you can add people by clicking _Grant Access_ and adding groups that you want to give access to. 
 
 _PS: If you want models that everyone can access, maybe make a list named general that sharepoint members have access to and add models that way_ 


[Return To Top](#go-to)



## Authorization Process

Calls to get items from sharepoint and user info are from the [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)

### Authorization Code Interceptor

The authorization flow within Unity requires a GameObject with the Authorization Code Interceptor script. 
In "Intercepted Hosts" elements should be any URLs you want the object to intercept. For example "https://graph.microsoft.com" as element 0 will intercept any call made to that url and capture the response. 
If there is no auth token it will then attempt to get one automatically. This URL should match the url in MicrosoftGraphService.cs in the project.
The client Id and client Secret should be set based on the AzureAD application client and secret.

The redirect (currently http://exist1.haptixgames.com:8080/exist/restxq/oauth-interceptor) is necessary for redirecting and capturing some information after the user logs in.
Scopes can vary, but should contain information for each element like openid, email, or AllSites.Read.

Authorization server URI should be https://login.microsoftonline.com/c3e32f53-cb7f-4809-968d-1cc4ccc785fe/oauth2/authorize
Access Token server URI should be https://login.microsoftonline.com/c3e32f53-cb7f-4809-968d-1cc4ccc785fe/oauth2/token
Access Token Uri parameters grant_type should have a key of "grant_type", and value of "authorization_code". Prompt should have a key of "prompt" and value of "login".

 ### MicrosoftGraphService

 Additional help [here](https://www.youtube.com/watch?v=nMHjWjgo7kY).
 This script should have an http path to whatever URL you want to intercept responses from matching Authorization Code Interceptor Above.

 ### SharePointFolderMakeGraphCall

 The script SharePointFolderMakeGraphCall iterates through all items in the Documents list currently. SetDataSource() has the paths it   should follow, so other lists can be added and iterated through.
 #### Additional authorization
 
 The OAuthInterceptor, once it has a token, will always attempt to use that token. If it is expired it will try and refresh that token automatically.

[Return To Top](#go-to)

## Tutorial

### What it does
The tutorial was made to imitate the actual scenes that are used throughout the app while also having an interactive aspect.
The user is greeted with a main menu with buttons leading them to the 4 main scenes within the app.
Only on the first use of the app will the user be taken tutorial right after logging in. Afterwards the visit is stored in 
the PlayerPreferences as a bool and from then on the user will be taken straight to the model selection screen after logging 
in. The user can revisit the tutorial if they later wish to by clicking the button labeled 'Tutorial' within the UserInfo
screen. 

### How it's done
We took pictures of active scenes on the Dow iPads themselves to get the resolution correct and inserted them into separate 
canvases. On these canvases we implemented the interactiveness by using invisible buttons and images with variable transparency.
Every part of a scene that is of any importance will have a an image object above it in order to highlight that specific
region of the screen. This image's transparency, or alpha value, is manipulated in code to range from 0 to 0.25 (Complete 
transparency to 25 percent transparent). This transparency is to get the user's attention and to indicate that this region 
of this screen is interactive. On top of this image is an invisible button so that the user's touch will trigger a text to 
pop up on the screen that explains a little about the corresponding scene element. Everything is located within the Tutorial.cs
script.

[Return To Top](#go-to)

## Model Selection

After retrieving all of the models from the backend and local storage, all of the models are put into their own prefab called Model Selection Entry.This displays the model and the name. When one of these is sellected that model is passed to the model configuration canvas and can be configured there. Once a user clicks "view" the model is placed in DontDestroyOnLoad so that it can be found in the AR View scene.

[Return To Top](#go-to)

## AR Screen

The AR Screen is the only part of the application that could not be built cross-platform within Unity. Therefore, we have created two scenes named iOS AR View and Android AR View. The iOS screen uses the native AR Kit and Android uses the native AR Core.

### iOS

iOS AR View contains a parent GameObject named AR Kit that contains all of the vital components for running an AR Session. These bundle was adapted from the UnityARKitScene scene in the examples of the plugin. IOS AR View is the object containing the scripts for placing object within the scene. iOS Ar View handles maintaining the AR session and changing and states of planes and button functionality. iOSPlacer handles the placing and updating of the object we have passed here.On each update it checks whether a plane has been touched and then will place the object in the correct location. Lean Touch is a prefab we can add from the plugin that will allow us to use the plugin within this scene.

### Android

The Android AR View contains 3 major components that make up the AR Session. AR Core Device contains the camera and scripts to run the AR camera, Environmental Light uses lighting conditions to make AR objects look more real in current lighting, and Plane Generator contains the scripts that find and create planes. In the AndroidARView object, there are two scripts for handling passing and displaying the object. First Android Model Passer takes the model passed from Model Selection and puts it into AndroidARView. AndroidARView handles the polling for touches on planes to place models as well as updating the state of the planes and model. Button switching is also handled here for highlighting and transform. Lean Touch is again required for moving and rotating the models.

[Return To Top](#go-to)

### Plane Visualization

#### Default Behavior

The default behaviour of the plane visualization feature is as follows:
* It will always be ON when there are no models placed
* It will automatically turn OFF after a model is placed on a plane
* After a model is placed, the user can choose to toggle the visualization of the planes at will

#### How it's done

The toggle within the UserInfo screen has a function called TogglePlanes() attached to it via
a script component ("iOSARView.cs" for the iOS side and "AndroidARView.cs" for the Android side). Since all of the planes in the scene are GameObjects with a tag of "plane" attached
to them on creation we can find all of the planes that were detected in the scene using the built in Unity function call 
GameObject.FindGameObjectsWithTag("plane"). This will return an array of GameObjects which we then iterate over 
and then proceed to either enable/disable the Renderer component of the plane. 

[Return To Top](#go-to)

### Resetting AR Session

#### What it does

The purpose of resetting the AR session is to start on a fresh AR view without restarting the entire scene itself. 
When the AR session gets reset all of the planes, feature points, and any tracking data will be cleared. 

#### How it's done

The button within the UserInfo screen has a function called ClearARSession() attached to it via
a script component ("iOSARView.cs" for the iOS side and "AndroidARView.cs" for the Android side).
The function itself starts a coroutine which is where the resetting of the AR session actually happens.
We first get session object from the scene. We then proceed to retrieve the session configuration from the session object 
and store it in another variable. This configuration data is important as it will make sure that our new session will have 
the same properties as our old one. After we have saved the session configuration we will destroy the existing session object
and create a new one. After a new one is made and we will then set the old session configuration variable to the new session
variable. 

[Return To Top](#go-to)

## Creating an AssetBundle
### Importing Scripts into Your Project
* Import these scripts from the Model Management Platform source
  * ModelPartHighlight.cs
  * CreateAssetBundles.cs
  * CreateAndroidAssetBundles.cs
* In your Unity project, import these scripts by copying them into your assets folder. 
* Place CreateAssetBundles and CreateAndroidAssetBundles into a folder named ‘Editor’

### Creating a Prefab
* Import the model file into your unity file structure.
* Place your model in the scene.
* If your model requires transform changes (scale, rotation, pivot point)
  * Create an Empty Game Object in the hierarchy
  * Reset the Parent’s Transform (In the inspector, click the gear icon on Transform)
  * Child the model to the empty Game Object
  * Adjust the child model, keeping the parent the same
*	Drag the Model (or its new parent) into your Assets folder. This will create a Prefab.
*	Rename your prefab to the name you wish to show in the application

### Adding Model Highlighting
*	Identify which pieces of the model require highlighting. All parent and child parts of the model may be highlighted, if they meet these requirements:
  *	The piece must have a mesh renderer
  *	The piece must not have a collider
*	Add ModelPartHighlight.cs onto the model piece in the prefab
*	Fill out the public variables on ModelPartHighlight.cs

### Creating the AssetBundle
*	Select the Model Prefab or its parent
*	If the preview window in the inspector is hidden, drag the grey bar up until the AssetBundle option is available
*	Click on the dropdown to the right of the word AssetBundle in the preview
  *	If a name does not already exist, select new and create it,
  *	Select your name of choice
*	Once you have created all AssetBundles, navigate to Assets -> Build Asset Bundles (iOS) or Assets -> Build Android Asset Bundles (Android)
*	This process will create a folder, either called AssetBundles (iOS) or AndroidAssetBundles (Android). These are your model AssetBundles!

### Uploading the AssetBundles
Simply Upload your model to the SharePoint list for the desired role!

[Return To Top](#go-to)
