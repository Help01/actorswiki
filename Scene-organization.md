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
"Starter" is an essential component. When the scenes are loaded or changed, the component "Starter" is processed first. Its functions include the initialization of handlers for the current level, the management of the additional (secondary) scenes at the beginning. The starters are responsible for which scenes can remain after the main scenes are closed.

Any starter serves as a single entry point into the program (the game level) - all the components inherited from Actor or Monocached will be initialized after the starters.

to be continued =)