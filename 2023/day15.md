# Упаковываем игрушки!

## 🇺🇸 Описание

*[Santa walks by as Bernard, the head elf, is yelling at the other elves..]*

> [🧝 Bernard (to his staff)] LET'S GO ELVES! LET'S GO! KEEP BOXING TOYS!
>
> [🎅🏻 Santa] Bernard.. Seems like it's not going well.
>
> [🧝 Bernard] Was anyone asking you!?
>
> [🎅🏻 Santa] Did you deploy the new toy boxing API yesterday?
>
> [🧝 Bernard] No, we didn't get to it. Julius called out sick.
>
> [🎅🏻 Santa] Taking too many sick days shows a lack of commitment. We should get rid of Julius.
>
> [🧝 Bernard (rolling eyes)] And then not replace him? Yeah. No Thanks.
>
> [🎅🏻 Santa] Well it was on the sprint and today's the last day of the sprint.
>
> [🧝 Bernard] We don't deploy on Fridays.
>
> [🎅🏻 Santa] Aren't we doing continuous deployment now? You had this whole big thing at the last
> shareholder meeting about it?
>
> [🧝 Bernard] No. For the 100th time. We're doing continuous delivery, which is completely different
> and gives us control over when we deploy.
>
> [🎅🏻 Santa] Well I need that BoxToys type. If you can't handle this project, Bernard, there are plenty of
> other elves who can. I need your full commitment.
>
> [🧝 Bernard] Ok. Fine. I'll do it myself.
>
> [🎅🏻 Santa] That's what I like to see!

The BoxToys type takes two arguments:

* the name of a toy;
* the number of boxes we need for this toy.

And the type will return a tuple containing that toy that number of times.

But there's one little thing.. We need to support the number of boxes being a union.
That means our resulting tuple can also be a union. Check out `test_nutcracker` in the tests to see how that works.

## 🇷🇺 Описание

*[Санта проходит мимо, пока Бернард, главный эльф, кричит на других эльфов...]*

> [🧝 Бернард (своим сотрудникам)] ДАВАЙТЕ, ЭЛЬФЫ! ДАВАЙТЕ! УПАКОВЫВАЕМ ИГРУШКИ!
>
> [🎅🏻 Санта] Бернард… Похоже, дела идут не очень.
>
> [🧝 Бернард] А кто-то спрашивал твоего мнения!?
>
> [🎅🏻 Санта] Ты развернул новый API для упаковки игрушек вчера?
>
> [🧝 Бернард] Нет, мы не успели. Юлий взял больничный.
>
> [🎅🏻 Санта] Слишком частые больничные показывают отсутствие приверженности делу. Мы должны избавиться от Юлия.
>
> [🧝 Бернард (закатывая глаза)] И при этом не заменить его? Отличная идея. Нет уж, спасибо.
>
> [🎅🏻 Санта] Но это было в спринте, а сегодня последний день.
>
> [🧝 Бернард] Мы не разворачиваем изменения в пятницу.
>
> [🎅🏻 Санта] Разве у нас не непрерывное развертывание сейчас? Ты говорил об этом на последнем собрании акционеров.
>
> [🧝 Бернард] Нет. В сотый раз объясняю: у нас непрерывная доставка.
> Это совсем другое и позволяет нам контролировать, когда мы развертываем изменения.
>
> [🎅🏻 Санта] Ну мне нужен тип `BoxToys`. Если ты не справляешься с проектом, Бернард, есть много других эльфов,
> которые справятся. Мне нужна твоя полная отдача.
>
> [🧝 Бернард] Ладно. Хорошо. Я сделаю это сам.
>
> [🎅🏻 Санта] Вот это мне нравится!

Тип BoxToys принимает два аргумента:

* название игрушки;
* количество коробок, необходимых для этой игрушки.

Тип возвращает кортеж, содержащий указанную игрушку заданное количество раз.

Но есть одна загвоздка:
Мы должны поддерживать ситуацию, когда количество коробок является объединением типов (union).
Это означает, что и результирующий кортеж также может быть объединением. Посмотрите на тест `test_nutcracker` в тестах,
чтобы понять, как это работает.

## 💻 Решение

Используем рекурсию для решения этой задачи: постепенно строим массив, добавляя элемент S на каждом шаге,
пока длина массива (`A["length"]`) не станет равна числу `N`.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type BoxToys<S, N extends number, A extends any[] = []> = N extends A["length"]
    ? A
    : BoxToys<S, N, [S, ...A]>;

// Тесты
type test_doll_actual = BoxToys<'doll', 1>;
type test_doll_expected = ['doll'];
type test_doll = Expect<Equal<test_doll_expected, test_doll_actual>>;

type test_nutcracker_actual = BoxToys<'nutcracker', 3 | 4>;
type test_nutcracker_expected =
    | ['nutcracker', 'nutcracker', 'nutcracker']
    | ['nutcracker', 'nutcracker', 'nutcracker', 'nutcracker'];
type test_nutcracker = Expect<Equal<test_nutcracker_expected, test_nutcracker_actual>>;
```