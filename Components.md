## Components
A component is a game brick with some game variables. In ACTORS framework components often used with _[Serializable]_  attribute for easy editing via unity inspector.

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

 

## Как обращаться к компонентам
Проведу аналогию с Unity: раньше мы делали что-то вроде `gameobject.GetComponent<{НАЗВАНИЕ}>()` - `gameobject` выступал в роли сущности. Теперь `gameobject` сам является лишь частью сущности.  

Через сущность мы можем попытаться обратиться к любому компоненту:
```csharp
   var entity = 0;  // берем первую сущность на сцене
   var cObject = entity.ComponentObject(); // напрямую берем ComponentObject
```
Проверить наличие компонента у сущности:
```csharp
   var entity = 0;  // берем первую сущность на сцене
   if (entity.HasComponentObject()) {} // проверяем наличие
```
Одновременно достать компонент и проверить его наличие.
```csharp
   var entity = 0;  // берем первую сущность на сцене
   ComponentObject cObject;  
   if (entity.TryGetComponentObject(out cObject)) {
   // do stuff with cObject
   } 
```

Одновременно достать несколько компонентов и проверить их наличие.
 
```csharp
ComponentAnimation cAnimation;
ComponentMotion    cMotion;

if (entity.Get(out cAnimation, out cMotion))
{
				 
}
```