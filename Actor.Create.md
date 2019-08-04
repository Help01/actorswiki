# [Actor](Actor).Create
```csharp
public static Actor Create(GameObject prefab, Vector3 position = default, bool pooled = false);
public static Actor Create(GameObject prefab, Transform parent, Vector3 position = default, bool pooled = false);
public static Actor Create(GameObject prefab, ModelComposer model, Vector3 position = default, bool pooled = false);
public static Actor Create(string prefabID, Vector3 position = default, bool pooled = false);
public static Actor Create(string prefabID, ModelComposer model, Vector3 position = default, bool pooled = false);
```
## Параметры
| Параметр | Описание  |
| --------------- |-----|
| prefab | Префаб игрового объекта 
| position | Стартовая позиция объекта 
| pooled | Будет ли объект использоваться в пуле
| parent | Родительский объект
| model | Модель для настройки сущности
| prefabID | Имя префаба

## Описание
Создает игровой объект. Если на префабе имеется компонент Actor, то активирует его (инициализирует). Иначе добавляет данный компонент и также активирует.

**Рекомендуется** на префаб заранее добавлять компонент Actor. 
