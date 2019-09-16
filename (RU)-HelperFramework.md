# static partial class HelperFramework
> Namespace: Pixeye.Framework.HelperFramework<br>

> Версия фреймворка: [[2019.09.15](https://github.com/dimmpixeye/actors/tree/2019.9.15)]<br>
> Последнее обновление статьи: [2019.09.16]


## Описание
HelperFramework - вспомогательный класс фреймворка. В нем содержится множество методов расширений, которые облегчат вашу жизнь! :D Они могут быть разбросаны по разным файлам, поэтому, возможно, Вы встретите их и в других статьях.

> Методы расширения первым параметром содержит this. Для более подробной информации о методах расширения, обратитесь к справочнику языка C#.

---

> Файл HelperFramework.cs <br>
> Последнее изменение файла: [2019.02.24]

## Math
| Тип        | Метод                 | Описание
| ---------- | --------------------- | ------------ 
| bool       | Every(this float step, float time) <br> Every(this int step, float time) | 
| bool       | PlusCheck(ref this float arg0_pl, float val_pl, float clamp_pl = 1f) <br> PlusCheck(ref this int arg0_pl, int val_pl, int clamp_pl = 1)  |
| bool       | MinusCheck(ref this float arg0_mn, float val_mn, float clamp_mn = 0f) <br> MinusCheck(ref this int arg0_mn, int val_mn, int clamp_mn = 0)
| void       | Plus(ref this float arg0_pl, float val_pl, float clamp_pl = 1f) <br> Plus(ref this int arg0_pl, int val_pl, int clamp_pl = 1)
| void       | Minus(ref this float arg0_mn, float val_mn, float clamp_mn = 0f) 


