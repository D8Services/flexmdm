# Flexapp



## Explanantion of Settings

Below is an explanation of each of the prerfences and what they do.

**serverURL**       = Your Jamf pro server URL ( At the moment the / on the end is not supported or tested)

**apiCredentials**  = The encoded credentials for your API user. See here https://www.jamf.com/developers/apis/classic/code-samples/

**deviceID**        = Is always $JSSID and is the UID of your device in jamf pro. Do not edit

**forceDataEntry**  = Determines if the user is forced to enter data on first launch (land on DataEntryView)

**isEditable**      = Determines if the user can edit their data from the details view. Edit after provisioning

**useLDAP**         = Not implemented

**enableAssetTag**  = Determines if you want to display the asset tag field from jamf (editable)

**enableUsername**  = Determines if you want to allow the user to enter their username into jamf

**labelUsername**   = You can rename how the label "Username" is visible to the user eg; "PC login name"

**enableFullname**  = Determines if you want to allow the user to enter their fullname into jamf

**enableEmail**     = Determines if you want to allow the user to enter their fullname into jamf

**enableDepartment** = Determines if you wish to allow the user to select their department from the list of deprments in JP

**enableBuilding**  = Determines if you wish to allow the user to select their building from the list of building in JP

**enableSites**     = If you are using sites, you can allow the user to choose their site from a list of sites from JP

**labelSites**      = You can rename how the label "Sites" is visible to the user eg; "School Code"

**labelBuilding**   = You can rename how the label "Building" is visible to the user eg; "Campus"

**labelDepartment** = You can rename how the label "Department" is visible to the user eg; "Cart Number"

**allowErase**      = Determines if the erase button is available to the user to wipe their own device (reset&re-enroll)

**clearActivation** = If the devcice is capable (supervised), and the user erases their device, do you want to clear ALock



## Examples of config

Use the following code samples as example app config
```
<dict>
    <key>serverURL</key>
    <string>https://d8.jamfcloud.com</string>
    <key>apiCredentials</key>
    <string>ZnJlZDp2aFF5ckZTdTF0c0Y=</string>
    <key>deviceID</key>
    <string>$JSSID</string>
    <key>forceDataEntry</key>
    <false/>
    <key>isEditable</key>
    <true/>
    <key>useLDAP</key>
    <true/>
    <key>enableAssetTag</key>
    <true/>
    <key>enableUsername</key>
    <true/>
    <key>labelUsername</key>
    <string>Username:</string>
    <key>enableFullname</key>
    <true/>
    <key>enableEmail</key>
    <true/>
    <key>enableDepartment</key>
    <true/>
    <key>enableBuilding</key>
    <true/>
    <key>enableSites</key>
    <true/>
    <key>labelSites</key>
    <string>Site</string>
    <key>labelBuilding</key>
    <string>BUILDINZ</string>
    <key>labelDepartment</key>
    <string>DEPARTOS</string>
    <key>allowErase</key>
    <true/>
    <key>clearActivation</key>
    <true/>
    <key>enabledEAs</key>
        <array>
            <integer>2</integer>
            <integer>3</integer>
            <integer>5</integer>
        </array>
    <key>editableEAs</key>
        <array>
            <integer>2</integer>
            <integer>3</integer>
        </array>
</dict>
```

```
<dict>
    <key>serverURL</key>
    <string>$JPS_URL</string>
    <key>apiCredentials</key>
    <string></string>
    <key>deviceID</key>
    <string>$JSSID</string>
    <key>frontPageGraphic</key>
    <string>https://d8web.s3-ap-southeast-1.amazonaws.com/cpa/CathayPacificVerticalEn_SM3.png</string>
    <key>submitText</key>
    <string>big black dog</string>
    <key>forceDataEntry</key>
    <false/>
    <key>isEditable</key>
    <true/>
    <key>useLDAP</key>
    <false/>
    <key>enableAssetTag</key>
    <true/>
    <key>enableUsername</key>
    <true/>
    <key>enableFullname</key>
    <true/>
    <key>enableEmail</key>
    <true/>
    <key>enableDepartment</key>
    <true/>
    <key>enableBuilding</key>
    <true/>
    <key>enableSites</key>
    <true/>
    <key>labelUsername</key>
    <string>Username:</string>
    <key>labelSites</key>
    <string>Site</string>
    <key>labelBuilding</key>
    <string>Building</string>
    <key>labelDepartment</key>
    <string>Department</string>
    <key>allowErase</key>
    <true/>
    <key>enabledEAs</key>
    <array/>
    <key>editableEAs</key>
    <array/>
</dict>
```


## Jamf Pro Configuration

If you are using LDAP and you have this option set in your Inventory Collection Prefences;

Collect user and location information from LDAP;

Then it is entirly possible that user changes for the fields
    Fullname
    Emailaddress
    Departmnent*
    Building*
    
will be replaced by the field mapped from the LDAP server based on the username entered by the user.
**OR(TL;DR).... LDAP will override the user input if that setting is configured**

* Building and Department may not be overwritten if the list of buildings in JAMF pro is different to fields mapped from LDAP. You can also unmap those fields in your LDAP settings to have JAMF only buildings and departments...




## Preference Interaction

The two main keys that interact mutually are "forceDataEntry" and "isEditable"

These two keys will determine your workflow, and here are some examples of how they can be used....


1. We just want the app to act as a helpdesk troubleshooting tool and allow the user to update their own inventry to force apps to install.
    If you set forceDataEntry to False
                            and isEditable to False
                                    The app will launch directly to the screen that shows the details pulled form the jamf pro server. There will be no edit button so the user can not change any of the details.
                                        (View only Mode)
                                    
   
   
2. We want to deploy devices and allow employees to enter their own details and edit/change them whenever they want after the inital setup.
        If you set forceDataEntry to False
                            and isEditable to true
                                    The app will launch directly to the screen that shows the details pulled from the jamf pro server. The user can then use the edit button to enter their details if they so choose. They can then edit them at anytime by following the same proces.
                                        Alternativly you could set forceDataEntry to "true" as its irrelevant. Just changes landing page on fiorst launch.
                                        ( Edit Always Mode )
                                    


3. We want to deploy devices and force users to enter their details as a part of our onboarding. We then dont want them to be able to edit them once they have onbaorded for the first time.
        If you set forceDataEntry to true
                            and isEditable to false
                                    The app will launch directly to the screen that requires them to enter their details. Once it successfully submits those details to the server they are tranfered to the details screen where the edit button is not availbale. This will persist even if they delete and re-install app.
                                        (Edit once, Then view Only)



Here are some "Real World examples" - With so many more possibilities!

1. (View only Mode) A company is using DEP to enroll all of their mobile devices. Every device is 1-1 and will remain with the same user for the liftime of the device. They use LDAP or SAML authentication at enrollment so user details are populated by jamf pro at enrollemnt.

    However, from time to time they may call the helpdesk with an issue. Sometimes it is an app not deploying, sometimes it is just basic problem. Helpdeks may need some details from them like their asset tag or serial number.
    
    Rather than instructing the user to navigate the menus to find the serial number, they can advise them to get it from the Flex app easily instaed.
    If our helpdesk are not jamf admins, they can also align asset tag to the user and even advise the user to send an inventory update command to see if that jogs the app install along.



2. ( Edit Always Mode ) A company wants to use a single prestage for all mobile devices regardless of what building, site or division the device is destined for. They dont want authentication at enrollment becasue IT will be enrolling devices on behalf of the users before deployment.

    They can setup the deployment of apps and config profiles, such a email config to be based on department.
    
    When an employee gets their device and onboards, they open the Flex app and they can enter what department they belong too.
    Once they do all apps and settings are deployed.
    Lets say now that their use case changes or they change roles in the company. They can simply edit their deprtment info in the app and its will automatically reconfigure the device for them.
    This way the device can be essentially reconfigured without having to be erased and re-provisioned.
  
  
  
3. (Edit once, Then view Only) A scool district handles many local schools and each school has an onsite tech who recieves the devices and then sets them up for each role in the shcool.

    Some are for Carts in the library, some are 1-1 studnet devices and others are staff devices.
    
    Without the onsite tech being a jamf admin, we want them to enroll the device, assign it to their "site" in jamf pro, assign it to a user if it is a 1-1 device or staff device and set the "role" type (Cart, Student, Staff).
    
    We dont want differnt prestages setup and  the local tech does the provisioning, we cant have authentication at enrollment.
    
    We setup a prestage that automatically puts every device into a static department called "New Enrollment".
    
    So we configure it so the Flex app is deployed automatcially to "New Enrollment". A config profile is also applied to "New Enrollment" to disallow all other apps except Flex, so it is the only ap they can use.
    
    When the local tech enrolls the device, they have to choose the department (renamed role) and Site, as well as enter any user details if it is not a cart device.
    This will then reassign the device to the correct role and remove it from the "New Enrollment" department and remove restrictions while deploying apps and configs for that role.
    
    The app will now be locked to the details screen and the local tech can then deploy the device to the users, safe in the knowlege that the end user can not reassign the role after intial configuration.
    
    
    
## Some other food for thought workflows.

The app could be used as a user driven "Mode" selector.. So the user could select a "mode" (renamed department or EA) which will dynamically apply certain settings or restrcitons controlled by the user.
Self service can do config profiles, but with this it could even be conditional app/ebook deployemnt based on mode as well as easy reporting for what device is currently in which mode.
        
        
Or.... What about this.. Say you have 800 mobile devices deployed already and we do not have any user details against the device record in jamf.
Also we do not know which user has which device...
We can deploy the flex app in (Edit once, Then view Only) or ( Edit Always Mode ) and then apply a "Single App Mode" Restrcition to the devices. Rendering the device useless until the user enters their details.
Onec details are entered the restriction is automatically removed.
Hopefully assigning users against all of our unknown devices.
        
       

        
