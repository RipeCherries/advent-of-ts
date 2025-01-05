# Цены на реактивное топливо

## 🇺🇸 Описание

While It's well known that with Christmas spirit at all-time lows, 🎅 Santa has had to resort to
using jet fuel to help power his sleigh. But in this economy, gas prices are through the roof!
To help keep costs down, the elves came up with a plan to track the distance between each stop on 🎅 Santa's route.

The elves don't really understand "miles" or "kilometers". Instead, they just use the dashes
they drew on the map to represent a unit of fuel needed to travel between locations.
They've written out 🎅 Santa's entire route as a string of locations separated by these markers.

For example:

`north_pole--candycane_forest----gumdrop_sea-------hawaii`

This means:

* 'candycane_forest' is 2 units from 'north_pole';
* 'gumdrop_sea' is 4 units from 'candycane_forest';
* 'hawaii' is 7 units from 'gumdrop_sea'.

The elves need help building a type that will take their route string and calculate how much
fuel is needed to travel between each destination.

## 🇷🇺 Описание

Известно, что в то время как дух Рождества находится на рекордно низком уровне, 🎅 Санте пришлось прибегнуть к
использованию реактивного топлива, чтобы привести в движение свои сани. Но в условиях нынешней экономики цены на
бензин просто зашкаливают! Чтобы сократить расходы, эльфы придумали план, как отслеживать
расстояние между каждой остановкой на маршруте 🎅 Санты.

Эльфы не очень понимают, что такое "мили" или "километры". Вместо этого они используют черточки, которые
нарисовали на карте, чтобы обозначить количество топлива, необходимого для перемещения между локациями.
Они записали весь маршрут 🎅 Санты в виде строки с локациями, разделенными этими отметками.

Например:

`north_pole--candycane_forest----gumdrop_sea-------hawaii`

Это означает:

* 'candycane_forest' находится на расстоянии 2 единиц топлива от 'north_pole';
* 'gumdrop_sea' находится на расстоянии 4 единиц топлива от 'candycane_forest';
* 'hawaii' находится на расстоянии 7 единиц топлива от 'gumdrop_sea'.

Эльфам нужна помощь в создании типа, который сможет принять строку маршрута и рассчитать,
сколько топлива нужно для перемещения между каждой точкой назначения.

## 💻 Решение

```typescript
import type { Expect, Equal } from "type-testing";

// Решение
type GetRoute<Str extends string> = BuildRoute<Trim<Str>>;

type BuildRoute<
    Str extends string,
    DashCount extends number[] = [],
> = Str extends `-${infer RestStr}`
    ? BuildRoute<RestStr, [...DashCount, 0]>
    : Str extends `${infer Destination}-${infer RestStr}`
        ? [[Destination, DashCount["length"]], ...BuildRoute<RestStr, [0]>]
        : Str extends ""
            ? []
            : [[Str, DashCount["length"]]];

type Trim<Str extends string> = Str extends `-${infer RestStr}`
    ? Trim<RestStr>
    : Str extends `${infer RestStr}-`
        ? Trim<RestStr>
        : Str;

// Тесты
type t0_actual = GetRoute<"north_pole--candycane_forest----gumdrop_sea-------hawaii">;
type t0_expected = [
  ["north_pole", 0], ["candycane_forest", 2], ["gumdrop_sea", 4], ["hawaii", 7]
];
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = GetRoute<"a-b-c-d">;
type t1_expected = [
  ["a", 0], ["b", 1], ["c", 1], ["d", 1]
];
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = GetRoute<"🎅--🎄---🏠----🤶">;
type t2_expected = [
  ["🎅", 0], ["🎄", 2], ["🏠", 3], ["🤶", 4]
];
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = GetRoute<"">;
type t3_expected = [];
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type t4_actual = GetRoute<"north_pole">;
type t4_expected = [["north_pole", 0]];
type t4 = Expect<Equal<t4_actual, t4_expected>>;

type t5_actual = GetRoute<"a--b----c-d---e">;
type t5_expected = [
  ["a", 0], ["b", 2], ["c", 4], ["d", 1], ["e", 3]
];
type t5 = Expect<Equal<t5_actual, t5_expected>>;

type t6_actual = GetRoute<"--a-b">;
type t6_expected = [["a", 0], ["b", 1]];
type t6 = Expect<Equal<t6_actual, t6_expected>>;

type t7_actual = GetRoute<"a-b--">;
type t7_expected = [["a", 0], ["b", 1]];
type t7 = Expect<Equal<t7_actual, t7_expected>>;

type t8_actual = GetRoute<"north pole-candy.cane">;
type t8_expected = [["north pole", 0], ["candy.cane", 1]];
type t8 = Expect<Equal<t8_actual, t8_expected>>;

type t9_actual = GetRoute<"a--------------------------------------------------b">;
type t9_expected = [["a", 0], ["b", 50]];
type t9 = Expect<Equal<t9_actual, t9_expected>>;

type t10_actual = GetRoute<"a--a-a---a">;
type t10_expected = [["a", 0], ["a", 2], ["a", 1], ["a", 3]];
type t10 = Expect<Equal<t10_actual, t10_expected>>;

// @ts-expect-error should not be a generic array
type e0 = Expect<Equal<t0_actual, Array<[string, number]>>>;

// @ts-expect-error should only accept string input
type e1 = GetRoute<123>;
```