# Entity Composer

Композитор сущности ( Entity Composer ) - специальная вспомогательная структура для настройки/создания новой сущности.

При создании композитора через конструктор нужно передать количество компонентов которое мы собираемся добавлять сущности.
Сущность будет автоматически создана при инициализации композитора.

* `Add<T>` создает и возвращает указанный нами компонент. Дальше мы можем настроить его переменные.
* `EntityComposer.id` - индекс сущности.
* `EntityComposer.Deploy()` - метод который информирует все группы о том что сущность с компонентами была добавлена.

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
            cObject.obj = gameObject;
            cObject.transform = transform;
            
            composer.Deploy();
     
        }
    }
```

_В чем преимущество такого подхода? Почему просто не добавить компонент сущности напрямую?_

* Хотя добавлять компоненты напрямую можно это не гарантирует что ваша сущность будет корректно настроена.
Добавив таким образом компонент, группы сразу начнут с ним работать - раньше чем вы обновите его переменные.

* При использовании композитора вы сделаете только одну проверку на добавление через метод Deploy. В случае с ручным добавлением вы сделаете столько проверок сколько добавили компонентов и тэгов. Например из примера выше вышло бы две проверки.

## Редактирование сущности

Для редактирования уже существующей сущности нам нужно получить ее индекс и передать ее в конструктор **первым** аргументом. Второй аргумент по прежнему кол-во компонентов которое мы хотим добавить.

```csharp
public class Test : MonoBehaviour
    {
        void Awake()
        {
            var entity = 1; // некая сущность с индексом 1
            var composer = new EntityComposer(entity,2);
            
            var cMotion  = composer.Add<ComponentMotion>();
            cMotion.direction = Vector2.right;
 
            var cLight = composer.Add<ComponentLight>();
            cLight.source = new GameObject("Light").AddComponent<Light>();

            composer.Deploy();
     
        }
    }
```

## Добавление тэгов

Если нам нужно передать пару тэгов сущности это делается через метод `Deploy`

```csharp
public class Test : MonoBehaviour
    {
        void Awake()
        {
            var entity = 1; // некая сущность с индексом 1
            var composer = new EntityComposer(entity,2);
            
            var cMotion  = composer.Add<ComponentMotion>();
            cMotion.direction = Vector2.right;
 
            var cLight = composer.Add<ComponentLight>();
            cLight.source = new GameObject("Light").AddComponent<Light>();

            composer.Deploy(Tag.GroupPiece, Tag.GroupDestructable);
     
        }
    }
```
