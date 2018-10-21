## Тэги

Тэги - это целочисленные константы с удобной настройкой в редакторе Unity. Тэги чаще всего используются как фильтры в группах компонентов либо как маркеры, состояния для объекта.

Для использования тэгов создайте статичный класс. Я по умолчанию использую класс Tag.

```csharp
public static class Tag
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
		[TagFilter(typeof(Tag))] public int tag;
	}
```
![Tags](https://i.gyazo.com/525c695244e549bbf31af61a39bdeb00.gif)

### Tag Node
Если вам нужен список тэгов то используйте массив `TagNode[]`

```csharp
[System.Serializable]
public struct TagNode
{
    [TagFilter(typeof(Tag))] public int tag;
 
}

public static partial class Game
{
    public static int[] Convert(this TagNode[] tagNodes)
    {
        var tags = new int[tagNodes.Length];
        for (int i = 0; i < tagNodes.Length; i++)
        {
            tags[i] = tagNodes[i].tag;
        }

        return tags;
    }
}

```
![Tag Node](https://i.gyazo.com/3b1fa2450f2f0c397ab921903b7e9471.gif)	

### Фильтры компонентов
Тэги работают как дополнительный фильтр у групп компонентов. Для этого используйте два атрибута над группой.

* `[GroupBy(params int[] filter)]` - указанные тэги должны быть у сущности.
* `[GroupExclude(params int[] filter)]` - указанные тэги должны отсутствовать у сущности.

Пример - Есть обработчик выстрелов у героя. Обработка происходит в группе WantShoot состоящая из нужных для стрельбы компонентов. Однако мы не хотим чтобы эта группа работала если герой в прыжке, бьет в ближнем бою или перекатывается. 

```csharp
        [GroupExclude(Tag.Jump, Tag.Melee, Tag.Roll)]
        public Group<ComponentWeapon, ComponentPlayer, ComponentObject, ComponentView, ComponentMotion> groupWantShoot;

        public void Tick()
        {
            var delta = Time.DeltaTime;
            // want shoot
            for (int i = 0; i < groupWantShoot.length; i++)
            {
                var cWeapon = groupWantShoot.component[i];
                var cPlayer = groupWantShoot.component2[i];
                var cObject = groupWantShoot.component3[i];
                
                cWeapon.rate = Math.Max(0, cWeapon.rate -= delta);

                if (cPlayer.source.GetButton(DataInputActions.Default.Fire))
                {
                    if (cWeapon.clip == 0)
                        continue;
                    if (cWeapon.rate > 0)
                        continue;

                    Timing.RunCoroutine(cWeapon.current._Shoot(cObject.entity));
                }
            }
        }

```

### Инъекция тэгов через компоненты
Нередко возникает ситуация когда мы хотим указать в исключении для групп некий компонент. Фреймворк поддерживает очень простую фильтрацию по компонентам: по наличию всех указанных. Однако мы можем решить эту задачу через атрибут компонента
`TagRequire` .

Для этого я обычно создаю тэг маску для компонента и называю ее так же как компонент. 
После этого указываю ее в атрибуте как на примере ниже:

```csharp
 [TagRequire(Tag.ComponentInAir)]
    public class ComponentInAir : IComponent
    {
        public float landingSpot = 0;
    } 
```

Каждый раз когда этот компонент будет добавлен сущности - тэг так же передастся. Тэг будет изъят у сущности вместе с ее уничтожением/отключением компонента.

### Добавление/Снятие тэгов
Тэги хранятся в объекте `Tags` , обращения к тэгам идут через методы расширения сущности.

```csharp

// сущность с индексом 0
int entity = 0;

// проверка есть ли у сущность указанный тэг
entity.Has(Tag.Jump);
// проверка есть ли у сущность хотя бы один из указанных тэгов
entity.HasAny(Tag.Jump, Tag.Attack, Tag.Roll);
// добавить указанный тэг
entity.Add(Tag.Jump);
// удалить указанный тэг
entity.Remove(Tag.Jump);
// удалить полностью указанный тэг
entity.RemoveAll(Tag.Jump);

```
Однотипные тэги суммируются. Так, дважды добавив сущности `Tag.Jump` и удалив один раз у нас все еще будет один `Tag.Jump` у сущности. Имейте это в виду при составлении логики. 

 