### Required information
- [Entities](https://github.com/dimmpixeye/ecs/wiki/%28RU%29-Entities)
- [Actors](https://github.com/dimmpixeye/ecs/wiki/%28RU%29-Actors)

***

### What is a model?
A model is one of the ways to configure an entity or actor from code. According to its purpose, it resembles blueprints.

>To the note: in the framework there is no strict requirement for the name of this design pattern.

### How to create it?

* Create static class Models

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