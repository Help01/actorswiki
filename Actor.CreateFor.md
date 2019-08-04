# [Actor](Actor).CreateFor
```csharp
public static Actor CreateFor(GameObject obj, bool pooled = false);
public static Actor CreateFor(GameObject obj, ModelComposer model, bool pooled = false);
```
## Параметры
| Параметр | Описание  |
| --------------- |-----|
| obj | Игровой объект, существующий на сцене
| pooled | Будет ли объект использоваться в пуле
| model | Модель для настройки сущности

## Описание
Если на объекте имеется компонент Actor, то активирует его (инициализирует). Иначе добавляет данный компонент и также активирует. 
