In the framework, all ```System```, ```Manager```, ```Controller``` classes are called ```Processing```.
 
## How to create

Processings are inherited from ```Processor```

```csharp
 public class ProcessingDamageble: Processor
 {

 }
```
## Where to initialize
To add processings into game use ```Starter``` classes. 
Starters have special ```Add<T>``` method to add all your processing classes by type.  You can add a processing class manually: to do this use the ```Toolbox.Add<T>()``` method, but this is not recommended. 
```csharp
public class StarterLevel1: Starter
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
## Updates
- Use ```ITick``` interface to add Update to your class.
- Use ```ITickFixed``` interface to add Fixed Update to your class.
- Use ```ITickLate``` interface to add Late Update to your class.

```csharp
public class ProcessingPlayer : Processor, ITick
```
## Signals
Signals allow you to communicate between decoupled parts of the game in Unity3d. You can learn more about signals by following this [link](https://github.com/dimmpixeye/Unity3d-Signals).

- Use ```IRecieve<Type>``` where Type is a struct.

```csharp
public class ProcessingDamageble : Processor, IReceive<SignalDamage>, ITick
{
	public void HandleSignal(ref SignalDamage arg){}
}
```




