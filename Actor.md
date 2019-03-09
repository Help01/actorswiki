The Actor is a bridge class that allows communicating between unity game objects and framework. Most of the time you will inherit from Actors when defining new objects.

The Actor class is *NOT* for defining game logic. You use Actors only to add some components to the entity, to have a connection between Framework and Unity out of the box and for initial setup of the object. 


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

* Now we are ready  to add some components!
```csharp
public class ActorModuleReactor : Actor
    {
       // use the FoldoutGroup attribute to decorate components nicely in   groups in the Unity Inspector
       [FoldoutGroup("Setup")]
        public ComponentStateMachine componentStateMachine;

        [FoldoutGroup("Setup")]
        public ComponentRecycle componentRecycle;

        protected override void Setup()
        {
             Add(componentStateMachine);
             Add(componentRecycle);
             Add(componentDescription);
        }
    }
```
