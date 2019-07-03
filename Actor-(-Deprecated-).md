The Actor is a bridge class that allows communicating between unity game objects and framework. Most of the time you will inherit from Actors when defining new objects.

The Actor class is *NOT* for defining game logic. It's for defining what is your Entity. You use Actors only to add some components to the Entity, to have a connection between Framework and Unity out of the box and for initial setup of the object. 


### How to create ###

* Inherit a new class from an Actor class. I recommend adding "Actor" word in the front of all your Actors scripts; this allows you to identify quickly in the project hierarchy all your actor classes.

```csharp
public class ActorModuleReactor : Actor {
```

* Add the ```Setup``` method. The ```Setup``` method is an essential part that allows safely initialize your actor on Awake and sync it in the right way with the framework. Please, use only ```Setup``` method instead of ```Awake``` and ```Start```.

```csharp
public class ActorModuleReactor : Actor
	{
		protected override void Setup()
		{
			 
		}
	}
```

*  Now we are ready to add some components!
```csharp
public class ActorModuleReactor : Actor
    {
       // use the FoldoutGroup attribute to decorate components nicely 
       // in groups in the Unity Inspector
       [FoldoutGroup("Setup")]
        public ComponentStateMachine componentStateMachine;

        [FoldoutGroup("Setup")]
        public ComponentRecycle componentRecycle;

        protected override void Setup()
        {
             // add your components to the Entity of the Actor
             Add(componentStateMachine);
             Add(componentRecycle);
             
             // add a component by its type. 
             Add<ComponentDescription>;
        }
    }
```
At the next frame your Entity will be added to all processings.

[![Actor in the Inspector](https://i.gyazo.com/2bdd01853f4df82d3ddf6e8f06241b1f.gif)](https://gyazo.com/2bdd01853f4df82d3ddf6e8f06241b1f)

### Manual Deploy ###
In some situations, you might want to hold your Actor and decide when deploying your Entity to processing.
In this case, inherit your Actor from ```IManualDeploy``` interface.

```csharp
public class ActorModuleReactor : Actor, IManualDeploy
```
Finally, once you are ready to ship your Actor entity use ```ForceDeploy()``` method with your Actor. 

### ComponentObject ###
An ```Actor``` adds a ```ComponentObject``` by default. The ComponentObject holds the ```Transform``` reference of the Actor attached to the ```GameObject```

### How Actor Works ###


 * Actor checks if the scene initialized from the Starter class. If not it holds Enable and Setup methods until initialization; this helps to be sure that Starter class initialize all Processing and Framework scene related scripts first.
* Actor creates a new entity and add ```ComponentObject``` to it.
* Setup method is triggered. All custom stuff and components developer wish to add goes to the Setup method.
* When an Actor enabled, Deploy method is triggered. The entity goes to all entity groups that are interested in this particular entity.
* When an Actor is disabled it notifies all entity groups that holds this particular entity that it's out.
* When an Actor destroyed, it notifies all entity groups that holds this particular entity that it's out and free the entity ID.


