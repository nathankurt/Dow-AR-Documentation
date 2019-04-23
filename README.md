# Dow AR Model Management Platform Documentation

## Go To

* [How To Use](#how-to-use)
* [Roles](#roles)
  * [Adding New Roles](#adding-new-roles)
  * [Adding People To Groups](#adding-people-to-groups)
  * [Editing Roles](#editing-roles)
  * [Giving Access to Models Based On Role](#giving-access-to-models-based-on-role)
* [Highlighting Different Models](#highlighting-different-models)
* [Authorization Process](#authorization-process)
  * [Authorization Code Interceptor](#authorizationcodeinterceptor)
  * [Microsoft Graph Service](#microsoftgraphservice)
  * [SharePointFolderMakeGraphCall](#sharepointfoldermakegraphcall)

## How To Use

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

## Highlighting Different Models

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
