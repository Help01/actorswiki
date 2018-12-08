## Обработчики

Обработчики - это название для всех систем, менеджеров, контроллеров и "глобальных" скриптов. 

## Как создавать
Обработчики наследуются от ProcessingBase. 
```csharp
 public class ProcessingDamageble : ProcessingBase
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
		 
		Add<ProcessingInputConnect>();
 
		Add<ProcessingPlayer>();
		Add<ProcessingMotion>();
		Add<ProcessingRender>();
		Add<ProcessingRenderShoot>();
		
		Add<ProcessingMotionBlur>();
		Add<ProcessingCamera>();
         }
}
```
## Дополнительные интерфейсы
Если обработчику нужны апдейты то используются интерфейсы ITick, ITickFixed, ITickLate.

```csharp
public class ProcessingPlayer : ProcessingBase, ITick
```

Для работы с сигналами используется интерфейс IRecieve<Тип передаваемого объекта>

```csharp
public class ProcessingDamageble : ProcessingBase, IReceive<SignalDamage>, ITick
{
	public void HandleSignal(ref SignalDamage arg){}
}
```

## Работа с группами

В 90% случаев обработчики имеют дело с группами сущностей.
Пример:

```csharp 
	public class ProcessingMotion : ProcessingBase, ITick, ITickFixed
	{
		public Group<ComponentMotion, ComponentObject> groupMotion;

                public void TickFixed()
		{
			foreach (var entity in groupMotion)
			{
				var cMotion = entity.ComponentMotion();
				var cObject = entity.ComponentObject();
				cObject.transform.GetComponent<Rigidbody2D>().MovePosition(cMotion.positionTo);
			}
		}

                	public void Tick()
		{
			var delta      = Time.delta;
                        
                        	var bounds = Game.roomBounds;
			
			foreach (var entity in groupMotion)
			{
				var cMotion = entity.ComponentMotion();
				var cObject = entity.ComponentObject();

				var position = cObject.transform.position;
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
 



