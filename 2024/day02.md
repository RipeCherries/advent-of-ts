# Блитцен исправляет беспорядок

## 🇺🇸 Описание

> [🎅 Santa's reindeer, ⚡ Blitzen, has a secret meeting with 🎩 Bernard, the head of the elves after
> ☄️ Comet and 💨 Dasher presented 🎅 Santa with their a pay increase demand.]
>
> [⚡ Blitzen] Did Santa go for it? Did he accept our Demand for a pay increase?
>
> [🎩 Bernard] Those moron's ☄️ Comet and 💨 Dasher just wrote down `number`. I told them to write down a number.
>
> [⚡ Blitzen] What in the actual F...
>
> [🎩 Bernard] I know I know. You have to really spell it out for them.
>
> [⚡ Blitzen] Alright. I'll take care of it. I'll budget 100k for each of us Reindeer.
> That’s enough to cover therapy for pulling Santa’s bloated cheeks across three continents,
> plus a little left over for a sleigh-side mojito bar.
>
> [🎩 Bernard] I swear if I have to hold their hand anymore we're gonna put Reindeer sausage on the menu
> like the humans in Alaska seem to enjoy so much.
>
> [⚡ Blitzen, agast] You realize I'm a reindeer? You gotta lay off the powered sugar bro.
>
> [🎩 Bernard] Over my dead body. It's the only thing that makes me feel good in this nightmare of a job.

Yesterday ☄️Comet and 💨Dasher just gave `number` as their demand to 🎅 Santa.
That's not nearly specific enough. We're gonna need something much more specific.

Look at the tests. See what we can change to make them all pass.

## 🇷🇺 Описание

> [🎅 Санта-Клаусовский олень ⚡ Блитцен устраивает тайную встречу с 🎩 Бернардом, главным эльфом,
> после того как ☄️ Комета и 💨 Вихрь представили 🎅 Санте свои требования о повышении зарплаты.]
>
> [⚡ Блитцен]: Санта согласился? Он принял наше Требование о повышении зарплаты?
>
> [🎩 Бернард]: Эти идиоты ☄️ Комета и 💨 Вихрь просто написали слово `number`. Я сказал им написать конкретное число.
>
> [⚡ Блитцен]: Что, чёрт возьми, они вообще...
>
> [🎩 Бернард]: Я знаю, знаю. Им нужно всё разжёвывать.
>
> [⚡ Блитцен]: Ладно, я разберусь. Заложу в бюджет по 100 тысяч для каждого из нас, оленей.
> Этого хватит, чтобы покрыть расходы на терапию после того, как мы тащим толстую задницу Санты через три континента,
> и ещё останется немного на бар с мохито прямо у саней.
>
> [🎩 Бернард]: Клянусь, если мне придётся и дальше держать их за руку, мы добавим в меню колбаски из оленины,
> как это делают люди на Аляске.
>
> [⚡ Блитцен, потрясён]: Ты вообще в курсе, что я — олень? Тебе нужно завязывать с сахарной пудрой, братан.
>
> [🎩 Бернард]: Только через мой труп. Это единственное, что даёт мне хоть какое-то удовольствие
> в этом кошмаре под названием «работа».

Вчера ☄️ Комета и 💨 Вихрь просто написали слово `number` как своё требование к 🎅 Санте.
Это совершенно недостаточно конкретно. Нам нужно что-то гораздо более точное.

Посмотрите на тесты. Подумайте, что мы можем изменить, чтобы они все прошли.

## 💻 Решение

Для решения задачи создадим литеральный тип `Demand` со значением `900_000`.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
type Demand = 900_000;

// Тесты
type t0_actual = Demand;
type t0_expected = 900_000;
type t0 = Expect<Equal<t0_actual, t0_expected>>;
type e0 = Expect<Equal<Demand, number>>;
```