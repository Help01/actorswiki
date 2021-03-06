## Шаблон
Акторы - это ECS `Entity Component System` фреймворк со спецификой работы на движке Unity. ECS решает множество задач в написании игр : [локальность данных](https://live13.livejournal.com/474198.html), низкая связанность кода, модульность, потенциально высокая производительность за счет сравнительно простой настройки распараллеливания вычислений. 

Среди разработчиков юнити широко распространена [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) `Object Oriented Design` парадигма однако для оптимальной работы с ECS нужно настраивать образ мышления на [DOD](https://en.wikipedia.org/wiki/Data-oriented_design) 
`Data Oriented Design`. 

Разница подходов выражена в том как мы воспринимаем объекты и работаем с ними: 
### OOP
```csharp
class Spaceship {
 
  Transform body;
  Vector3 position;
  float  speed;

  void Move();
}
```
Для **OOP** характерно думать категорией отдельно взятого объекта. Весь мир состоит из отдельных объектов которые общаются друг с другом посредством своих методов и интерфейсов. Когда мы пишем метод Move в OOP мы подсознательно хотим двигать конкретный кораблик в пространстве. 

Классы объектов могут наследоваться друг от друга расширяя функционал и скрывая базовый функционал и переменные.
Так появляется класс Дредноут наследуемый от Spaceship и выполняющий все те же функции + добавляется стрельба из пушки.

```csharp
class Dreadnought: Spaceship {

  Cannon weapon;   
  void Shoot();

}
```


### DOD
```csharp
class Spaceships {
 
  Transform[] body;
  Vector3[] position;
  float[] speed

  void Move();
}
```
Для **DOD** характерно думать о том как изменяются данные. С точки зрения DOD при описании движения нет разницы двигается ли кораблик или астероид до тех пор пока они живут по одним законам. Так, если в движении участвуют компоненты скорости и тела то сам объект уже не важен. Программисты составляют композиции компонентов обрабатывающихся по единым правилам. 

```csharp
class Entities{
 
  Transform[] body;
  Vector3[] position;
  float[] speed
  Cannon[] weapon;

  void Move();
  void Shoot();
}
```

Одна из задач фреймворка предоставить удобную среду для работы с объектами в стиле DOD.
 
## Component
Компонент - это кирпич игры описывающий некий набор переменных которыми будет обладать сущность.
Во фреймворке компоненты часто пишутся с атрибутом [Serializable] для того чтобы редактировать поля в инспекторе Unity.

Правила компонентов:

* По возможности нужно стараться делать компоненты небольшими. 
* Компонент должен описывать одну задачу в игре. В крайнем случае несколько тесносвязанных задач.
* Компоненты могут наследоваться от других компонентов _если нужно_.
* Компоненты **не должны** содержать никакой игровой логики. 
* Компоненты **могут** содержать методы инициализации, настроек, сброса значений или другие утилитарные методы связанные с компонентом.

_При сомнениях о том расширять функционал компонента или создавать новый как правило лучше создавать новый. Составление композиций компонентов одна из главных задач этого подхода и оптимальные сборки приходят с опытом._

```csharp
[Serializable]
public class ComponentMotion : IComponent{
public float speed;
public Vector3 direction;
}
```
```csharp
[Serializable]
public class ComponentDamageble : IComponent{
public int hp;
}
```
```csharp
[Serializable]
public class ComponentObject : IComponent{
public Transform body;
public GameObject go;
}
```

#### Storage
Для каждого компонента создается хранилище, `Storage<T>` где T - тип компонента.
Например `Storage<ComponentMotion>` ,  Storage хранит массив компонента. Этот массив может только расти. Хранилища всех компонентов растут пропорционально количеству сущностей активных в игре. Так, если уровень состоит из 20 астероидов и 1 корабля игрока то размер хранилищ будет равен 21.

Таким образом каждый из представленных в игре компонентов может быть "доступен" любой из сущности. Чтобы на уровне игровой логики определить какой из компонентов активен используется дополнительный массив bool масок.
 
| Component/Entities    | Entity Asteroid 1  | Entity Asteroid 2  | Entity SpaceShip 3  | Entity Player 4   |
| -------------         |:-------------:    | -----:            |  -----:             | -----:            |          
| Component Motion      | :heavy_check_mark:| :heavy_check_mark:|  :heavy_check_mark: |:heavy_check_mark: |
| Component Health      | :heavy_check_mark:| :heavy_check_mark:|  :heavy_check_mark: |:heavy_check_mark: |
| Component Shoot       | :x:               |   :x:             |  :heavy_check_mark: |:heavy_check_mark: |
| Component Rotate      | :heavy_check_mark:| :heavy_check_mark:|  :heavy_check_mark: |:heavy_check_mark: |
| Component Input       | :x:               |   :x:             |  :x:                |:heavy_check_mark: |
| Component Render      | :heavy_check_mark:| :heavy_check_mark:|  :heavy_check_mark: |:heavy_check_mark: |
| Component Loot        | :heavy_check_mark:| :heavy_check_mark:|  :heavy_check_mark: |:x:                |

Выше приведена таблица компонентов и сущностей астероидов, кораблика и игрока.

При **OOP** парадигме мы бы описывали их как разные классы объектов, а в случае с корабликом в лучшем случае у нас был бы некий AI controller и мы бы добавляли контроллер игрока отдельному кораблю чтобы управлять им.

В **DOD** парадигме мы описываем функционал объекта через композицию компонентов не придавая значения типу объекта.
Мы предполагаем что астероиды это нечто что имеет компонент движения, разворота, здоровья ,отрисовки и лута. 
Но астероиды не умеют стрелять и у них нет управления. 

Мы можем _манипулировать_ набором компонентов изменяя поведение сущностей. Так, мы легко можем активировать компонент Shoot у астероида и он начнет стрелять. А у игрока в бою отбили пушку на корабле, мы убираем компонент Shoot и корабль игрока больше не может стрелять.

## Entity
В описании выше часто применялось слово сущность. Сущность `Entity` - это инкремент, абстракция выраженная числом. 

В **OOP** подходе мы обращаемся к экземплярам классов объекта чтобы получить переменные/методы. 

В **ECS** подходе мы обращаемся к массиву храналища чтобы получить нужный нам компонент "объекта". 
Сущность служит индексом массива компонентов. `ComponentMotion[EntityAsteroidID]` 

_Что происходит когда сущность создается/уничтожается?_

Когда сущность "уничтожается" она уходит в область зарезервированных сущностей. При попытке создания сущности в следующий раз индекс будет взят из этой области.

Если зарезервированная область пуста следующая сущность возьмет новый индекс. Массив хранилищ компонентов будет расширен до значения этого индекса.

## System
В фреймворке системами называются обработчики `Processing`. Выше было сказано, что компоненты не хранят игровой логики. Обработчики - это скрипты поведений которые обрабатывают поступающие компоненты. Обработчики ничего не знают об объектах и не принадлежат отдельно взятому объекту. 

Помимо этого обработчики хорошие кандидаты для "контроллеров, менеджеров" если вам когда либо такие понадобятся.
Как правило Processing наследуется от `ProcessingBase` и добавляется в систему через `Starter` классы. Оттуда обработчики передаются в `Toolbox`- сервис локатор. При переходе с основной сцены на новую все обработчики будут почищены и удалены. Исключение составляют обработчики наследуемые от интерфейса `IKernel`. Однако это уже более сложный вид работы.

```csharp 
public class ProcessingPlayers : ProcessingBase
{
    public Group<ComponentPlayer> groupPlayer;
}
```

```csharp
 public class StarterLevel1 : Starter
{
    protected override void Setup()
    {
        Add<ProcessingCamera>();
        Add<ProcessingPlayers>();
        Add<ProcessingInputConnect>();
        Add<ProcessingShadows>();
        Add<ProcessingAnimations>();
        Add<ProcessingMotion>();
        Add<ProcessingMotionAir>();
    }
}
```

#### GROUP < T >
Обработчики работают с группами. Группы - это композиция компонентов у сущности. Каждая сущность может состоять во множестве групп. Сортировка групп автоматическая. По умолчанию можно указать до 6 компонентов в группе, однако если нужно больше это легко сделать отредактировав класс `GroupBase.cs` Чтобы сущность попала в конкретную группу должны быть выполнены условия : у сущности есть указанные компоненты и фильтры.

```csharp 
public class ProcessingPlayers : ProcessingBase
{
    // у сущности должен быть компонент Motion, Rotate и Render чтобы попасть в эту группу.
    public Group<ComponentMotion, ComponentRotate, ComponentRender> groupMovable;
}
```

За инициализацию группы отвечает метод 	`ProcessingGroupAttributes.Setup(this);` где this - экземляр класса в котором есть группы. Этот метод уже есть в ProcessingBase поэтому разработчику не нужно делать лишних движений. Однако про метод полезно знать если вы захотите иметь группу скажем у юнити компонента наследуемого от `Monobehavior`.

Мы работаем с ссылками на группы: все поля с одинаковой группой в разных классах будут ссылаться на один источник группы.


## Actor
Актор - это вспомогательный ванильный класс для визуальной настройки сущности в инспекторе. Актор так же служит связующим элементом между игровым объектом Unity и фреймворком. Actor наследуется от `Monocached` класса. Как правило в акторах нет смысла писать логики, они служат для сборки сущности. Дальнейшая работа происходит уже в обработчиках. Акторы хороши еще тем, что деактивация или уничтожение объекта с Актором влечет автоматиеческое удаление сущности из групп. При создании объекта с актором автоматически создается сущность. 

```csharp
    public class ActorPlayer : Actor
    {
        [FoldoutGroup("Setup")] public ComponentLight componentLight;
         protected override void Setup()
        {
            Set(Tag.GroupAlly);
            Set(componentLight);    
        }
     }
```

[![Actors](https://i.gyazo.com/7b9ac415341813d64120b42f5bacec81.gif)](https://gyazo.com/7b9ac415341813d64120b42f5bacec81)

## Monocached
MonoCached - базовый класс в фреймворке для юнити компонентов. Наследуется от Monobehavior. Monocached классы отвечают за правильное уничтожение объекта через пулы `Pool`. В зависимости от настройки пула или его отстутсвия объект будет либо деактивирован и использован  в будущем либо полностью уничтожен.