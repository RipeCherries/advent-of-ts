# Анализ области видимости переменных: Новые требования Бернарда

## 🇺🇸 Описание

After discovering the elves' code automation scheme, 🎩 Bernard was... impressed!
But now he's sending it back along with every lazy elf's worst nightmare: more work...

> [🎩 Bernard] This is a good start, but we need to track which variables are actually being used.
> We can't have unused variables cluttering up our toy
> production scripts - who knows what kind of bugs that could cause!

The elves need to enhance their code analyzer to track both declared variables AND their usage.
For each script, they need to report:

* All variables that are declared (using `let`, `const`, or `var`);
* All variables that are actually used (as function arguments).

For example, when analyzing:

```typescript
let robotDog = "deluxe_model";
assembleToy(robotDog);
```

The analyzer should produce:

```typescript
{
    declared: ["robotDog"], 
    used: ["robotDog"]
}
```

And if variables are declared but never used (like in `let teddyBear = "standard_model";`),
they should only appear in the `declared` array, not the `used` array.

Implement a type `AnalyzeScope` that performs this analysis, handling:

* Variable declarations with any amount of whitespace;
* Function calls with variable references;
* Multiple declarations and usages in the same script;
* Empty or whitespace-only scripts.

## 🇷🇺 Описание

После того как 🎩 Бернард обнаружил схему автоматизации кода у эльфов, он был… впечатлён!
Но теперь он отправляет её обратно вместе с худшим кошмаром каждого ленивого эльфа: дополнительной работой...

> [🎩 Бернард] Это хороший старт, но нам нужно отслеживать, какие переменные действительно используются.
> Нельзя допускать, чтобы неиспользуемые переменные захламляли наши скрипты
> для производства игрушек — кто знает, к каким ошибкам это может привести!

Эльфам нужно улучшить свой анализатор кода, чтобы он отслеживал как объявленные переменные, так и их использование.
Для каждого скрипта необходимо сообщать:

* Все переменные, которые были объявлены (с использованием `let`, `const` или `var`);
* Все переменные, которые фактически используются (например, как аргументы функций).

Например, при анализе следующего кода:

```typescript
let robotDog = "deluxe_model";
assembleToy(robotDog);
```

Анализатор должен выдавать:

```typescript
{
    declared: ["robotDog"],
    used: ["robotDog"]
}
```

Если переменные объявлены, но никогда не используются (например, в `let teddyBear = "standard_model"`;),
они должны отображаться только в массиве `declared`, но не в массиве `used`.

Реализуйте тип `AnalyzeScope`, который выполняет этот анализ, обрабатывая:

* Объявления переменных с любым количеством пробелов;
* Вызовы функций с использованием переменных;
* Несколько объявлений и использований переменных в одном скрипте;
* Пустые скрипты или скрипты, содержащие только пробелы.

## 💻 Решение

Для решения задачи будем использовать реализацию предыдущего дня, но с небольшой модификацией.

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
            : Acc

// Тесты
type t0_actual = AnalyzeScope<`let teddyBear = "standard_model";`>;
type t0_expected =
    {
        declared: ["teddyBear"];
        used: [];
    };
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = AnalyzeScope<`buildToy(teddyBear);`>;
type t1_expected = {
    declared: [];
    used: ["teddyBear"];
};
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = AnalyzeScope<`
    let robotDog = "deluxe_model";
    assembleToy(robotDog);
`>;
type t2_expected = {
    declared: ["robotDog"];
    used: ["robotDog"];
};
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = AnalyzeScope<`
    let robotDog = "standard_model";
    const giftBox = "premium_wrap";
    var ribbon123 = "silk";
      
    \t
    wrapGift(giftBox);
    \r\n
    addRibbon(ribbon123);
`>;
type t3_expected = {
    declared: ["robotDog", "giftBox", "ribbon123"];
    used: ["giftBox", "ribbon123"];
};
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type t4_input = "\n\t\r \t\r ";
type t4_actual = AnalyzeScope<t4_input>;
type t4_expected = {
    declared: [];
    used: [];
};
type t4 = Expect<Equal<t4_actual, t4_expected>>;
```