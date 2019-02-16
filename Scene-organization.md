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
* Scenes Depends On - what scenes should be added with the transition to this scene.

In the picture (see below) I want the Scene Kernel, the Scene Camera and the scene with room to be added to the scene.
![Стартер 2](https://i.gyazo.com/b96b3c8ea695dd0bedb384f237d1dad0.png)
How to work with the scene if there is no camera added to it but a camera is very necessary? You can easily drag the camera scene into the hierarchy. When you start the game from the editor, the framework will detect that such a scene already exists and will not re-add it.

to be continued...