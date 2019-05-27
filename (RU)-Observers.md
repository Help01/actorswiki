# Реактивные компоненты
В фреймворке можно _подписаться_ на изменения переменной и произвести действие в момент изменения.

### Когда это нужно
В основном это удобно для работы с интерфейсами которым часто требуется отслеживать параметры. Например здоровье героя.

### Как подписаться
#### Вариант 1
Используя метод ```this.ValueChange()``` создать и вернуть абстрактную сущность с обозревателем.
```csharp
	sealed class ProcessorTesting: Processor, ITick
	{

		public float hp;
		public ent observer;
		
		public ProcessorTesting()
		{
                        // где source = объект из которого берется переменная
			observer = this.ValueChange(source => hp, HandleChangeHP);
		}

                // метод который должен сработать с изменением. Возвращает изменившиеся значение.
		void HandleChangeHP(float val) 
                {
                         Debug.Log($"HP IS {val}"); 
                }
          }

```
#### Вариант 2
Необязательно создавтаь новую сущность, можно повесить обозревателя на уже существующую. Иногда это удобно когда мы не хотим заморачиваться с уничтожением обозревателя, ведь он будет уничтожен вместе с сущностью которой мы его добавили. 
```csharp
	sealed class ProcessorTesting: Processor, ITick
	{

		public float hp;
		public ent observer;
		
		public ProcessorTesting()
		{
                         // допустим это игрок
                        observer = Entity.Create();
                        // где source = объект из которого берется переменная
			this.ValueChange(source => hp, HandleChangeHP, observer);
		}

                // метод который должен сработать с изменением. Возвращает изменившиеся значение.
		void HandleChangeHP(float val) 
                {
                         Debug.Log($"HP IS {val}"); 
                }
          }

```
> На заметку:  
Можно вешать несколько обозревателей на одну сущность. Для этого достаточно продолжать передавать в метод -```this.ValueChange()``` ту же сущность.

### Как отписаться
Так как обозреватели живут на сущностях то они автоматически будут уничтожены вместе с сущностью. 
Метод ```this.ValueChange()``` возвращает новую сущность с обозревателем и закешировав ее можно потом вызвать ```Release```

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
Если нужно просто очистить стэк обозрения у сущности не уничтожая ее то можно воспользоваться 
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