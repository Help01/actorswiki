## Multiple Scenes
The framework is based on [Multi-Scene editing](https://docs.unity3d.com/Manual/MultiSceneEditing.html).

There are two variants of scenes:
* Main
* Secondary

The main scenes are all the scenes in which the component “Starter” or the component inherited from “Starter” will be present.

Each of the main scenes always has the following structure:

* [SETUP] – The component “Starter” is added to this object, it is allowed to use SETUP for other components of settings.
* [SCENE] is the “root” of the main scene.
  * Dynamic - all created GO (GameObject) will be loaded here during the game session.

All the scenes of the framework by default do not contain a camera as it is supposed to use a separate scene with a camera.

## The component “Starter”
![Стартер](https://i.gyazo.com/9f8964dad3333abbe57a9d3f35c3cc5e.png)
"Starter" is an essential component. When the scenes are loaded or changed, the component "Starter" is processed first. Its functions include the initialization of handlers for the current scene and the management of the additional (secondary) scenes at the beginning of the Play mode running. The starters are responsible for which scenes can remain after the main scenes are closed.

Each of the starters serves as a single entry point into the program (e.g. the game level scene) - all the components inherited from Actor or Monocached will be initialized after the starters.

The following public variables are responsible for:
* Scenes To Keep - which scenes should remain after the transition from this scene.
* Scenes Depends On - which scenes should be added with the transition to this scene.

In the picture (see below) I want the Scene Kernel, the Scene Camera and the scene with room to be added to the scene.
![Стартер 2](https://i.gyazo.com/b96b3c8ea695dd0bedb384f237d1dad0.png)
How to work with the scene if there is no camera added to it but you need the camera to be? You can easily drag the camera scene into the hierarchy. When you start the game from the editor, the framework will detect that such a scene already exists and will not re-add it.

## Creating new scenes
Tools->Actors->Add->Scene

[![Создание новых сцен](https://i.gyazo.com/98602454af6ebf11cbb8a1048de87bd0.gif)](https://gyazo.com/98602454af6ebf11cbb8a1048de87bd0)

Enter the name and the pathname of the added scene in the “New scene” window.

![Создание новый сцен 2](https://i.gyazo.com/83802bb527796edb65a413d275b4bd3a.png)

Use the method 'Setup' for the registration of handlers. Use 'PostSetup' if you need the analogue of the function 'Start' ( MonoBehaviour.Start() ).

```csharp
public class StarterLevel1 : Starter 
{
    protected override void Setup()
    {
        Add<ProcessingCamera>();
        Add<ProcessingPlayers>();
        Add<ProcessingInputConnect>();
    }

   protected override void PostSetup()
    {
        var player = FindObjectOfType<ActorPlayer>();   
    }

```
When we move from one main scene to another, all the handlers added to the scene are unloaded from the system.

## Building Settings

Remember to specify / check your scenes in Building Settings. The order of the scenes is not important, just make sure that the scene that starts your game is first in the list of scenes.

## Scene Kernel

It`s particularly worth mentioning 'Scene Kernel'. It is the root scene. From it comes the initialization of the framework, plug-ins, templates. Without this scene, nothing will work and it must always be present both in 'Scenes Depends On' and in 'Scenes To Keep' on the main scenes.

In Scene Kernel it is better not to put anything, especially everything that associated with the game.


## From the Cook-Book
[Methods for working with scenes](https://github.com/dimmpixeye/Unity3d-Cook-Book/blob/master/ACTORS%20scenes.md)
