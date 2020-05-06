Актуально (05.05.2020)

## Component
Компонент - это кирпич игры описывающий некий набор переменных. Наборы компонентов формируют контекст сущности и хранят ее состояние. Во фреймворке компоненты часто пишутся с атрибутом _[Serializable]_ для того чтобы редактировать поля в инспекторе Unity.  
>Помимо самого класса компонента данных обязательно нужно объявить класс хранилища компонентов (зона хелперов в примере).  

>Начинайте имя компонента с `Component`, в противном случае ломается debug-режим фреймворка (если для вас это не подходит, то переключить в release можно с помощью пункта меню Actors в Unity)
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

<details>
	<summary>VS Code</summary>  
	
Shortcut для вызова сниппета - `comp`. После чего нажатием TAB или мышкой выбираем его, вводим имя компонента (без префикса Component, он уже прописан) - готово.  
Настройка сниппета: `File>Preferences>User Snippets` выбираем `csharp`, затем вставляем этот "код"(разметку json) в файл. Форматирование уже заботливо выполнено мной, но если вам не нравится перенос строки, то: SHIFT+ALT+F.  
>P.S. это глобальный файл для всех C# сниппетов
```json
	"Component": {
		"prefix": "сomp",
		"body": [
			"using System;",
			"using Pixeye.Actors;",
			"namespace Pixeye.Source",
			"{",
				"\t[Serializable]",
				"\tpublic class Component${1:FullName}",
				"\t{",
				"\t\t//Data",
				"\t}",

			"#region HELPERS",
			"\tpublic static partial class Component",
			"\t{",
				"\t\tpublic const string ${1:FullName} = \"Pixeye.Source.Component${1:FullName}\";",

				"\t\tinternal static ref Component${1:FullName} Component${1:FullName}(in this ent entity) \n\t\t\t\t=> ref Storage<Component${1:FullName}>.components[entity.id];",
			"\t}",

			"\tsealed class StorageComponent${1:FullName} : Storage<Component${1:FullName}>",
			"\t{",
				"\t\tpublic override Component${1:FullName} Create() => new Component${1:FullName}();",
				"\t\tpublic override void Dispose(indexes disposed)",
				"\t\t{",
					"\t\t\tforeach (var id in disposed)",
					"\t\t\t{",
						"\t\t\t\tref var component = ref components[id];",
					"\t\t\t}",
				"\t\t}",

			"\t}",

			"#endregion",


			"}"
		],
		"description": "New Actors Component with Helper"
	}
```

</details>

##### File Templates
<details>
	<summary>Rider</summary>  

Воспользуйтесь этим меню:  
![Меню Code Templates](https://i.gyazo.com/a562d8ff8f30bdf2763c5e9efbefbe96.png)  

Создайте новый шаблон:  
```csharp
#set($FullName = ${NAME}) 
#set($end = $FullName.substring(9))
#set($component = $FullName.substring(1))


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
    
        internal static ref ${FullName} c$component(in this ent entity)
        => ref Storage<${FullName}>.components[entity.id];
    }
    
   sealed class Storage${FullName} : Storage<${FullName}>
     {
         public override ${FullName} Create() => new ${FullName}();
         
        public override void Dispose(indexes disposed)
        {
            foreach (var index in disposed)
            {
                ref var component = ref components[index];
            }
        }
         
     }
    #endregion
}
```  
[Руководство](https://www.jetbrains.com/help/rider/Using_File_and_Code_Templates.html#) по File Templates.
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

<details>
	<summary><b>Advanced</b></summary>  

### Создание компонентов без хелперов

Если мы не хотим использовать хелперы (из-за проблем с переименованием или по религиозным соображениям), то можем воспользоваться атрибутом `[ActorsComponent]`:
```csharp
using Pixeye.Actors;
using UnityEngine;
 
 namespace Components
 {
     [ActorsComponent]
     public class ComponentClick
     {
         public string Id;
         public Vector2 Point;
     }
 }
```
#### Особенности использования  

**Быстрый доступ из групп к компоненту через метод-расширение сломается**, так как до он был определен в коде хелпера, но теперь:
```csharp
var cClick = entity.ComponentClick()// вызовет ошибку компилляции
```
>Этот метод ничего не мешает написать снова, но тогда остается проблема с переименованием(нужно изменить имя метода-расширения вместе с именем компонента) и порция дополнительного кода.  

Поэтому для доступа к компоненту *из групп* используйте (самый быстрый способ):
```csharp
var cClick = Storage<ComponentClick>.components[entity.id]; 
```
Или напишите универсальные методы для доступа:
```csharp
	public static class Component
	{
		/*Возвращает компонент сущности, не проверяя его наличия.
		 * (группа гарантирует наличие компонента)
		 */
		public static ref T FromGroup<T>(in this ent entity)
		  => ref Storage<T>.components[entity.id];

		/*Возвращает компонент сущности в component-переменной.
		 * Ограничение where T: class существует, чтобы предостеречь
		 * от возврата компонента по значению, если он явл. структурой.
		 * Используйте для компонентов-структур предыдущий метод.
		 */
		public static void FromGroup<T>(in this ent entity, out T component) where T: class
		  => component = Storage<T>.components[entity.id];
	}
```
Тогда обращение к компоненту будет выглядеть так:
```csharp
entity.FromGroup(out ComponentInput inputData);
```

**Создание нового компонента через `entity.Set<T>()` станет дороже**, т.к. будет использоваться реализация `Storage<T>.Create` по умолчанию - `Activator.CreateInstance<T>`. Это медленнее, чем прямой вызов конструктора. В местах первых массовых инициализаций сущностей можно использовать `entity.Set<T>(T component)`:
```csharp
//создаем компонент вручную, минуя дефолтный Activator.CreateInstance<T>()
var cPlayer = new ComponentPlayer();
entity.Set(cPlayer);
```
*Dispose остается не реализованным*, поэтому за сброс значений компонентов сущности после `entity.Release()` отвечаете вы. В `entity.Set<T>()` вам может попасться старый компонент от релизнутой сущности (со старыми данными). Благодаря этому `entity.Set<T>()` не всегда будет использовать `Activator.CreateInstance<T>`, а брать компоненты из пула, что очень дешево. Поэтому применение `entity.Set<T>(T component)` целесообразно, только когда пул пуст (например, в начале игры) и нужно инициализировать большое кол-во сущностей.  

</details>
