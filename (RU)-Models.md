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