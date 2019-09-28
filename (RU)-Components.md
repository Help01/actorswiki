Актуально (28.09.2019)

## Component
Компонент - это кирпич игры описывающий некий набор переменных. Наборы компонентов формируют контекст сущности и хранят ее состояние. Во фреймворке компоненты часто пишутся с атрибутом _[Serializable]_ для того чтобы редактировать поля в инспекторе Unity.

```csharp
using System;
using Pixeye.Actors;

namespace Pixeye.Source
{
	[Serializable]
	public class ComponentHealth
	{
		public int val;
	}

	#region HELPERS

	public static partial class Component
	{
		public const string Health = "Pixeye.Source.ComponentHealth";

		internal static ref ComponentHealth ComponentHealth(in this ent entity)
			=> ref Storage<ComponentHealth>.components[entity.id];
	}

	sealed class StorageComponentHealth : Storage<ComponentHealth>
	{
		public override ComponentHealth Create() => new ComponentHealth();

		public override void Dispose(indexes disposed)
		{
			foreach (int id in disposed)
			{
				ref var component = ref components[id];
				component.val = 0;
			}
		}
	}

	#endregion
}
```

### Как создавать 

#### Шаблон компонента
Где {FullName} - имя компонента. Самым удобным способом будет создать шаблон внутри вашего IDE для быстрого создания компонентов.
```csharp
using Pixeye.Actors;

namespace Pixeye.Source
{
 
    public class ${FullName}
    {
     
    }
      
   #region HELPERS
   public static partial class Component
    {
        public const string $end = "Pixeye.Source.$FullName";
    
		internal static ref ${FullName} ${FullName}(in this ent entity)
		=> ref Storage<${FullName}>.components[entity.id];
    }
    
   sealed class Storage${FullName} : Storage<${FullName}>
     {
	     public override ${FullName} Create() => new ${FullName}();
	     
	    public override void Dispose(indexes disposed)
		{
			foreach (var id in disposed)
			{
				ref var component = ref components[id];
			}
		}
	     
     }
    #endregion
}
```
 
#### Через юнити
Create->Actors->Add->Component

[![Image from Gyazo](https://i.gyazo.com/29e7fd2c1f07c7ff8104fa6d1dc7ca45.gif)](https://gyazo.com/29e7fd2c1f07c7ff8104fa6d1dc7ca45)

## Как обращаться к компонентам
Проведу аналогию с Unity: раньше мы делали что-то вроде `gameobject.GetComponent<{НАЗВАНИЕ}>()` - `gameobject` выступал в роли сущности. Теперь `gameobject` сам является лишь частью сущности.  

Через сущность мы можем попытаться обратиться к любому компоненту:
```csharp
   var entity = 1;  // берем первую сущность на сцене
   var cObject = entity.ComponentObject(); // напрямую берем ComponentObject
```
Проверить наличие компонента у сущности:
```csharp
   var entity = 1;  // берем первую сущность на сцене
   if (entity.Has<ComponentObject>()) {} // проверяем наличие
```
Одновременно достать компонент и проверить его наличие.
```csharp
   var entity = 1;  // берем первую сущность на сцене
   ComponentObject cObject;  
   if (entity.Get<ComponentObject>(out cObject)) {
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


