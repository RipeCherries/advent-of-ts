# Контроль над разумом оказывается удивительно эффективным

## 🇺🇸 Описание

> [🎅 Santa was the victim of a magical mind-control potion, prepared by his own Mrs. Claus: 💋 Crystal Claus.
> It's the next morning and 🎅 Santa is just walking into the break room where all the reindeer
> were preparing for the day's chores.]
>
> [🎅 Santa] My dear wonderful reindeer! How truly magnificent you all are!
> I don't do enough to show my appreciation for you!
>
> [🔴 Rudolph] Um. Are you feeling ok? Also, if the answer is "yes", you better not be about to
> give us $15 starbucks gift cards again as our "big bonus this season".
>
> [🎅 Santa] 🔴 Rudolph, 🔴 Rudolph, sweet 🔴 Rudolph. I have had an awakening!
> It actually is $15 dollars! But it's a $15 dollar per hour salary increase for every reindeer!!
>
> [Silence. Utter silence in the room. The reindeer are in shock. Several long moments pass.
> The reindeer are expecting 🎅 Santa to laugh in their faces and say it's all a joke...]
>
> [🎅 Santa] draw up some composable contracts! I'll sign them all today!

You heard him! We need to make a general purpose `compose` function.
This particular (simplified) function just takes functors, and returns the result of calling each,
passing the result of the previous.

## 🇷🇺 Описание

> [🎅 Санта стал жертвой магического зелья для контроля разума, которое приготовила
> его собственная миссис Клаус: 💋 Кристалл Клаус. На следующее утро 🎅 Санта входит в комнату отдыха,
> где все олени готовились к дневным делам.]
>
> [🎅 Санта] Мои дорогие, замечательные олени! Как же вы великолепны!
> Я недостаточно ценю вас, и это нужно исправить!
>
> [🔴 Рудольф] Эээ... Ты точно в порядке? И если ответ "да", то ты ведь не собираешься снова дарить
> нам подарочные карты Starbucks на $15 как наш "главный бонус в этом сезоне", правда?
>
> [🎅 Санта] 🔴 Рудольф, 🔴 Рудольф, милый 🔴 Рудольф. Я пережил пробуждение!
> Это действительно $15! Но это $15 в час — повышение зарплаты для каждого оленя!!
>
> [Тишина. Полная тишина в комнате. Олени в шоке. Несколько долгих секунд.
> Они ждут, что 🎅 Санта сейчас рассмеется им в лицо и скажет, что это шутка...]
>
> [🎅 Санта] Подготовьте составные контракты! Я подпишу их все сегодня!

Вы слышали его! Нам нужно создать универсальную функцию `compose`. Эта конкретная (упрощённая) функция
принимает функторы и возвращает результат их вызова, передавая результат предыдущего в следующий.

## 💻 Решение

Типизируем функцию `compose`, которая последовательно применяет три переданные функции,
передавая результат одной в другую. Также типизируем все вспомогательные функции
(и допишем новые типы `FirstChar` и `FirstItem`).

```typescript
import type { Equal, Expect } from 'type-testing';

// Решение
const compose = <A, B, C, D>(
    f: (arg: A) => B,
    g: (arg: B) => C,
    h: (arg: C) => D
) => (a: A): D => (
    h(g(f(a)))
);

const upperCase = <T extends string>(x: T) => x.toUpperCase() as Uppercase<T>;
const lowerCase = <T extends string>(x: T) => x.toLowerCase() as Lowercase<T>;

type FirstChar<T extends string> = T extends `${infer FirstChar}${infer _}` ? FirstChar : T;
const firstChar = <T extends string>(x: T) => x[0] as FirstChar<T>;

type FirstItem<T extends any[]> = T extends [infer FirstItem, ...infer _] ? FirstItem : T;
const firstItem = <const T extends any[]>(x: T) => x[0] as FirstItem<T>;

const makeTuple = <const T>(x: T) => [x] as [T];
const makeBox = <const T>(value: T) => ({value}) as { value: T };

// Тесты
const t0 = compose(upperCase, makeTuple, makeBox)("hello!").value[0];
type t0_actual = typeof t0;
type t0_expected = "HELLO!";
type t0_test = Expect<Equal<t0_actual, t0_expected>>;

const t1 = compose(makeTuple, firstItem, makeBox)("hello!" as const).value;
type t1_actual = typeof t1;
type t1_expected = "hello!";
type t1_test = Expect<Equal<t1_actual, t1_expected>>;

const t2 = compose(upperCase, firstChar, lowerCase)("hello!");
type t2_actual = typeof t2;
type t2_expected = "h";
type t2_test = Expect<Equal<t2_actual, t2_expected>>;
```