# Установка фреймворка

> Актуально для версий юнити 2019+

Оглавление
1. [Способы установки](#one)
2. [Дополнение к установке](#additionally)

## Способы установки <a name="one"></a>

Существует 3 способа установки фреймворка:
1. Качая и устанавливая *последний стабильный (без постфикса 'a' и 'b')* [релиз](https://github.com/dimmpixeye/actors/releases)
2. **Рекомендуется!** Установка фреймворка через Packages прямо с GitHub
3. Установка фреймворка через Symlink (для этого потребуется доп. скрипт)

> Второй способ рекомендуется по следующим причинам:
> 1. Быстрая и удобная установка, не требующая лишних телодвижений
> 2. Быстрое и удобное обновление фреймворка "в пару кликов"
> 3. Если у Вас IDE позволяет просматривать unity packages, то нет необходимости код фреймворка "внедрять" в проект, чтобы > посмотреть код фреймворка.  
> *Пример такой IDE - Rider*.  
> Если же Ваша IDE не поддерживает вышеописанную возможность, то можете использовать третий способ установки.

### Первый способ
1. Скачайте последний [релиз](https://github.com/dimmpixeye/actors/releases)
2. Распакуйте в удобное для Вас место
3. Создайте новый проект
4. Для загрузки фреймворка с диска, откройте **Window/Package Manager**.  
Нажмите **"+"** в верхнем левом углу и нажмите **"Add package from disk..."**  
Выберете файл *package.json* из загруженного архива с фреймворком  
Примерный результат:  
![Результат установки фреймворка](https://thumb.cloud.mail.ru/weblink/thumb/xw1/TV5N/4Gi8tNDX9/Вариант%201_1.JPG?x-email=aleksey_ohezin%40mail.ru)

### Второй способ
1. Создайте новый проект
2. Откройте файл **Ваш_проект/Packages/manifest.json**  
![Примерный вид manifest.json](https://thumb.cloud.mail.ru/weblink/thumb/xw1/4Noq/3XMdD6Cyr/Вариант%202_1.JPG?x-email=aleksey_ohezin%40mail.ru)
3. Добавьте в этот файл `"com.pixeye.ecs": "https://github.com/dimmpixeye/ecs.git",`  
![manifest.json после добавления строки](https://thumb.cloud.mail.ru/weblink/thumb/xw1/YEQZ/2ALMRvRjM/Вариант%202_2.JPG?x-email=aleksey_ohezin%40mail.ru)
4. Откройте Ваш проект. Дождитесь загрузки библиотек. Готово!

### Третий способ
> Подготовка проекта
1. Скачайте архивом [LibLinker](https://cloud.mail.ru/public/2kL7/3dNLwjx4F)
2. Создайте новый проект
3. Закиньте папку **LibLinker** в **Assets**
4. Для удобства, создайте папку для фреймворка - **Framework**  
![Примерный результат](https://thumb.cloud.mail.ru/weblink/thumb/xw1/5bPr/gPFtd9VD7/Вариант%203_1.JPG?x-email=aleksey_ohezin%40mail.ru)

> Подготовка репозитория с фреймворком  
> Необходимо скачать репозиторий фреймворка на локальный диск.  
Для этого существует несколько способов, но я опишу самый простой.  
1. Переходим на [github фреймворка](https://github.com/dimmpixeye/actors) и нажимаем **Clone or download/Open in Desktop** (для этого у Вас должна быть установлена десктопная версия GitHub'а). **Download ZIP не нужно нажимать! Такой способ будет равносилен первому!**  
2. Следуйте инструкциям в **GitHub Desktop** для копирования репозитория фреймворка

> Импорт фреймворка в проект
1. Находясь в Unity, **нажмите ПКМ по папке Framework** и выберите ** Create/Folder (Symlink)**, далее выберите папку **Runtime** из **локального репозитория фреймворка**.  
Проделайте тоже действие для папки Editor.  
В случае необходимости - можно и для папке Install  
![Примерный результат](https://thumb.cloud.mail.ru/weblink/thumb/xw1/2ra9/57bYvWFWE/Вариант%203_2.JPG?x-email=aleksey_ohezin%40mail.ru)

# Дополнительно <a name="additionally"></a>

1. Для работы фреймворка необходимы только библиотеки **Package Manager UI и TextMesh Pro**.  
Отключить лишние библиотеки unity расположенные в Packages можно в **Window/Package Manager**.  
!["Чистый проект с фреймворком"](https://thumb.cloud.mail.ru/weblink/thumb/xw1/3KNn/BkfACkdgV/Вариант%201_2.JPG?x-email=aleksey_ohezin%40mail.ru)  

2. В **Actors/Install** имеется 2 дополнительных библиотеки. Их использование не обязательно.  
![Доп. библиотеки Project Template и Plug Console](https://thumb.cloud.mail.ru/weblink/thumb/xw1/4w5z/3zgQDsQjJ/Вариант%201_3.JPG?x-email=aleksey_ohezin%40mail.ru)  
**0 Project Template** предназначена для загрузки шаблона проекта со стартовыми сценами  
**1 Plug Console** позволяет использовать внутриигровую консоль
