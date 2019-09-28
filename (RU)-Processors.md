( Актуально на 28.09.2019 )
## Обработчики

Обработчики - это название для всех систем, менеджеров, контроллеров и "глобальных" скриптов. 

## Как создавать
Обработчики наследуются от Processor. 
```csharp
 public class ProcessorDamageble : Processor
 {

 }
```
## Где объявлять
Как правило обработчики добавляются в Toolbox через Starter класс.

```csharp
public class StarterLevel1 : Starter
{
	protected override void Setup()
	{
		 
		Add<ProcessorInputConnect>();
 
		Add<ProcessorgPlayer>();
		Add<ProcessorgMotion>();
		Add<ProcessorRender>();
		Add<ProcessorRenderShoot>();
		
		Add<ProcessorMotionBlur>();
		Add<ProcessorCamera>();
         }
}
```
## Дополнительные интерфейсы
Если обработчику нужны апдейты то используются интерфейсы ITick, ITickFixed, ITickLate.

```csharp
public class ProcessorPlayer : Processor, ITick
```

Для работы с сигналами используется интерфейс IRecieve<Тип передаваемого объекта>

```csharp
public class ProcessorDamageble : Processor, IReceive<SignalDamage>, ITick
{
	public void HandleSignal(ref SignalDamage arg){}
}
```

Для того чтобы обработчик не был уничтожен тулбоксом при смене сцены используйте интерфейс IKernel
Например:
```csharp
public class ProcessorTimer : ProcessorgBase, IKernel
```
На моей практике ни разу не возникало необходимости в использовании IKernel внутри кода игры. IKernel используется внутри фреймворка для базовых обработчиков так что используя его вы должны понимать зачем вам это нужно.

## Работа с группами

В 90% случаев обработчики имеют дело с группами сущностей.
Пример из игры Cryoshock:

```csharp 
public class ProcessorMotion : Processor, ITick, ITickFixed
{
public Group<ComponentMotion, ComponentRigidbody, ComponentObject> groupMotion;

public override void HandleEvents()
{
	       // do something with added entities. 
	       foreach (ent entity in groupMotion.added)
		{
		   var cRigid  = entity.ComponentRigidBody();
	           var cObject = entity.ComponentObject();
	           cRigid.body = cObject.transform.GetComponent<Rigidbody2D>();
		}
	 
}

public void TickFixed(float delta)
{
      foreach (var entity in groupMotion)
      {
	 var cMotion = entity.ComponentMotion();
	 var cRigid  = entity.ComponentRigidBody();
	 cRigid.body.MovePosition(cMotion.positionTo);
      }
}

public void Tick(float delta)
	{
        var bounds = Game.roomBounds;
			
	foreach (var entity in groupMotion)
	{
		var cMotion = entity.ComponentMotion();
                var cRigid = entity.ComponentRigidBody();

		var position = cRigid.body.transform.position;
		var velocity = cMotion.velocity;


		velocity.x += (0 - velocity.x) * delta / cMotion.drag;
		velocity.y += (0 - velocity.y) * delta / cMotion.drag;

		cMotion.velocity = velocity;

		position.x += (cMotion.velocity.x + cMotion.direction.x * cMotion.speed) * delta;
		position.y += (cMotion.velocity.y + cMotion.direction.y * cMotion.speed) * delta;

		position.x = Mathf.Clamp(position.x, bounds.min.x, bounds.max.x);

		cMotion.positionTo = position;
	}


   }
}
```
Код выше работают для  всех сущностей у которых есть ComponentMotion отвечающий за движение и физику , ComponentObject отвечающий за  объект ( transform и go ), ComponentRigidbody отвечающий за Rigidbody.
 
Наличие этих компонентов может быть у таких сущностей как игрок или монстр. 

### Конструкторы
Обработчики создаются по типу во время игры и не наследуются от классов monobehavior. Чтобы провести инициализацию обработчика используем его конструктор. Например :

```csharp
public ProcessorMotion()
{ 
}
```

### События групп
У каждой группы есть два события:
- Add срабатывает когда новая сущность появляется в группе.
- Remove срабатывает когда некая сущность покидает группу.

Ближайшие аналоги это методы OnEnable/OnDisable у monobehavior класса.
Обрабатываются Все сущности добавленные или удаленные из группы за последний процессор.
Для этого в процессоре вызывается метод HandleEvents.
```csharp
public override void HandleEvents()
{
	       // do something with added entities. 
	       foreach (ent entity in groupMotion.added)
		{
		   var cRigid  = entity.ComponentRigidBody();
	           var cObject = entity.ComponentObject();
	           cRigid.body = cObject.transform.GetComponent<Rigidbody2D>();
		}
	 
}
```

Событие в качестве аргумента использует значение типа int символизирующий сущность.

В примере всегда когда сущность добавляется к группе groupMotion работает код выше. Он назначает реальный ригидбоди объекта своему компоненту. 


### Обработка групп
Все группы являются IEnumerable. Для того чтобы прогнать группу нам достаточно использовать такую конструкцию:
```csharp
foreach (var entity in groupMotion) 
 {
              
 }
```
где entity - сущность участвующая в группе.
Далее, чтобы работать с компонентами достаточно вытащить их из сушности.
```csharp
foreach (var entity in groupInAir)
{
var cInAir    = entity.ComponentInAir();
var cObject   = entity.ComponentObject();
var cMotion   = entity.ComponentMotion();
}
```
Дополнительные проверки на наличие компонента не нужны так как группы фильтруются раньше обработки. При уходе нужного компонента группа уберет сущность. Можно догадаться, что обращаясь к entity мы можем попытаться вытащить любой компонент, даже если его нет в группе, однако это небезопасно и так лучше не делать.

## На заметку

- Группа сущностей имеет фильтр из компонентов. Проверка на соответствие идет слева направо. Если хотя бы одного компонента у сущности нет он не попадает/выбывает из группы. Имеет большой смысл слева размещать самые "ходовые" компоненты чтобы избежать лишних проверок. 

- Используйте в обработке группы только те компоненты которые присутствуют в фильтре. Методы типа TryGet созданы для других случаев ( работы вне групп ) 

- Правилом хорошего тона будет сначала вытащить все нужные компоненты для работы в начале обработки. Переменные компонентов можно начинать с маленькой буквы c. Это позволит визуально отличить компоненты от всех остальных переменных.
 
```csharp
	foreach (var entity in groupInAir)
	{
	var cInAir    = entity.ComponentInAir();
	var cRigid    = entity.ComponentRigidBody();
	var cMotion   = entity.ComponentMotion();
```

- Группы в обработчиках - это ссылки. Физически группы расположены в другом месте. Таким образом два разных обработчика использующие группу с одинаковыми фильтрами не порождают две группы а ссылаются на одну и ту же группу.

- Группы могут быть использованы вне обработчиков. Для этого достаточно в методе инициализации нужного класса указать
```csharp
ProcessorGroups.Setup(this);
```
_Это продвинутая техника и вы должны точно понимать зачем вам это и как работать с этим._

- Группы обрабатываю сущности. Одна и та же сущность может работать в нескольких группах одновременно.
Например первая группа обрабатывает движения, а вторая рендер одного и того же объекта. 

- Имеет большое значение как обработчики следуют друг за другом. Код обрабатывается сверху вниз по очереди. Иногда ситуация не бажная, а просто требует поменять местами процессы.







