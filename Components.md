## Component
A component is a game brick with some game variables. In ACTORS framework components often used with _[Serializable]_  attribute for easy editing via unity inspector. You don't put logic inside of components.
 _ You can use some initialization code if it's needed. _
## Component template
Where {NAME} - is the name of a component. 
The most convenient way will be making template inside of your IDE for fast generating.
 
```csharp
namespace Homebrew
{
    public class Component{NAME} : IComponent
{
   // body
}

  public static  class ExtenstionComponent{NAME}
    {
        public static Component{NAME} Component{NAME}(this int entity)
        {
            return Storage<Component{NAME}>.Instance.components[entity];
        }
 
        public static bool HasComponent{NAME}(this int entity)
        {
            return Storage<Component{NAME}>.Instance.HasComponent(entity);
        }
    }
}
``` 
An example:
```csharp
using System;
namespace Homebrew
{
	[Serializable]
	public class ComponentHealth : IComponent
	{
		public int HP = 1;
		public int HPMax = 5;
	}

	public static class ExtensionComponentHealth
	{
		public static ComponentHealth ComponentHealth(this int entity) { return Storage<ComponentHealth>.Instance.components[entity]; }

		public static bool HasComponentHealth(this int entity) { return Storage<ComponentHealth>.Instance.HasComponent(entity); }
	}
}
```

[![Component example](https://i.gyazo.com/da2aa114339d78ce8152f3990eb9499b.gif)](https://gyazo.com/da2aa114339d78ce8152f3990eb9499b) 

## How to get a component from an entity?

```csharp
   var entity = 0;  // entity with id 0
   var cObject = entity.ComponentObject(); // taking ComponentObject without any checks
```
Check if an entity has component:
```csharp
   var entity = 0;  // entity with id 0
   if (entity.HasComponentObject()) {} // return bool
```
Check if an entity has one ore more components and get them.
 ```csharp
ComponentAnimation cAnimation;
ComponentMotion    cMotion;

if (entity.Get(out cAnimation, out cMotion))
{
	// do something			 
}
```

## How to add a component to an entity?
Components can be added via Actor class in the setup method.
```csharp
public class ActorPlayer: Actor
	{
		[FoldoutGroup("Setup")]
		public ComponentHealth componentHealth;
                [FoldoutGroup("Setup")]
		public ComponentMotion componentMotion;
// this method is used for adding components
protected override void Setup()
	{ 
          // add serialized component variables
             Add(componentHealth);
             Add(componentMotion);
          // add by type
             Add<ComponentPlayer>();
        }
        }
```
Another way of adding components is using EntityComposer.
```csharp
            var composer = new EntityComposer(2);
            
            var cMotion  = composer.Add<ComponentMotion>();
            cMotion.direction = Vector2.right;
            
            var cObject = composer.Add<ComponentObject>();
            cObject.obj = gameObject;
            cObject.transform = transform;
            
            composer.Deploy();
```
_I will add more info about how to add components and work with actor/entity composer._ 

## Interface ISetup
Use ```ISetup``` when you want to add initialization logic to your component.
Setup method will be automatically handled once component added to an entity.
```csharp
[Serializable]
	public class ComponentHealth : IComponent, ISetup
	{
		public int HP = 1;
		public int HPMax = 5;
		public Animator animator;

		public void Setup(int entity)
		{
			animator = entity.GameObject().GetComponent<Animator>(); 	
		}
	}
```

По умолчанию с фреймворком идет **один** компонент: ComponentObject

```csharp
namespace Homebrew
{
    public class ComponentObject : IComponent, IDisposable
    {
        public Transform transform;
        public GameObject obj;

        internal Dictionary<int, Transform> cachedTransforms;

        public void Dispose()
        {
            transform = null;
            obj = null;
            cachedTransforms.Clear();
        }
    }

    public static partial class Game
    {
        public static ComponentObject ComponentObject(this int entity)
        {
            return Storage<ComponentObject>.Instance.components[entity];
        }

        public static bool TryGetComponentObject(this int entity, out ComponentObject component)
        {
            component = Storage<ComponentObject>.Instance.TryGet(entity);
            return component != null;
        }

        public static bool HasComponentObject(this int entity)
        {
            return Storage<ComponentObject>.Instance.HasComponent(entity);
        }
    }
}
```

Этот компонент самостоятельно добавляется при наличии Actor класса на объекте. Компонент отвечает за связь сущности с GO 
`(GameObject)`

 
 