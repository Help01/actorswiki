## Component
Компонент - это кирпич игры описывающий некий набор переменных которыми будет обладать сущность.
Во фреймворке компоненты часто пишутся с атрибутом _[Serializable]_ для того чтобы редактировать поля в инспекторе Unity.

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

## Шаблон компонента
Где {НАЗВАНИЕ} - имя компонента. Самым удобным способом будет создать шаблон внутри вашего IDE для быстрого создания компонентов.
```csharp
namespace Homebrew
{
    public class Component{НАЗВАНИЕ} : IComponent
{
   // тело
}

  public static partial class Game
    {
        public static Component{НАЗВАНИЕ} Component{НАЗВАНИЕ}(this int entity)
        {
            return Storage<Component{НАЗВАНИЕ}>.Instance.components[entity];
        }

        public static bool TryGetComponent{НАЗВАНИЕ}(this int entity, out Component{НАЗВАНИЕ} component)
        {
            component = Storage<Component{НАЗВАНИЕ}>.Instance.TryGet(entity);
            return component != null;
        }

        public static bool HasComponent{НАЗВАНИЕ}(this int entity)
        {
            return Storage<Component{НАЗВАНИЕ}>.Instance.HasComponent(entity);
        }
    }


}
```

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