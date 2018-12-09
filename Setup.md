## Step 1
Download a fresh version of the framework. You can find it in the [release](https://github.com/dimmpixeye/Actors-Unity3d-Framework/releases/) folder.

## Step 2
Create an empty project and delete everything from it. Unpack the Install.unitypackage that you have downloaded with the framework.
 
![How it should look](https://i.gyazo.com/c611c353000320c652f0c90d6d09e02a.png)

If you don't need some unity libraries, you can remove them from the project by unchecking from the packages window.

## Step 3
RMB Assets -> Import Package -> Custom Package -> Choose Install.unitypackage

[![Step 3](https://i.gyazo.com/fe2407b92621458309dca7241ae5b98d.gif)](https://gyazo.com/fe2407b92621458309dca7241ae5b98d)

## Step 4
_If you are going to use framework folder outside of the project (symlinking in windows for example) skip this step._
RMB Assets-> Import Package -> Custom Package -> Выбираем Assets/[0]Framework/-Install/Framework.unitypackage

## Step 5
_Skip if you have done step 4_
RMB Assets-> Create -> Folder Symlink -> Choose a folder PATH_TO_YOUR_FRAMEWORK_PROJECT/Assets/[0]Framework/Runtime 

[![Step 5](https://i.gyazo.com/d74241c122a2e47947f0cbddc3629bfb.gif)](https://gyazo.com/d74241c122a2e47947f0cbddc3629bfb)

If you did everything right, you would see a Runtime folder inside of your project with green arrow symbols right to the name.
 
![Step 5](https://i.gyazo.com/48d37bc7940c77dfca83979e6f79d194.png)
**Attention** Be sure that the Runtime folder is located inside of the [0]Framework folder.  

## Step 6
Edit->Project Settings->Player->Scripting Runtime Version .Net 4.x Equivalent 

![Step 6](https://i.gyazo.com/b34a9b1308312b910f4e78375c95f290.png)

## Step  7
RMB Assets-> Import Package -> Custom Package -> Choose the Template.unitypackage ( Locate the file in the -Install folder )
After template installing feel free to delete Install folder. If everything goes right your structure of the project should look similar to picture below:

![Step 7](https://i.gyazo.com/36febd0d11e93cae34858e65715f9ad4.png)

## Step 8
Be sure that Scene Camera, Scene Game, Scene Kernel are added to the Build Settings. The order doesn't matter but be sure to put scene you want to start the game from as first. Scene Kernel is essential for the framework.  

![Step 8](https://i.gyazo.com/eb741a5a05f4ffac385fde3ce80207b5.png)

## Step 9
Edit -> Project Settings -> Script Execution Order
Setup script execution order like on the picture below: 

![Step 9](https://i.gyazo.com/0b587602c2c63d98c123ec5d7ba8690b.png)


That's it! You are ready to work with the framework :) 👍 