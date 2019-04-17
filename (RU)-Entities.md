## Сущности

Сущность (entity) - хранит информацию об объекте. Каждой сущности присваивается уникальный индекс(ID). 

### Как создавать?

#### Вариант 1
Вернет новую сущность.
```csharp
public void Setup()
{
    ent e = Entity.Create();
}
```
#### Вариант 2
Вернет новую сущность и назначит ей новый игровой объект (gameobject). Объект будет получен из папки Resources по имени.
```csharp
public void Setup()
{    
     ent e = Entity.Create("Obj Fluffy Unicorn");
}
```
#### Вариант 3
Вернет новую сущность и назначит ей новый игровой объект (gameobject). Объект будет получен из предоставленного разработчиком префаба (prefab).
```csharp
public GameObject prefabFluffyUnicorn;
public void Setup()
{    
     ent e = Entity.Create(prefabFluffyUnicorn);
}
```

### Кеширование созданного объекта в пул

Игровые объекты созданные вместе с сущностью можно разместить в пуле. Для этого достаточно указать дополнительный аргумент при создании сущности.
```csharp
public void Setup()
{    
     // помечаем что объект из пула
     ent e = Entity.Create("Obj Fluffy Unicorn", true);
}
```
Краткое описание что такое [объектный пул](https://ru.wikipedia.org/wiki/%D0%9E%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D1%8B%D0%B9_%D0%BF%D1%83%D0%BB) для незнакомых с этим шаблоном программирования.

### Создание сущности по схеме
Схема (blueprint) - вспомогательный объект фреймворка хранящий настройки компонентов для сборки сущностей.
Например, нам нужно в коде создать кролика.

![Кролик](https://i.gyazo.com/dc5859bf7bffa6954276844ed851afa4.png)

Каждый раз когда нам понадобится кролик мы будем описывать его. По моим представлением кролик это нечто что красиво, умеет прыгать, какать шариками и может быть съеденым. 
```csharp
public void Setup()
{    
     // помечаем что объект из пула
     ent e = Entity.Create("Obj Bunny", true);
     // добавляем компоненты нашему кролику
     e.Add<ComponentCute>();
     e.Add<ComponentJumping>();
     e.Add<ComponentConsumable>();
     e.Add<ComponentCanPoo>();
}
```
Однако описывать так сущности не очень весело. Возможно завтра у нас появится кролик-хищник мстящий людям за съеденных собратьев. Этот кролик больше не какает и вышел на тропу войны.

```csharp
public void Setup()
{    
     // помечаем что объект из пула
     ent e = Entity.Create("Obj Bunny", true);
     // добавляем компоненты нашему кролику-хищнику
     e.Add<ComponentCute>();
     e.Add<ComponentJumping>();
     e.Add<ComponentConsumable>();
     e.Add<ComponentKiller>();
}
```

Обладая схемами этих видов кроликов мы бы могли создавать их таким образом:

```csharp
ent e = Entity.Create("Obj Bunny", Blueprints.Bunny);
```
```csharp
ent e = Entity.Create("Obj Bunny", Blueprints.BunnyPredator);
```

> О том как создавать и настраивать схемы можно будет почитать в отдельном разделе который появится в скором будущем.


### Как уничтожать сущность


