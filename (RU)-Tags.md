## Тэги

Тэги - это целочисленные константы с удобной настройкой в редакторе Unity. Тэги чаще всего используются как фильтры в группах компонентов либо как маркеры, состояния для объекта.

Для использования тэгов создайте статичный класс. Я по умолчанию использую класс Tags.

```csharp
public static class Tags
	{
	        // регионами я разделяю для себя тэги различных групп. Так же я указываю индексы которые "резервирую" под эту группу тэгов.
                #region Actions 0 - 1000
                // TagField атрибут позволяет "видеть" эти тэги в инспекторе Unity
		[TagField(categoryName = "Action")] public const int Hit = 0;
		[TagField(categoryName = "Action")] public const int Stun = 1;
                #endregion
	 
	}
```

Для того чтобы выбрать тэг из инспектора Unity используется атрибут `[TagFilter(typeof(Tags))]` где typeof() указывает на класс в котором есть тэги. В нашем примере это класс Tags.

```csharp
public class DemoTags : MonoBehaviour
	{
		// Используем тэг фильтр чтобы увидеть тэг.
		[TagFilter(typeof(Tags))] public int tag;
	}
```

[![Tags](https://i.gyazo.com/525c695244e549bbf31af61a39bdeb00.gif)](https://gyazo.com/525c695244e549bbf31af61a39bdeb00)
	 