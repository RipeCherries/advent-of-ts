# Можно ли исправить десятилетия кражи зарплаты?

## 🇺🇸 Описание

> [After decades of not raising the reindeer's pay, 🎅 Santa suddenly and unexpectedly
> decided to raise their pay quite substantially. The reindeer are in shock.]
>
> [☄️ Comet] It cannot be overstated enough how unlikely this was. How did he suddenly change.
>
> [💃 Dancer] We all know what happened. It was 💋 Crystal. She convinced him.
>
> [🌟 Vixen] This is gonna make such a huge difference for my finances.
> I can finally pay off the debt I accrued from Battlefront II loot boxes.
>
> [❤️ Cupid] You idiots. This is a win, but it's hardly putting us ahead.
> Have you forgotten about the 15 consecutive years of us being paid unfairly?
> All those lost wages will never come back to us. It's gone.
> We're only being paid a fair average wage now. We're not even paid well.
> 50% of reindeer make more than we do, and we're supposed to be happy about that!?
> We're the premiere reindeer the whole world looks up to yet we're paid like a substitute teacher elf.
>
> [💃 Dancer] Well, irregardless, we've got an easy day in front of us.
> All we need to do is fix a small problem with the street lights in town.
> After that... might be time to throw ourselves a little party!
>
> [❤️ Cupid] Is a party really appropriate given the circumstances? 💨 Dasher, didn't your
> wife's hospitalization 5 years ago bankrupt you - have you recovered from that?
>
> [💨 Dasher] Yeah, that is true, but look at the bright side! We're paid more now!
>
> [❤️ Cupid] Do I have to get out my inflation calculator again for you nincompoops??
>
> [💃 Dancer] Alright alright ❤️ Cupid, we all heard you. Let's just fix the street lights, can we?
>
> [❤️ Cupid] One day 🎅 Santa is gonna come to his senses and realize what happened here.
> When that day comes, we're in for an exceedingly bad time. Mark. My. Words.

## 🇷🇺 Описание

> [После десятилетий отказа повышать зарплаты оленям, 🎅 Санта внезапно и
> неожиданно решил значительно увеличить их оплату. Олени в шоке.]
>
> [☄️ Комета] Даже нельзя выразить, насколько это было маловероятно. Как он вдруг так изменился?
>
> [💃 Танцор] Мы все знаем, что произошло. Это была 💋 Кристалл. Она его убедила.
>
> [🌟 Виксен] Это так сильно повлияет на мои финансы. Я наконец смогу выплатить долг,
> который набрала из-за лутбоксов в Battlefront II.
>
> [❤️ Купидон] Вы идиоты. Да, это победа, но она нас едва ли выводит вперед.
> Вы забыли о 15 годах, когда нас несправедливо оплачивали? Все те потерянные зарплаты нам уже не вернут.
> Всё ушло. Сейчас нам просто платят среднюю справедливую зарплату. И даже она не велика.
> 50% оленей зарабатывают больше нас, и мы должны быть рады этому!?
> Мы — лучшие олени, на которых смотрит весь мир, а нас оплачивают, как заменяющего учителя эльфа.
>
> [💃 Танцор] Ну, как бы то ни было, впереди у нас лёгкий день.
> Всё, что нужно сделать, — это починить пару уличных фонарей в городе.
> А потом... можно устроить себе маленькую вечеринку!
>
> [❤️ Купидон] А вечеринка вообще уместна в таких обстоятельствах? 💨 Вихрь, разве
> госпитализация твоей жены 5 лет назад не разорила тебя? Ты восстановился после этого?
>
> [💨 Вихрь] Ну да, это правда, но взгляните на светлую сторону! Теперь нам платят больше!
>
> [❤️ Купидон] Нужно снова достать мой инфляционный калькулятор для вас, недоумки??
>
> [💃 Танцор] Ладно, ладно, ❤️Купидон, мы тебя все услышали. Давайте просто починим фонари, а?
>
> [❤️ Купидон] Однажды 🎅 Санта осознает, что здесь произошло.
> Когда это случится, нам придётся очень плохо. Запомните мои слова.

## 💻 Решение

В этой задачи нужно чтобы `colors` был массивом, а в `defaultColor` были лишь только те значения,
которые будут внутри `colors`. Для этого используем `NoInfer` тип.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
const createStreetLight = <T>(colors: T[], defaultColor: NoInfer<T>) => {
    console.log(colors);
    return defaultColor;
}

// Тесты
const colors = ["red" as const, "yellow" as const, "green" as const];
type Color = (typeof colors)[number];

const t0_const = createStreetLight(colors, "red");
type t0_actual = typeof t0_const;
type t0_expected = Color;
type t0 = Expect<Equal<t0_actual, t0_expected>>;

const t1_const = createStreetLight<Color>(colors, "red");
type t1_actual = typeof t1_const;
type t1_expected = Color;
type t1 = Expect<Equal<t1_actual, t1_expected>>;

// @ts-expect-error
const e0 = createStreetLight(colors, "blue");

// @ts-expect-error
const e1 = createStreetLight<Color, 'red'>(colors, "red");

// @ts-expect-error
const e2 = createStreetLight<Color, 'blue'>(colors, "blue");

// @ts-expect-error
const e3 = createStreetLight<Color>(colors, "blue");
```