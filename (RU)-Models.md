### Необходимые материалы
- [Сущности](https://github.com/dimmpixeye/ecs/wiki/%28RU%29-Entities)
- [Акторы](https://github.com/dimmpixeye/ecs/wiki/%28RU%29-Actors)

***

### Что такое модель ?

Модель - это один из способов настройки сущности или актора из кода. По своему назначению напоминает схемы (blueprints).
> На заметку: в фреймворке нет жесткого требования к названию этого шаблона проектирования.

### Как создавать ?

* Создайте static class Models

```csharp
public static class Models
{

}
```
* Добавьте статик метод с названием описывающим вашу модель сущности.
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
Логически блок кода модели можно разделить на несколько частей:
- Первыми добавляются компоненты чья настройка не нужна.
- Далее идет список компонентов требующих настроек в рамках модели.
- После идет настройка объявленных компонентов
- В самом конце добавляются необходимые тэги.

#### Зачем EntityComposer
Методы добавления компонентов являются отложенными действиями. Это значит, что добавление в системы произойдет только на следующий кадр. EntityComposer добавляет все компоненты через одну операцию. 

### Как использовать?

#### Cпособ 1
Вернет нового актора и назначит ему новый игровой объект (gameobject). Объект будет получен из папки Resources по имени.
Настройки для актора будут взяты из ```Models.Bunny```
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


