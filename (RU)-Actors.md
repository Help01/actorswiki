## Акторы
Актор (Actor) служит мостом между фреймворком и физическим объектом в Unity. Задача актора визуально настроить базовые компоненты сущности, инициализировать сущность. 

### Как создавать?

> Акторы самостоятельно создают сущность.

#### Вариант 1
Вернет нового актора и назначит ему новый игровой объект (gameobject). Объект будет получен из папки Resources по имени.
```csharp
public void Setup()
{
   Actor a = Actor.Create("Obj Alpaca");
}
```
#### Вариант 2
Вернет нового актора и назначит ему новый игровой объект (gameobject). Объект будет получен из предоставленного разработчиком префаба (prefab).
```csharp
public GameObject prefabFluffyUnicorn;
public void Setup()
{    
     Actor a = Actor.Create(prefabFluffyUnicorn);
}
```
#### Обращение к сущности из актора

```csharp
public GameObject prefabFluffyUnicorn;
public void Setup()
{    
     ref ent e = ref Actor.Create(prefabFluffyUnicorn).entity;
}
```
 
### Как уничтожать Акторов
Напрямую уничтожение актора не потребуется. Как правило в фреймворке вы будете иметь дело с сущностями. Для уничтожения вам нужно вызвать ```entity.Release()``` метод. 

```csharp
           Actor a = Actor.Create(prefabFluffyUnicorn);
           a.entity.Release();
```

_На заметку_
> Любое логические действие уничтожения и удаления в фреймворке идет через методы `Release()`- если видите этот метод то он 100% связан с уничтожением.

#### Что происходит когда Актор уничтожен?

* С уничтожением актора его сущность сбрасывается в область зарезервированных сущностей. Все новые сущности сначала попытаются получить индекс оттуда.
* Все компоненты уничтоженной сущности помечаются как неактивные. Сущность выходит из обращения всех групп. При этом отрабатываются все события выхода из групп если такие присутстввуют. 
* Если актор был использован в пуле объектов то его GameObject будет деактивирован для последующего использования. 

> Уничтожение является отложенным действием и произойдет на следующий кадр.

### Настройка актора

#### Вариант 1 - наследование от актора
В этом варианте создается новый класс который наследуется от класса Actor. Это самый простой способ работы с акторами. Добавляются поля компонентов которые нужно передать сущности. 

```csharp
public class ActorUnit : Actor
{
  [FoldoutGroup("Setup")]
  public ComponentCollider componetCollider;
  [FoldoutGroup("Setup")]
  public ComponentObject componentObject;
}
```

Дальше используется метод ```Setup()``` для передачи компонентов сущности.

```csharp
public class ActorUnit : Actor
{

  [FoldoutGroup("Setup")]
  public ComponentCollider componetCollider;
  [FoldoutGroup("Setup")]
  public ComponentObject componentObject;

  protected override void Setup()
  {
     // добавит компонент
     Add(componetCollider);
  
     // добавит компонент, но не инициализирует его в системах. Компонент будет оставаться невидимым пока его не добавят через Add
     // Используется когда важно полностью настроить сущность, но работа некоторых компонентов ненужна на старте. 
     AddLater(componentObject);
    
     // создаст/добавит компонент по типу
     Add<ComponentMotion>();
    
     // добавит тэг
     Add(Tag.Alpaca);
  }
 
}
```

#### Вариант 2 - модели

##### Cпособ 1
Вернет нового актора и назначит ему новый игровой объект (gameobject). Объект будет получен из папки Resources по имени.
Настройки для актора будут взяты из ```Models.Bunny```
```csharp
Actor.Create("Obj Unit", Models.Bunny);
Actor.Create("Obj Sprite", Models.Bunny);
```

##### Cпособ 2
Вернет актора и назначит ему игровой объект найденный на сцене.  
Настройки для актора будут взяты из ```Models.Bunny```
```csharp
var obj = GameObject.Find("Hollow Soul");
Actor.CreateFor(obj, Models.Bunny);
```

[Настройка моделей](https://github.com/dimmpixeye/ecs/wiki/(RU)-Models)