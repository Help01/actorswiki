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

to be continued