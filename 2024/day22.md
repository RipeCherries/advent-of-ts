# Экстренный парсинг JSON

## 🇺🇸 Описание

> [🎅 Santa, bursting into the workshop, waving a piece of paper frantically] EMERGENCY!
> The sleigh's navigation system is sending messages about some kind of critical malfunction but
> I can't make heads or tails of it!
>
> [🎩 Bernard, grabbing the paper] Let me see that... Oh no. OH NO. It looks like JSON, but...
> with TRAILING COMMAS!? It's a MESS!
>
> [⚡ Blitzen, peering over their shoulders] Can't we just use `JSON.parse()`?
>
> [🎩 Bernard, gasps in horror] And risk a runtime error at 30,000 feet? On CHRISTMAS EVE?!
> We need something type-safe. Something that can catch these issues as early as possible. Something...
>
> [🎅 Santa, interrupting] Just fix it! If we can't parse these messages correctly,
> we might end up delivering presents to fish in the Pacific Ocean instead of children in the Pacific Northwest!

The elves need your help to build a type-safe JSON parser that can decode these critical messages from the sleigh!
The parser should convert JSON strings into their equivalent TypeScript types, catching any errors as early as possible.

For example:

```typescript
type test = Parse<`{
  "altitude": 123,
  "warnings": [
    "low_fuel",\t\n
    "strong_winds",
  ],
}`>;
// Should become:
type test = {
    altitude: 123;
    warnings: ["low_fuel", "strong_winds"];
};
```

The parser MUST handle:

* Objects with string/number keys and any JSON value;
* Arrays with mixed types;
* Primitive values (strings, numbers, booleans, null);
* Nested objects and arrays;
* Those dreaded trailing commas in objects and arrays;
* Various amounts of whitespace and newlines.

## 🇷🇺 Описание

> [🎅 Санта, врываясь в мастерскую, размахивая листом бумаги] ЧРЕЗВЫЧАЙНАЯ СИТУАЦИЯ! Навигационная система саней
> отправляет сообщения о какой-то критической неисправности, но я ничего не могу понять!
>
> [🎩 Бернард, хватает бумагу] Дай-ка посмотреть... О нет. О НЕТ. Это похоже на JSON, но...
> С ЗАПЯТЫМИ В КОНЦЕ!? Это КОШМАР!
>
> [⚡ Блитцен, заглядывая через плечо] А нельзя просто использовать `JSON.parse()`?
>
> [🎩 Бернард, в ужасе] И рисковать ошибкой во время выполнения на высоте 30,000 футов?
> В РОЖДЕСТВЕНСКУЮ НОЧЬ?! Нам нужно что-то безопасное для типов. Что-то, что сможет ловить
> такие ошибки как можно раньше. Что-то...
>
> [🎅 Санта, перебивает] Просто исправьте это! Если мы не сможем правильно разобрать эти сообщения,
> мы можем доставить подарки рыбе в Тихом океане вместо детей на северо-западе Тихого океана!

Эльфам нужна твоя помощь, чтобы создать безопасный для типов парсер JSON, который сможет декодировать
эти важные сообщения от саней! Парсер должен преобразовывать JSON-строки в их эквивалентные типы TypeScript,
улавливая ошибки как можно раньше.

Например:

```typescript
type test = Parse<`{
  "altitude": 123,
  "warnings": [
    "low_fuel",\t\n
    "strong_winds",
  ],
}`>;
// Должно преобразоваться в:
type test = {
    altitude: 123;
    warnings: ["low_fuel", "strong_winds"];
};
```

Парсер ДОЛЖЕН уметь:

* Обрабатывать объекты со строковыми/числовыми ключами и любыми JSON-значениями;
* Работать с массивами со смешанными типами;
* Обрабатывать примитивные значения (строки, числа, булевые значения, null);
* Работать с вложенными объектами и массивами;
* Справляться с этими ужасными запятыми в конце в объектах и массивах;
* Игнорировать различные количества пробелов и переносов строк.

## 💻 Решение

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
type Whitespace = ' ' | '\n' | '\r' | '\t'

type Int = '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'

type EscapeChar = {
    '\\': '\\'
    b: '\b'
    f: '\f'
    n: '\n'
    r: '\r'
    t: '\t'
    '"': '"'
}

type Trim<T extends string> = T extends `${Whitespace}${infer Body}`
    ? Trim<Body>
    : T extends `${infer Body}${Whitespace}`
        ? Trim<Body>
        : T

type RemoveComma<S extends string> = S extends `${Whitespace}${infer R}`
    ? RemoveComma<R>
    : S extends `,${infer R}`
        ? Trim<R>
        : S

type ParseValue<T extends string> =
    | ParseInt<T>
    | ParseString<T>
    | ParseBoolean<T>
    | ParseNull<T>
    | ParseArray<T>
    | ParseObject<T>

type ParseInt<
    T extends string,
    K extends string = '',
> = T extends `${infer D extends Int}${infer Body}`
    ? ParseInt<Body, `${K}${D}`>
    : K extends `${infer Head extends number}`
        ? [Head, T]
        : never

type ParseString<T extends string> = T extends `"${infer Body}` ? ParseStringBody<Body> : never

type ParseStringBody<
    T extends string,
    K extends string = '',
> = T extends `${infer Head}${infer Body}`
    ? Head extends '"'
        ? [K, Body]
        : Head extends '\\'
            ? Body extends `${infer Escape extends keyof EscapeChar}${infer Body}`
                ? ParseStringBody<Body, `${K}${EscapeChar[Escape]}`>
                : never
            : ParseStringBody<Body, `${K}${Head}`>
    : never

type ParseBoolean<T extends string> = T extends `true${infer Body}`
    ? [true, Body]
    : T extends `false${infer Body}`
        ? [false, Body]
        : never

type ParseNull<T extends string> = T extends `null${infer Body}` ? [null, Body] : never

type ParseArray<T extends string> = T extends `[${infer Head}` ? ParseArrayBody<Head> : never

type ParseArrayBody<T extends string, Acc extends unknown[] = []> = T extends `]${infer Body}`
    ? [Acc, Body]
    : ParseValue<Trim<T>> extends [infer Value, infer Body extends string]
        ? ParseArrayBody<RemoveComma<Body>, [...Acc, Value]>
        : never

type ParseObject<T extends string> = T extends `{${infer Body}` ? ParseObjectBody<Body> : never

type ParseObjectBody<
    T extends string,
    Obj extends Record<any, unknown> = {},
> = T extends `}${infer Body01}`
    ? [Obj, Body01]
    : ParseValue<Trim<T>> extends [infer Head extends string | number, infer Body02 extends string]
        ? Trim<Body02> extends `:${infer Body03}`
            ? ParseValue<Trim<Body03>> extends [infer Value, infer Body04 extends string]
                ? ParseObjectBody<
                    RemoveComma<Body04>,
                    { [Char in Head | keyof Obj]: Char extends Head ? Value : Obj[Char] }
                >
                : never
            : never
        : never

type Parse<T extends string> = ParseValue<Trim<T>> extends [infer Parsed, ''] ? Parsed : never

// Тесты
type t0_actual = Parse<
    `{
    "a": 1, 
    "b": false, 
    "c": [
      true,
      false,
      "hello",
      {
        "a": "b",
        "b": false
      },
    ],
    "nil": null,
  }`
>
type t0_expected = {
    a: 1
    b: false
    c: [true, false, 'hello', {
        a: 'b'
        b: false
    }]
    nil: null
};
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = Parse<'1'>
type t1_expected = 1;
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = Parse<'{}'>
type t2_expected = {};
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = Parse<'[]'>
type t3_expected = [];
type t3 = Expect<Equal<t3_actual, t3_expected>>;


type t4_actual = Parse<'[1]'>;
type t4_expected = [1];
type t4 = Expect<Equal<t4_actual, t4_expected>>;


type t5_actual = Parse<'true'>
type t5_expected = true;
type t5 = Expect<Equal<t5_actual, t5_expected>>;

type t6_actual = Parse<'["Hello", true, false, null]'>;
type t6_expected = ['Hello', true, false, null];
type t6 = Expect<Equal<t6_actual, t6_expected>>;

type t7_actual = Parse<`{
  "hello\\r\\n\\b\\f": "world",
}`>
type t7_expected = {
    'hello\r\n\b\f': 'world',
}
type t7 = Expect<Equal<t7_actual, t7_expected>>;

type t8_actual = Parse<'{ 1: "world" }'>;
type t8_expected = { 1: 'world' };
type t8 = Expect<Equal<t8_actual, t8_expected>>;

type t9_actual = Parse<`{
  "altitude": 123,
  "warnings": [
    "low_fuel",\t\n
    "strong_winds",
  ],
}`>;
type t9_expected = {
    altitude: 123;
    warnings: ["low_fuel", "strong_winds"];
};
type t9 = Expect<Equal<t9_actual, t9_expected>>;
```