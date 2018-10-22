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