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
	public void HandleSignal(ref SignalDamage arg)
		{}
}

```

