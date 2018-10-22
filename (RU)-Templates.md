## Шаблоны

Шаблоны - это SO ( [Scriptable Object](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/scriptable-objects) ) отвечающие за настройки игровых систем, базовые параметры для компонентов и объектов. Шаблоны так же полезно использовать как скрипты благодаря возможности писать логику.  

## Создание шаблона

1) Выбираем папку **[1]Source** или ту папку что отвечает за хранение игрового кода.
2) Tools->Actors->Add->Template 
3) В выбранной папке будет создан новый файл. Он будет сразу выбран и ему нужно будет задать имя. Правило названия: TemplateName `например TemplateWeapon` 
4) Если все было сделано правильно у вас получится файл такого содержания:
```csharp
using UnityEngine;
namespace Homebrew
{
    [CreateAssetMenu(fileName = "Template Test", menuName = "Actors/Templates/Test")]
    public class TemplateTest : Template
    {
    }
}
```
5) В папке **[2]Content/Resources/Data** или другой папке отвечаюзей за ассеты ПКМ _(правый клик мыши)_ -> Create -> Actors -> Templates -> Название вашего шаблона.

[![If,kjys](https://i.gyazo.com/df264fb0fe9db9aa9dc12e49642cdf23.png)](https://gyazo.com/df264fb0fe9db9aa9dc12e49642cdf23)


## Пример использования
```csharp
    [CreateAssetMenu(fileName = "sample_weapon", menuName = "Actors/Samples/Weapon")]
         public class TemplateWeapon : Template
         {
             [FoldoutGroup("Ammo")] public GameObject ammoPrefab;
             [FoldoutGroup("Ammo")] public TemplateAmmo AmmoTemplate;
     
             [FoldoutGroup("Setup")] public GameObject weaponThrowable;
             [FoldoutGroup("Setup")] public GameObject weaponShootFx;
             [FoldoutGroup("Setup")] public GameObject weaponShootLightFx;
     
             [FoldoutGroup("Setup")] public string weaponName;
             [FoldoutGroup("Setup")] public int shots = 1;
             [FoldoutGroup("Setup")] public int clipSize = 6;
             [FoldoutGroup("Setup")] public float kickback;
             [FoldoutGroup("Setup")] public float angle;
             [FoldoutGroup("Setup")] public float delay;

             void HandleShoot(int entity);

          }
```

[![Шаблон](https://i.gyazo.com/c3f656eb313c06501d59145ebeecb2a3.gif)](https://gyazo.com/c3f656eb313c06501d59145ebeecb2a3)

Теперь можно создать несколько ассетов с настройками для оружий. Шаблоны идеальные помощники для констант объектов которые меняться не будут. Даже в случае необходимости простых линейных апгрейдов лучше сделать новый ассет вроде Weapon 1 Upgrade. 


Далее добавляем шаблон к компоненту:

```csharp
using System;
using Homebrew;
using UnityEngine;


[Serializable]
public class ComponentActiveWeapon : IComponent
{
    public TemplateWeapon current;
    public Transform muzzle;
    public int clip;
    public float rate;
}
```

Компоненты хранят видоизменяемые параметры, например остаток патрон, прицел, а шаблон макс. запас обоймы, тип патрон, отдачу, кол-во выстрелов и так далее.

Логику самого выстрела мы можем сделать в шаблоне через метод Shoot передав единственным аргументом индекс сущности.
Все необходимые компоненты для работы мы сможем легко получить через этот индекс. 

Особенности использования SO объектов как скриптов:

* SO не принадлежат объектам и не могут хранить локальные/временные переменные. 
* Не рекомендуется менять переменные SO - это приведет к непредвиденным обстоятельствам _например в редакторе значения перезапишутся и останутся с сессии_
* Лучше всего относиться к методам SO как к глобальным скриптам/функциям для выполнения какой-то конкретной задачи передавая все необходимые аргументы в метод.
* Если вы вызываете метод из Template и передаете индекс сущности чтобы получить нужные компоненты, убедитесь, что все эти компоненты участвуют в группе обработчика откуда вы вызвали Template. Таким образом можно гарантировать что на момент обращения все эти компоненты действительно находились у сущности.

Использование SO для логики - лишь один из инструментов для решения задач. Не стоит им злоупотреблять.

