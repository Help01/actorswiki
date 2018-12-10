## Assembly Definition
В фреймворке используется строгое деление проекта на динамические библиотеки с помощью [Assembly Definition](https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html) от Unity.
Это ускоряет время компиляции скриптов так как не нужно будет пересобирать весь проект каждый раз при редактировании скрипта. Так же это позволяет явно разделить зоны ответственности проекта и не позволит пихать что-то связанное с игрой в область фреймворка и наоборот. Однако этот паттерн работает только если вы будете _везде_ придерживаться правил Assembly Definition. 

Вам потребуется как минимум по одному ASM файлу на корневую папку. Если вы используете Editor папки то обязательно нужно создать дополнительный ASM под эту папку Editor , указать что она работает только в редакторе и связать ее с ASM уровнем выше.

Правила оформления ASM:
- Namespace.Source <- обычные asm
- Namespace.Source.Editor <- asm редактора

![Пример ASM](https://i.gyazo.com/e8980bba613d5604546e615740935bf7.png)

## Структура папок
* [0]Framework - ASM Homebrew.Framework 
    * Common - Здесь лежат библиотеки написанные разработчиком, файлы общего назначения ( например документация )
    * Runtime - "Тело" фреймворка. Здесь содержится вся логика работы с фреймворком. Сюда стоит залезать только чтобы расширить функционал фреймворка или что-то поправить. Фреймворк никак не связан с игрой и не "видит" ее.

* [1]Source - ASM Homebrew.Game ( Ref : Homebrew.Framework )

Это папка с исходниками игры. Тут лежат все скрипты напрямую связанные с проектом. Для того чтобы игра "видела" фреймворк в Homebrew.Game должны быть ссылки на Homebrew.Framework и по необходимости другие ASM. Так например если вы хотите использовать TextMesh Pro то должны подключить его библиотеку Unity.TextMeshPro _( расположена в Packages/TextMeshPro/Scripts/Runtime )_ к Homebrew.Game.

* [2]Content - Здесь хранятся все ресурсы по игре: графика, звуки, анимации, шрифты. Структура нижеуказанных папок обязательна для работы с фреймворком.
    * Resources
        * Data
             * Blueprints - Здесь хранятся схемы и ассет с настройками для схем
        * Prefabs - Сюда кладем префабы к которым хотим обращаться.
    * Scenes - Папка с сценами проекта.

* [3]ThirdParty - Если у вас есть купленные библиотеки то по умолчанию их можно класть сюда. Это необязательная папка.
* Plugins - Папка плагинов. Традиционно используется для нейтив библиотек написанных на других языках. 
* Streaming Assets - После сборки проекта эта папка будет в него добавлена. Это позволяет использовать ресурсы из этой папки в оригинальном формате. Полезно для моддинга. 

Узнать больше о папках предусмотренных Unity можно в разделе [SpecialFolders](https://docs.unity3d.com/Manual/SpecialFolders.html)

Это рекомендуемая настройка папок, однако можно следовать своей структуре, но папки [2]Content/Resources/Prefabs и [2]Content/Resources/Data должны быть. Так же если вы хотите адекватно обновлять фреймворк с гитхаба структура папки [0]Framework обязательна.

## Названия файлов
Это необязательный пункт. Основан на моем личном опыте работы с проектами. С точки зрения разработчика и проекта в целом смысловые названия файлов не имеют ценности: разные люди могут интерпритировать название по-разному. На первый план выходит хранение файлов по типу, быстрый поиск и прозрачность.

### Названия скриптов
Начинаются с вида скрипта. Это позволяет понять о чем скрипт не залезая внутрь. В рамках фреймворка где компоненты строго разделены по своему функционалу это имеет смысл.

* Все акторы начинаются со слова Actor : ActorPlayer, ActorEnemy1, ActorEnemy2, ActorTank, ActorSpaceShip, ActorAsteroid
* Все компоненты данных начинаются с Data : DataPlayer, DataView, DataMove, DataWeapon
* Все обработчики начинаются с Processing : ProcessingAmmo, ProcessingAI, ProcessingMove
* Все сигналы начинаются с Signal : SignalDamage, SignalJobsDone
* Все компоненты Unity : ComponentTag, ComponentAnimationEvents
* И так далее для любого типа скрипта: Sample,Blueprint,Factory

![Названия скриптов](https://i.gyazo.com/958b430486bcf32451d94dfce87044e7.png)

Следуя этому правилу очень легко запомнить, что все "менеджеры и контроллеры" фреймворка начинаются со слов Processing. Так, захотев воспользоваться сигналами в фреймворке вы уже будете искать что-то типа ProcessingSignals.

### Названия ассетов и префабов.
ТИП АССЕТА пробел ТИП пробел УКАЗАТЕЛЬ пробел ИНДЕКС
Например : Animation Enemy Attack, Sample Weapon 2

![Пример](https://i.gyazo.com/e063a7922892cd70e3e7551c1b145315.png)


PS Не ленитесь использовать готовые паттерны по названиям или разрабатывать свои. С ростом проекта растут и издержки на его поддержание. Дисциплина, прозрачность, единообразие являются залогом более продуктивной и устойчивой работы.





