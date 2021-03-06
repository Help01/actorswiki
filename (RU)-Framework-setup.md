Актуально для версий с 2018 до 2019.02.24.

## Шаг 1
Скачиваем себе проект фреймворка любым удобным способом. Для более быстрой установки обязательно качаем Install.unitypackage. Его можно найти в [релизах](https://github.com/dimmpixeye/Actors-Unity3d-Framework/releases/).

## Шаг 2
Создаем пустой проект. Удаляем из его ассетов все лишнее и забрасываем Install.unitypackage.

![Как должно выглядить](https://i.gyazo.com/c611c353000320c652f0c90d6d09e02a.png)

Отключить лишние библиотеки unity расположенные в Packages можно через Window->Package Manager. Вам понадобятся только Package Manager UI и TextMesh Pro для работы c фреймворком.

## Шаг 3
ПКМ Assets-> Import Package -> Custom Package -> Выбираем Install.unitypackage

[![Шаг 3](https://i.gyazo.com/fe2407b92621458309dca7241ae5b98d.gif)](https://gyazo.com/fe2407b92621458309dca7241ae5b98d)

## Шаг 4
_Пропускаем если хотим использовать фреймворк из другого проекта Unity. Это удобно для обновлений с гитхаба._
ПКМ Assets-> Import Package -> Custom Package -> Выбираем Assets/[0]Framework/-Install/Framework.unitypackage

## Шаг 5
_Пропускаем если использовали шаг 4_
ПКМ Assets-> Create -> Folder Symlink -> Выбираем папку ВАШ_ПУТЬ_К_ПРОЕКТУ_С_ФРЕЙМВОРКОМ/Assets/[0]Framework/Runtime 

[![Шаг 5](https://i.gyazo.com/d74241c122a2e47947f0cbddc3629bfb.gif)](https://gyazo.com/d74241c122a2e47947f0cbddc3629bfb)

Если все сделано правильно то появится папка Runtime в вашем проекте вместе с зелеными стрелочками-индикаторами справа от названия.

![Шаг 5](https://i.gyazo.com/48d37bc7940c77dfca83979e6f79d194.png)

**Внимание!** Убедитесь, что папка Runtime находиться внутри папки [0]Framework для корректной работы.

## Шаг 6
Edit->Project Settings->Player->Scripting Runtime Version .Net 4.x Equivalent 

![Шаг 6](https://i.gyazo.com/b34a9b1308312b910f4e78375c95f290.png)

## Шаг 7
ПКМ Assets-> Import Package -> Custom Package -> Выбираем Template.unitypackage ( он расположен в папке -Install )
После этого можно удалять папку Install. Если все было сделано правильно ваша структура проекта будет выглядить примерно так :

![Шаг 7](https://i.gyazo.com/36febd0d11e93cae34858e65715f9ad4.png)

## Шаг 8
Убедитесь, что Scene Camera, Scene Game, Scene Kernel заброшены в Build Settings. Порядок не имеет значения.
Scene Kernel является обязательной сценой, остальные включены в качестве примера и необязательны. 

![Шаг 8](https://i.gyazo.com/eb741a5a05f4ffac385fde3ce80207b5.png)

## Шаг 9
Edit -> Project Settings -> Script Execution Order
Настройте время испольнения скриптов: 

![Шаг 9](https://i.gyazo.com/0b587602c2c63d98c123ec5d7ba8690b.png)


Вот и все. Фреймворк готов к бою 👍 