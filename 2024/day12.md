# Длинный список имен Бернарда

## 🇺🇸 Описание

> [Things are staring to heat up in the North Pole. The reindeer have been blackmailing Mrs. 
> Claus in order to get fair pay, threatening to expose her affair with 🪩 Jamie Glitterglum.]
>
> [ 🎅Santa] GET ME THOSE NAMES!!!!!!!!!!
>
> [🎩 Bernard] I'm workin' on it, I'm workin' on it!
>
> [🎅 Santa] TODAY! I NEED THEM TODAY!! ACTUALLY I NEED THEM LAST MONTH!!!
>
> [🎅 Santa throws a giant box of delicate glass Christmas tree ornaments against the factory wall, 
> sending shards of glass flying in every direction]
>
> [🎩 Bernard] Ah, there's the classic 🎅 Santa we all know and love. Smashing priceless ornaments 
> while screaming unintelligibly. Truly, the Christmas spirit personified.
>
> [🎅 Santa] TODAY! I NEED THEM TODAY!! ACTUALLY I NEED THEM LAST MONTH!!!
>
> [🎩 Bernard] You want every name? Fine. I'll even get you the ones with seven middle 
> initials and the kids named after TikTok trends.

🎩 Bernard has a very long list of names from the Social Security Administration, 
but we need to format the data into objects so 🎅 Santa can ingest it into his existing system.

Help 🎩 Bernard before 🎅 Santa continues his violent tirade. He's not about to spend a bunch of time 
looking at each child so instead he's just deciding whether a child is naughty or nice based on the number 
of characters in their name!

## 🇷🇺 Описание

> [Дела на Северном Полюсе начинают накаляться. Северные олени шантажируют миссис Клаус, 
> требуя справедливой оплаты труда, угрожая раскрыть её роман с 🪩 Джейми Глиттерглумом.]
> 
> [🎅 Санта] ДОСТАНЬ МНЕ ЭТИ ИМЕНА!!!!!!!!!!
>
> [🎩 Бернард] Я работаю над этим, я работаю!
> 
> [🎅 Санта] СЕГОДНЯ! МНЕ НУЖНЫ ОНИ СЕГОДНЯ!! НА САМОМ ДЕЛЕ, МНЕ НУЖНЫ ОНИ ЕЩЁ В ПРОШЛОМ МЕСЯЦЕ!!!
> 
> [🎅 Санта бросает огромную коробку с хрупкими стеклянными ёлочными игрушками в стену фабрики, 
> разбрасывая осколки стекла во все стороны.]
> 
> [🎩 Бернард] Ага, вот он, классический 🎅 Санта, которого мы все знаем и любим. 
> Разбивает бесценные игрушки, крича что-то невнятное. Истинное воплощение рождественского духа.
> 
> [🎅 Санта] СЕГОДНЯ! МНЕ НУЖНЫ ОНИ СЕГОДНЯ!! НА САМОМ ДЕЛЕ, МНЕ НУЖНЫ ОНИ ЕЩЁ В ПРОШЛОМ МЕСЯЦЕ!!!
> 
> [🎩 Бернард] Ты хочешь все имена? Хорошо. Я найду даже тех, у кого семь средних имен, и детей, 
> которых назвали в честь трендов в TikTok.

🎩 Бернард имеет очень длинный список имен из Управления социального обеспечения, но нам нужно отформатировать 
данные в объекты, чтобы 🎅 Санта мог загрузить их в свою систему.

Помогите 🎩 Бернарду, пока 🎅 Санта продолжает свою яростную тираду. У Санты нет времени разглядывать каждого 
ребенка, поэтому он будет решать, послушный ребенок или нет, основываясь на количестве символов в его имени!

## 💻 Решение

Для того чтобы перебрать массив такой длины нельзя использовать рекурсивные типы, т.к. будет вызвана ошибка глубины
рекурсии. Поэтому используем `mapped types`.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
type Split<T extends string, _Acc extends any[] = []> =
    T extends `${infer First}${infer Rest}`
        ? Split<Rest, [..._Acc, First]>
        : _Acc;

type Even = 2 | 4 | 6 | 8 | 0;

type NaughtyOrNice<T extends string> = Split<T>['length'] extends Even ? 'naughty' : 'nice';

type FormatNames<T extends [string, string, string][]> = {
    [K in keyof T]: T[K] extends [infer Name extends string, infer _, infer Num] ? {
        name: Name,
        count: Num extends `${infer T extends number}` ? T : never,
        rating: NaughtyOrNice<Name>
    } : never
};

// Тесты
type t0_actual = FormatNames<Names>['length'];
type t0_expected = 31682;
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = FormatNames<Names>[0];
type t1_expected = {
  name: 'Liam',
  count: 20802,
  rating: 'naughty'
};
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = FormatNames<Names>[11194];
type t2_expected = {
  name: 'Yanni',
  count: 19,
  rating: 'nice'
};
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = FormatNames<Names>[2761];
type t3_expected = {
  name: 'Petra',
  count: 148,
  rating: 'nice'
};
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type t4_actual = FormatNames<Names>[31680];
type t4_expected = {
  name: 'Aala',
  count: 5,
  rating: 'naughty'
};
type t4 = Expect<Equal<t4_actual, t4_expected>>;

type t5_actual = FormatNames<Names>[31681];
type t5_expected = {
  name: 'Aagya',
  count: 5,
  rating: 'nice'
};
type t5 = Expect<Equal<t5_actual, t5_expected>>;

export type Names = [
    ["Liam", "M", "20802"],
    ["Noah", "M", "18995"],
    ["Oliver", "M", "14741"],
    ["Emma", "F", "13527"],
    ["Charlotte", "F", "12596"],
    ["Amelia", "F", "12311"],
    ["Sophia", "F", "11944"],
    ["James", "M", "11670"],
    ["Elijah", "M", "11452"],
    ["Mia", "F", "11359"],
    ["Mateo", "M", "11229"],
    ["Theodore", "M", "11041"],
    ["Henry", "M", "10941"],
    ["Lucas", "M", "10842"],
    ["Isabella", "F", "10808"],
    // ...
]
```