## Processing

In the framework, all ```System```, ```Manager```, ```Controller``` classes are called ```Processing```.
 
## How to create

Processings are inherited from ```ProcessingBase```

```csharp
 public class ProcessingDamageble: ProcessingBase
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





