# Генератор кода для ленивого эльфа

## 🇺🇸 Описание

Some of the lazier elves in the workshop are trying to automate their toy-making duties using code
with a familiar syntax. Before 🎅 Santa finds out about their automation scheme, the code needs to be
parsed and validated by 🎩 Bernard to make sure no corners are being cut!

Implement a type `Parse` that analyzes these scripts and breaks them down into their basic components.
The scripts include:

* Variable declarations (using `let`, `const`, or `var`);
* Function calls (like `wrapGift` or `buildToy`).

For example, when an elf writes:

```typescript
let teddyBear = "standard_model";
buildToy(teddyBear);
```

It needs to be decoded for 🎩 Bernard's review as:

```typescript
[
    {
        id: "teddyBear",
        type: "VariableDeclaration"
    },
    {
        argument: "teddyBear",
        type: "CallExpression"
    }
]
```

The code validator needs to handle:

* Different ways of declaring variables (`let`, `const`, and `var`);
* Any function call that takes a parameter;
* Various amounts of spacing, tabs, and empty lines (elves aren't great at formatting).

## 🇷🇺 Описание

Некоторые из самых ленивых эльфов в мастерской пытаются автоматизировать свои обязанности по
созданию игрушек с помощью кода с понятным синтаксисом. Прежде чем 🎅 Санта узнает об их схеме автоматизации,
код должен быть разобран и проверен 🎩 Бернардом, чтобы убедиться, что нигде не срезаны углы!

Реализуйте тип `Parse`, который анализирует эти скрипты и разбивает их на основные компоненты. Скрипты включают:

* Объявления переменных (с использованием `let`, `const` или `var`);
* Вызовы функций (такие как `wrapGift` или `buildToy`).

Например, если эльф пишет:

```typescript
let teddyBear = "standard_model";
buildToy(teddyBear);
```

Это должно быть декодировано для 🎩Бернарда как:

```typescript
[
    {
        id: "teddyBear",
        type: "VariableDeclaration"
    },
    {
        argument: "teddyBear",
        type: "CallExpression"
    }
]
```

Валидатор кода должен обрабатывать:

* Разные способы объявления переменных (`let`, `const` и `var`);
* Любые вызовы функций, которые принимают параметры;
* Различное количество пробелов, табуляций и пустых строк (эльфы не очень хорошо форматируют код).

## 💻 Решение

Для начала реализуем вспомогательные типы для типов переменных `VariableTypes` и специальных символов `SpecialChars`.
Также создадим тип, который будет пропускать все специальные символы `SkipSpecialChars`.
Во время парсинга в типе `Parse` сначала очищаем строку от специальных символов, затем проверяем является ли строка
объявлением переменной или вызовом функции. В зависимости от результата добавляем в `Acc` соответствующий объект.

```typescript
import type { Expect, Equal } from "type-testing";

// Решение
type VariableTypes = 'var' | 'let' | 'const';

type SpecialChars = '\n' | '\r' | '\t' | ';' | ' ';

type SkipSpecialChars<T> = T extends `${SpecialChars}${infer Rest}` ? SkipSpecialChars<Rest> : T;

type Parse<T extends string, Acc extends {}[] = []> =
    SkipSpecialChars<T> extends `${VariableTypes} ${infer VarId} = "${string}"${infer Rest}`
        ? Parse<Rest, [...Acc, { type: 'VariableDeclaration', id: VarId }]>
        : SkipSpecialChars<T> extends `${infer FnName}(${infer ArgName})${infer Rest}`
            ? Parse<Rest, [...Acc, { type: 'CallExpression', argument: ArgName }]>
            : Acc;

// Тесты
type t0_actual = Parse<`
let teddyBear = "standard_model";
`>;
type t0_expected = [
    {
        id: "teddyBear";
        type: "VariableDeclaration";
    },
];
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = Parse<`
buildToy(teddyBear);
`>;
type t1_expected = [
    {
        argument: "teddyBear";
        type: "CallExpression";
    },
];
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = Parse<`
let robotDog = "deluxe_model";
assembleToy(robotDog);
`>;
type t2_expected = [
    {
        id: "robotDog";
        type: "VariableDeclaration";
    },
    {
        argument: "robotDog";
        type: "CallExpression";
    },
];
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = Parse<`
  const giftBox = "premium_wrap";
    var ribbon123 = "silk";
  
  \t
  wrapGift(giftBox);
  \r\n
      addRibbon(ribbon123);
`>;
type t3_expected = [
    {
        id: "giftBox";
        type: "VariableDeclaration";
    },
    {
        id: "ribbon123";
        type: "VariableDeclaration";
    },
    {
        argument: "giftBox";
        type: "CallExpression";
    },
    {
        argument: "ribbon123";
        type: "CallExpression";
    },
];
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type t4_input = "\n\t\r \t\r ";
type t4_actual = Parse<t4_input>;
type t4_expected = [];
type t4 = Expect<Equal<t4_actual, t4_expected>>;
```