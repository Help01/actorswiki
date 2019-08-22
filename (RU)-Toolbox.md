# Toolbox
> Namespace: Pixeye.Framework.Toolbox<br>
> Наследуется от: Singleton\<Toolbox\>

> Версия фреймворка: [20.08.2019](https://github.com/dimmpixeye/ecs/tree/4d34ca97a89f72bde610249f47475895b70c2360)  (4d34ca97a89f72bde610249f47475895b70c2360)

## Описание
Является контейнером для процессоров и прочего.

## Статичные методы
| Тип             | Метод       | Описание  |
| --------------- |-------------| -----|
| T           | Add\<T\>(Type)  | Создает объект и добавляет его в Toolbox |
| void           | Add(object)  | Добавляет объект в Toolbox |
| T         | Get\<T\>()  | Возвращает экземпляр объекта из Toolbox |
| object         | Get(Type)  | Возвращает экземпляр объекта из Toolbox |
| void           | Remove(object)  | Удаляет объект из Toolbox |
| void           | InitializeObject(object)  | Инициализирует объект |