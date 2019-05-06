### Required information
- [Entities](https://github.com/dimmpixeye/ecs/wiki/%28RU%29-Entities)
- [Actors](https://github.com/dimmpixeye/ecs/wiki/%28RU%29-Actors)

***

### What is "Model"?
The Model is one of the ways to configure an entity or an actor using code. Its purpose is similar to "Blueprints".

>Note: in the framework there is no strict requirement for the name of this design pattern.

### How to create it?

* Create static class Models

```csharp
public static class Models
{

}
```
* Add a static method with the name which characterizes your model of entity.
```csharp
public static void Bunny(EntityComposer composer)
{
  composer.Add<ComponentAI>();

  var cAbilityJump = composer.Add<ComponentAbilityJump>();
  var cAbilityEat  = composer.Add<ComponentAbilityEat>();

  cAbilityJump.distance = 10f;
  cAbilityEat.foodType  = "Carrot";
	 
  composer.Add(Tag.CanPoo);
}
```
Logically, the model code block can be divided into some parts:
- Components that do not need to be set up are added first.
- Next is a list of components requiring to be set up within the model.
- After you set up the declared components.
- Finally, the tags you need are added.

#### What is Entity Composer for?

The methods of adding components are deferred actions. It means, that they are added to the system in the next frame.  Entity Composer adds all components not in the current operation but in the next one.

### How to use?

#### Method 1...
...returns a new actor and assigns it a new game object (gameobject). The object will be obtained from the Resources folder by name. The settings for the actor will be taken from Models.Bunny
```csharp
Actor.Create("Obj Unit", Models.Bunny);
Actor.Create("Obj Sprite", Models.Bunny);
```

#### Cпособ 2
Вернет актора и назначит ему игровой объект найденный на сцене.  
Настройки для актора будут взяты из ```Models.Bunny```
```csharp
var obj = GameObject.Find("Hollow Soul");
Actor.CreateFor(obj, Models.Bunny);
```

#### Способ 3
Вернет cущность и и назначит ей новый игровой объект (gameobject). Объект будет получен из папки Resources по имени.
Настройки для актора будут взяты из ```Models.Bunny```
```csharp
Entity.Create("Obj Sprite", Models.Bunny);
```

### Зачем использовать ?
* Модели убирают настройку сущности из рабочей области проекта что ведет к повышению читаемости кода.
* Модели универсальны: одну и ту же модель можно применить на разных префабах. 
* Модели добавляют все компоненты одной операцией. _( справедливо и для акторов )_
* Модели могут быть абстрактны и не привязаны к игровому объекту.
* Модели не требуют наследований от класса Actor и размещения унаследованных компонентов на префабах.