Актуально (28.09.2019)

## Component
Компонент - это кирпич игры описывающий некий набор переменных. Наборы компонентов формируют контекст сущности и хранят ее состояние. Во фреймворке компоненты часто пишутся с атрибутом _[Serializable]_ для того чтобы редактировать поля в инспекторе Unity.  
>Помимо самого класса компонента данных обязательно нужно объявить класс хранилища компонентов (зона хелперов в примере).  

>Начинайте имя компонента с `Component`, в противном случае ломается debug-режим фреймворка (переключить в release можно с помощью пункта меню Actors в Unity)
```csharp
using System;
using Pixeye.Actors;

namespace Pixeye.Source
{
	[Serializable]
	public class ComponentHealth
	{
		public int val;
	}

	#region HELPERS

	public static partial class Component
	{
		public const string Health = "Pixeye.Source.ComponentHealth";

		internal static ref ComponentHealth ComponentHealth(in this ent entity)
			=> ref Storage<ComponentHealth>.components[entity.id];
	}

	sealed class StorageComponentHealth : Storage<ComponentHealth>
	{
		public override ComponentHealth Create() => new ComponentHealth();

		public override void Dispose(indexes disposed)
		{
			foreach (int id in disposed)
			{
				ref var component = ref components[id];
				component.val = 0;
			}
		}
	}

	#endregion
}
```

### Как создавать 

#### Шаблон компонента
Где {FullName} - имя компонента. Самым удобным способом будет создать шаблон внутри вашего IDE для быстрого создания компонентов.
```csharp
using Pixeye.Actors;

namespace Pixeye.Source
{
 
    public class ${FullName}
    {
     
    }
      
   #region HELPERS
   public static partial class Component
    {
        public const string $end = "Pixeye.Source.$FullName";
    
		internal static ref ${FullName} ${FullName}(in this ent entity)
		=> ref Storage<${FullName}>.components[entity.id];
    }
    
   sealed class Storage${FullName} : Storage<${FullName}>
     {
	     public override ${FullName} Create() => new ${FullName}();
	     
	    public override void Dispose(indexes disposed)
		{
			foreach (var id in disposed)
			{
				ref var component = ref components[id];
			}
		}
	     
     }
    #endregion
}
```
##### Сниппеты  
<details>
	<summary>Visual Studio</summary>

Shortcut для вызова сниппета - `comp`. После двойного нажатия TAB вам нужно ввести имя компонента (без префикса Component, он уже прописан), после чего вы можете нажать снова TAB и ввести namespace, в котором находится класс компонента данных (или отредактируйте сниппет, исключив это).
```xml
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
  <CodeSnippet Format="1.0.0">
    <Header>
      <Title>Класс компонента данных</Title>
      <Shortcut>comp</Shortcut>
      <Description>Шаблон компонента данных</Description>
      <SnippetTypes>
        <SnippetType>Expansion</SnippetType>
      </SnippetTypes>
    </Header>
    <Snippet>
      <Imports>
        <Import>
          <Namespace>Pixeye.Actors</Namespace>
        </Import>
      </Imports>
      <Declarations>
        <Literal>
          <ID>shortname</ID>
          <ToolTip>Имя компонента без суффиксов</ToolTip>
          <Default>Name</Default>
        </Literal>
        <Literal>
          <ID>namespace</ID>
          <ToolTip>Неймспэйс в котором находится класс компонента данных</ToolTip>
          <Default>Namespace</Default>
        </Literal>
      </Declarations>
      <Code Language="csharp">
        <![CDATA[
        public class Component$shortname$
        {
			$end$
        }
      
   #region HELPERS
   public static partial class Component
    {
     public const string $shortname$ = "$namespace$.Component$shortname$";
		internal static ref Component$shortname$ Component$shortname$(in this ent entity)
		=> ref Storage<Component$shortname$>.components[entity.id];
    }
    
   sealed class Storage$shortname$ : Storage<Component$shortname$>
     {
	     public override Component$shortname$ Create() => new Component$shortname$();
	     
	    public override void Dispose(indexes disposed)
		  {
			  foreach (var id in disposed)
			  {
				ref var component = ref components[id];
				//dispose (reset) logic
			  }
		  }
      
     }
    #endregion
	]]>
      </Code>
    </Snippet>
  </CodeSnippet>
</CodeSnippets>
```  

Чтобы использовать сниппет, разместите файл с расширением **.snippet** по расположению **%USERPROFILE%\Documents\Visual Studio 2019\Code Snippets\Visual C#\My Code Snippets**.  

[Руководство](https://docs.microsoft.com/ru-ru/visualstudio/ide/walkthrough-creating-a-code-snippet?view=vs-2019 "Официальное руководство с msdn") по сниппетам. [(доп.)](https://professorweb.ru/my/programs/visual-studio/level2/2_17.php "Руководство на DoctorWeb")  

</details>

<details>
<summary>Live Template in Rider</summary>  
	
Перейдите в `Settings -> Editor -> Live template`. Выберите C#, нажмите `New template` (справа ввверху). Заполните поле кода сниппета:
```csharp
    public class Component$shortname$
     {
			$end$
     }
      
   #region HELPERS
   public static partial class Component
    {
     public const string $shortname$ = "$namespace$.Component$shortname$";
		internal static ref Component$shortname$ Component$shortname$(in this Pixeye.Actors.ent entity)
		=> ref Storage<Component$shortname$>.components[entity.id];
    }
    
   sealed class Storage$shortname$ : Storage<Component$shortname$>
     {
	     public override Component$shortname$ Create() => new Component$shortname$();
	     
	     public override void Dispose(Pixeye.Actors.indexes disposed)
		  {
			  foreach (var id in disposed)
			  {
				ref var component = ref components[id];
				//dispose (reset) logic
			  }
		  }
      
     }
    #endregion
```
Настройте сниппет:  

![Настройки](https://i.gyazo.com/42b7d5c53fadc9ffcd556cd28fd50800.png)  

Расположите переменные в удобном для вас порядке их ввода (как на картинке). Также нажмите `change macro` и выберите *containing type name* для переменной `shortname`:  

![Порядок](https://i.gyazo.com/b2f99435db63fc9f095b35a2827599a9.png)  

Настройте `change macro` для `namespace`:  

![macro](https://i.gyazo.com/891d22af20b4f0d5dc8e9a01db0ec853.png)  
[Руководство](https://www.jetbrains.com/help/idea/creating-and-editing-live-templates.html "live templates for rider") по сниппетам.  

</details>  

##### File Templates
<details>
	<summary>Rider</summary>  
	
Откройте настройки: `Editor | File Templates | C#`. Нажмите создать, заполните поле для кода:  

```csharp
	 namespace $namespace$
 {
 public class $name$
     {
     
     }
      
   #region HELPERS
   public static partial class Component
    {
     public const string $end$ = "$namespace$.$name$";
		internal static ref $name$ $name$(in this Pixeye.Actors.ent entity)
		=> ref Storage<$name$>.components[entity.id];
    }
    
   sealed class Storage$name$ : Storage<$name$>
     {
	     public override $name$ Create() => new $name$();
	     
	     public override void Dispose(Pixeye.Actors.indexes disposed)
		  {
			  foreach (var id in disposed)
			  {
				ref var component = ref components[id];
				//dispose (reset) logic
			  }
		  }
     }
    #endregion
}  
```
Настройте:  

![Меню настроек шаблона](https://i.gyazo.com/643df99b2f273dd3edfe50b6ed866307.png)  

Далее в `Edit variables` расположите переменные в нужном порядке и настройте для них макросы:  

![Настройка первой переменной](https://i.gyazo.com/794d0e4413c62e8cba437f9bf065754a.png)
![Второй переменной](https://i.gyazo.com/24bedbb453962151c12eee591b10237f.png)
![Третьей переменной](https://i.gyazo.com/1f06ca2fd61c44b5f2959e4dc43f2b4f.png)  


</details>

#### Через юнити
Create->Actors->Add->Component

[![Image from Gyazo](https://i.gyazo.com/29e7fd2c1f07c7ff8104fa6d1dc7ca45.gif)](https://gyazo.com/29e7fd2c1f07c7ff8104fa6d1dc7ca45)

## Как обращаться к компонентам
Проведу аналогию с Unity: раньше мы делали что-то вроде `gameobject.GetComponent<{НАЗВАНИЕ}>()` - `gameobject` выступал в роли сущности. Теперь `gameobject` сам является лишь частью сущности.  

Через сущность мы можем попытаться обратиться к любому компоненту:
```csharp
   var entity = 1;  // берем первую сущность на сцене
   //этот метод-расширение определен в хелпере
   var cObject = entity.ComponentObject(); // напрямую берем ComponentObject
```
Проверить наличие компонента у сущности:
```csharp
   var entity = 1;  // берем первую сущность на сцене
   if (entity.Has<ComponentObject>()) {} // проверяем наличие
```
Одновременно достать компонент и проверить его наличие.
```csharp
   var entity = 1;  // берем первую сущность на сцене
   ComponentObject cObject;  
   if (entity.Get<ComponentObject>(out cObject)) {
   // do stuff with cObject
   } 
```

Одновременно достать несколько компонентов и проверить их наличие.
 
```csharp
ComponentAnimation cAnimation;
ComponentMotion    cMotion;

if (entity.Get(out cAnimation, out cMotion))
{
				 
}
```


