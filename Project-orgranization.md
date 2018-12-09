## Assembly Definition
The framework contains some [Assembly Definition](https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html) dynamic libraries. If you don't want use those be sure to delete them.
 
Usually, Assembly Definition speeds up compilation time of scripts, but it will work if you use Assembly Definition _everywhere_ in the project.

The other case of using Assembly Definition is to divide your scripts into domains strictly. It allows you not to mix scripts from the framework and your game. 
 

Naming conventions in the framework:
- Namespace.Source
- Namespace.Source.Editor <- for editor folders.

![Example](https://i.gyazo.com/e8980bba613d5604546e615740935bf7.png)

## Folder structure
* [0]Framework - Homebrew.Framework 
    * Common - I use the Common folder for documentation, misc files, and libraries that I write myself.
    * Runtime -  The framework core. All framework logic is here. Edit scripts from this folder only if you need to extend the framework. Also be sure not to include any game scripts here.

* [1]Source - Homebrew.Game ( Ref: Homebrew.Framework )
It is the folder with your game source. If you are using Assembly Definition be sure to link other Assembly Definition files to the Homebrew.Game. ( Unity.TextMeshPro for example. It can be found in Packages/TextMeshPro/Scripts/Runtime )

* [2]Content - All non-coding resources that you want to use in the game. 
    * Resources
        * Data
             * Blueprints 
        * Prefabs 
    * Scenes 

* [3]ThirdParty - For all assets that you bought/downloaded.
* Plugins - Unity folder for native plugins.
* Streaming Assets 

You can learn more about Unity folders from the
[SpecialFolders](https://docs.unity3d.com/Manual/SpecialFolders.html) link.

Right now it's essential to have [2]Content folder with Resources/Prefabs and Resources/Data folders. 
 
##  Naming conventions
_Optional_
Conventions based on my own experience of working with Unity.


### Script naming
I always put a type of script in the front. It allows understanding what the _purpose_ of the script is. In ECS paradigm where components and logic strictly divided it makes sense.
 

* All actors start with the _actor_ word: ActorPlayer, ActorEnemy1, ActorEnemy2, ActorTank, ActorSpaceShip, ActorAsteroid
* All components start with the _component_ word : ComponentPlayer, ComponentView, ComponentMotion, ComponentWeapon
* All systems, controllers, managers start with the _processing_ word :  ProcessingAI, ProcessingMotion, ProcessingRender
* All signals start with the _signal_ word : SignalDamage, SignalJobsDone
* The same for all other types: Sample,Blueprint,Factory

![Scripts naming](https://i.gyazo.com/958b430486bcf32451d94dfce87044e7.png)

Следуя этому правилу очень легко запомнить, что все "менеджеры и контроллеры" фреймворка начинаются со слов Processing. Так, захотев воспользоваться сигналами в фреймворке вы уже будете искать что-то типа ProcessingSignals.

### Названия ассетов и префабов.
ТИП АССЕТА пробел ТИП пробел УКАЗАТЕЛЬ пробел ИНДЕКС
Например : Animation Enemy Attack, Sample Weapon 2

![Пример](https://i.gyazo.com/e063a7922892cd70e3e7551c1b145315.png)


PS Не ленитесь использовать готовые паттерны по названиям или разрабатывать свои. С ростом проекта растут и издержки на его поддержание. Дисциплина, прозрачность, единообразие являются залогом более продуктивной и устойчивой работы.