#  Reactive components
In the framework, you can _subscribe_ to change a variable and perform an action at the time of the change.

### When you need it?
Basically it is convenient for working with the interfaces that often need to observe some parameters. For example, the health of the main character.

### How to subscribe?
#### The 1st way.
Using the ```this.Value Change ()``` method to create and return an abstract entity with an observer.
```csharp
	sealed class ProcessorTesting: Processor, ITick
	{

		public float hp;
		public ent observer;
		
		public ProcessorTesting()
		{
                        //where "source" = the object from which the variable is taken
			observer = this.ValueChange(source => hp, HandleChangeHP);
		}

                // The method that will work when the change occurred. Returns the changed value.
		void HandleChangeHP(float val) 
                {
                         Debug.Log($"HP IS {val}"); 
                }
          }

```
#### The 2nd way.
It is not necessary to create a new entity, you can connect the observer to an existing one. Sometimes it is convenient when we don’t want to bother with the destruction of the observer, because it will be destroyed along with the entity of which we added it. 
```csharp
	sealed class ProcessorTesting: Processor, ITick
	{

		public float hp;
		public ent observer;
		
		public ProcessorTesting()
		{
                         // imagine that this is a player
                        observer = Entity.Create();
                        // where "source" = the object from which the variable is taken
			this.ValueChange(source => hp, HandleChangeHP, observer);
		}

                // The method that will work when the change occurred. Returns the changed value.
		void HandleChangeHP(float val) 
                {
                         Debug.Log($"HP IS {val}"); 
                }
          }

```
> Note,  
You can connect more than one observer to an entity. To do this, it is enough to continue to transfer to the -```this.Value Change ()``` method the same entity.

### How to unsubscribe?
Because of observers depend on entities, they will automatically be destroyed along with the entities.
The method ```this.ValueChange()```returns a new entity with an observer and cashed it, you can call it with the help of the method ```Release```

```csharp
        public ent observer;
	public void Tick(float delta)
		{
			if (Input.GetKeyDown(KeyCode.A))
			{
				observer.Release();
			}
		}
```
If you just need to clear the stack of observers of the entity without destroying it, you can use
```observer.ReleaseObservers();```
```csharp
        public ent observer;
	public void Tick(float delta)
		{
			if (Input.GetKeyDown(KeyCode.A))
			{
				observer.ReleaseObservers();
			}
		}
```