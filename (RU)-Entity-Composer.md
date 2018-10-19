# Entity Composer

Композитор сущности ( Entity Composer ) - специальная вспомогательная структура для настройки/создания новой сущности.

## Создание сущности

```csharp
public class Test : MonoBehaviour
    {
        void Awake()
        {
            var composer = new EntityComposer(2);
            
            var cMotion  = composer.Add<ComponentMotion>();
            cMotion.direction = Vector2.right;
            
            var cObject = composer.Add<ComponentObject>();
            cObject.entity = composer.id;
            cObject.obj = gameObject;
            cObject.transform = transform;
            
            composer.Deploy();
     
        }
    }
```

При создании композитора через конструктор нужно передать сколько компонентов мы собираемся добавлять сущности.
Сущность будет автоматически создана при инициализации композитора.

* `Add<T>` создает и возвращает указанный нами компонент. Дальше мы можем настроить его переменные.
* `EntityComposer.id` - индекс сущности.
* `EntityComposer.Deploy()` - метод который информирует все группы о том что сущность с компонентами была добавлена.

