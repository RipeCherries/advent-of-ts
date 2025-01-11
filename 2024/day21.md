# Новые требования Бернарда 2: Электрическое бунтарство

## 🇺🇸 Описание

After discovering the unused variables in the elves' code, 🎩 Bernard has one more request...

> [🎩 Bernard] These scripts are getting better, but we need to track ALL the variables - both the ones 
> being used AND the ones collecting dust. We had a close call where a toy almost got left out of its 
> gift box because the variable was declared but never used - if we have to check all this by hand it's going 
> to take forever and kids might not get their presents!

The elves need to improve their code analyzer one final time to create a complete linting tool that will:

* Track all variable declarations and usage;
* Identify unused variables;
* Return everything in a single, organized report.

For example, when analyzing this script:

```typescript
let robotDog = "standard_model";
const giftBox = "premium_wrap";
var ribbon123 = "silk";

wrapGift(giftBox);
addRibbon(ribbon123);
```

The linter should produce:

```typescript
{
    scope: {
        declared: ["robotDog", "giftBox", "ribbon123"],
            used:["giftBox", "ribbon123"]
    },
    unused: ["robotDog"]
}
```

Implement a type `Lint` that performs this analysis, handling:

* All previous functionality (variable declarations and usage tracking);
* Identification of `unused` variables in a separate unused array;
* Various amounts of whitespace, tabs, and empty lines;
* Empty scripts (which should return empty arrays for all fields).

## 🇷🇺 Описание

После того как были обнаружены неиспользуемые переменные в коде эльфов, 🎩 Бернард выдвинул еще одну просьбу...

> [🎩 Бернард] Эти скрипты становятся лучше, но нам нужно отслеживать ВСЕ переменные - как используемые, 
> так и собирающие пыль. Мы чуть не упустили игрушку, которая могла остаться вне подарочной коробки, 
> потому что переменная была объявлена, но не использовалась. Если нам придется проверять всё это вручную, 
> это займет целую вечность, и дети могут не получить свои подарки!

Эльфы должны в последний раз улучшить свой анализатор кода, чтобы создать полноценный 
инструмент линтинга, который будет:

* Отслеживать все объявления и использование переменных;
* Выявлять неиспользуемые переменные;
* Возвращать всё в едином, организованном отчете.

Например, при анализе следующего скрипта:

```typescript
let robotDog = "standard_model";
const giftBox = "premium_wrap";
var ribbon123 = "silk";

wrapGift(giftBox);
addRibbon(ribbon123);
```

Линтер должен выдать:

```typescript
{
    scope: {
        declared: ["robotDog", "giftBox", "ribbon123"], 
        used:["giftBox", "ribbon123"]
    },
    unused: ["robotDog"]
}
```

Реализуйте тип `Lint`, который выполняет этот анализ, обрабатывая:

* Всю предыдущую функциональность (отслеживание объявлений и использования переменных);
* Выявление неиспользуемых переменных в отдельном массиве `unused`;
* Различное количество пробелов, табуляций и пустых строк;
* Пустые скрипты (которые должны возвращать пустые массивы для всех полей).

## 💻 Решение

Для решения задачи воспользуемся реализацией прошлого дня с добавлением типа для исключения использованных
переменных из объявленных.

```typescript
import type { Expect, Equal } from "type-testing";

// Решение
type VariableTypes = 'var' | 'let' | 'const';

type SpecialChars = '\n' | '\r' | '\t' | ';' | ' ';

type SkipSpecialChars<T> = T extends `${SpecialChars}${infer Rest}` ? SkipSpecialChars<Rest> : T;

type Accumulator = {
    declared: string[],
    used: string[]
}

type AnalyzeScope<T extends string, Acc extends Accumulator = { declared: [], used: [] }> =
    SkipSpecialChars<T> extends `${VariableTypes} ${infer VarId} = "${string}"${infer Rest}`
        ? AnalyzeScope<Rest, { declared: [...Acc["declared"], VarId], used: Acc['used'] }>
        : SkipSpecialChars<T> extends `${infer FnName}(${infer ArgName})${infer Rest}`
            ? AnalyzeScope<Rest, { declared: Acc['declared'], used: [...Acc['used'], ArgName] }>
            : Acc;

type ExcludeFromArray<Declared extends string[], Used extends string[]> =
    Declared extends [infer First, ...infer Rest extends string[]]
        ? First extends Used[number]
            ? ExcludeFromArray<Rest, Used>
            : [First, ...ExcludeFromArray<Rest, Used>]
        : [];

type Lint<T extends string> = AnalyzeScope<T> extends {
        declared: infer Declared extends string[],
        used: infer Used extends string[]
    }
    ? {
        scope: { declared: Declared, used: Used },
        unused: ExcludeFromArray<Declared, Used>
    } : never;

// Тесты
type t0_actual = Lint<`
let teddyBear = "standard_model";
`>;
type t0_expected = {
    scope: { declared: ["teddyBear"]; used: [] };
    unused: ["teddyBear"];
};
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = Lint<`
buildToy(teddyBear);
`>;
type t1_expected = {
    scope: { declared: []; used: ["teddyBear"] };
    unused: [];
};
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = Lint<`
let robotDog = "deluxe_model";
assembleToy(robotDog);
`>;
type t2_expected = {
    scope: { declared: ["robotDog"]; used: ["robotDog"] };
    unused: [];
};
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = Lint<`
let robotDog = "standard_model";
  const giftBox = "premium_wrap";
    var ribbon123 = "silk";
  
  \t
  wrapGift(giftBox);
  \r\n
      addRibbon(ribbon123);
`>;
type t3_expected = {
    scope: { declared: ["robotDog", "giftBox", "ribbon123"]; used: ["giftBox", "ribbon123"] };
    unused: ["robotDog"];
};
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type t4_actual = Lint<"\n\t\r \t\r ">;
type t4_expected = {
    scope: { declared: []; used: [] };
    unused: [];
};
type t4 = Expect<Equal<t4_actual, t4_expected>>;
```