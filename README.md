# Unexpected error deploying or pushing Experience Bundle with LWC

### Summary

When executing `sfdx force:source:deploy`, `sfdx force:source:push` or `sfdx force:package:version:create`, the command fails when there are both ExperienceBundle and LightningComponentBundle in the metadata to push and if they do not already exist in the environment/scratch org.
The terminal returns the following error:
_"An unexpected error occurred. Please include this ErrorId if you contact support: 930263533-401529 (1302355068)"_

### Steps To Reproduce:

**Repository to reproduce:** [CommunityLWCError](https://github.com/RemiLeGuin/CommunityLWCError)

1. Clone the repository on your local machine.
2. Push it to a new scratch org with **communities** and **experience bundle metadata** enabled -> Unexpected error
3. Deploy it to a environment with **communities** and **experience bundle metadata** enabled -> Unexpected error
4. Create a new unlocked package and a new version of it using the definition file in the repository -> Unexpected error
5. Delete the "lwc" folder.
6. Push, deploy or create a package version -> Any of the steps 2, 3 or 4 succeeds.

You can also try to reproduce the full steps without using my repository:
1. Create a new Trailhead Playground and define a password for your admin user.
2. Enable Dev Hub and Unlocked Packages.
3. In Visual Studio Code, create a new empty project.
4. Connect to your new Trailhead Playground and set it as the default Dev Hub.
5. Set the _config/project-scratch-def.json_ file:
   - Add "Communities" and "Sites" to the "features": []
   - Add `"communitiesSettings": { "enableNetworksEnabled": true }` to the Settings.
   - Add `"experienceBundleSettings": { "enableExperienceBundleMetadata": true }` to the Settings.
6. Create a scratch org with the project-scratch-def.json file previously defined.
7. Open the created scratch org and create a new community with any template.
8. Pull sources from your scratch org to your local project.
9. Delete the following folders as they are not packageable: _appMenus_, _networkBranding_ and _profiles_
10. Create a new package running a command-line such as: `sfdx force:package:create --name MyPackage --packagetype Unlocked --nonamespace --path force-app`
11. Create a new package version running a command-line such as: `sfdx force:package:version:create --definitionfile config/project-scratch-def.json --package MyPackage --path force-app --installationkeybypass --wait 10`
The package version creation will work.
13. Now create a new Lightning Web Component and edit its meta.xml file to add the following tags:
```
<targets>
    <target>lightningCommunity__Page</target>
    <target>lightningCommunity__Default</target>
</targets>
```
No need to push it to the scratch org.
14. Create a new package version again. This is going to fail.

### Expected result

We should be able to push, deploy or create an unlocked package version with both Experience Bundle and Lightning Web Component.

### Actual result

The terminal returns the following error:
_"An unexpected error occurred. Please include this ErrorId if you contact support: 930263533-401529 (1302355068)"_
Full log here: [error.txt](https://github.com/forcedotcom/cli/files/4984307/error.txt)

### Additional information

**SFDX CLI Version**: 7.66.2-4f159a1d07

**SFDX plugin Version**: 49.2.3

**OS and version**: Windows 10 (2004)