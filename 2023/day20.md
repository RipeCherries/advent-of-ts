# TypeScript ASCII Art!

## 🇺🇸 Описание

Your goal for this challenge is to take an input like Hi and turn it into ASCII art!

So for example Hi would turn into:

```text
█ █ █  
█▀█ █  
▀ ▀ █
```

but there's a twist! You'll also need to handle newlines! Take a look at the tests to see
some examples of that in action.

## 🇷🇺 Описание

Ваша цель в этом задании — взять ввод, например, `Hi`, и превратить его в ASCII-арт!

Например, `Hi` превратится в:

```text
█ █ █  
█▀█ █  
▀ ▀ █
```

Но есть загвоздка! Тебе также нужно обработать переносы строк!
Взгляни на тесты, чтобы увидеть примеры этого в действии.

## 💻 Решение

Используем рекурсию, строковые шаблоны и условные типы для решения данной задачи.

* `LettersKeys` представляет все допустимые символы (заглавные и строчные буквы);
* `Sentence` преобразовывает строку в одну линию ASCII-арта;
* `Parse` преобразовывает строку в массив из трёх строк ASCII-арта;
* `ToAsciiArt` обрабатывает многострочный текст (с `\n`) и преобразовывает каждую строку в массив строк ASCII-арта.

```typescript
import { Equal, Expect } from "type-testing";

// Решение
type Letters = {
    A: ['█▀█ ', '█▀█ ', '▀ ▀ '],
    B: ['█▀▄ ', '█▀▄ ', '▀▀  '],
    C: ['█▀▀ ', '█ ░░', '▀▀▀ '],
    E: ['█▀▀ ', '█▀▀ ', '▀▀▀ '],
    H: ['█ █ ', '█▀█ ', '▀ ▀ '],
    I: ['█ ', '█ ', '▀ '],
    M: ['█▄░▄█ ', '█ ▀ █ ', '▀ ░░▀ '],
    N: ['█▄░█ ', '█ ▀█ ', '▀ ░▀ '],
    P: ['█▀█ ', '█▀▀ ', '▀ ░░'],
    R: ['█▀█ ', '██▀ ', '▀ ▀ '],
    S: ['█▀▀ ', '▀▀█ ', '▀▀▀ '],
    T: ['▀█▀ ', '░█ ░', '░▀ ░'],
    Y: ['█ █ ', '▀█▀ ', '░▀ ░'],
    W: ['█ ░░█ ', '█▄▀▄█ ', '▀ ░ ▀ '],
    ' ': ['░', '░', '░'],
    ':': ['#', '░', '#'],
    '*': ['░', '#', '░'],
};

type LettersKeys = keyof Letters | Lowercase<keyof Letters>;

type TransformLine<
    Sentence extends string,
    Index extends number,
> = Sentence extends `${infer Char extends LettersKeys}${infer Rest}`
    ? `${Letters[Uppercase<Char>][Index]}${TransformLine<Rest, Index>}`
    : "";

type Parse<Sentence extends string, Counter extends unknown[] = []> = Counter["length"] extends 3
    ? []
    : [TransformLine<Sentence, Counter["length"]>, ...Parse<Sentence, [...Counter, unknown]>];

type ToAsciiArt<T extends string> = T extends `${infer Sentence}\n${infer Rest}`
    ? [...Parse<Sentence>, ...ToAsciiArt<Rest>]
    : Parse<T>;

// Тесты
type test_0_actual = ToAsciiArt<"   * : * Merry * : *   \n  Christmas  ">;
type test_0_expected = [
    "░░░░░#░░░█▄░▄█ █▀▀ █▀█ █▀█ █ █ ░░░#░░░░░",
    "░░░#░░░#░█ ▀ █ █▀▀ ██▀ ██▀ ▀█▀ ░#░░░#░░░",
    "░░░░░#░░░▀ ░░▀ ▀▀▀ ▀ ▀ ▀ ▀ ░▀ ░░░░#░░░░░",
    "░░█▀▀ █ █ █▀█ █ █▀▀ ▀█▀ █▄░▄█ █▀█ █▀▀ ░░",
    "░░█ ░░█▀█ ██▀ █ ▀▀█ ░█ ░█ ▀ █ █▀█ ▀▀█ ░░",
    "░░▀▀▀ ▀ ▀ ▀ ▀ ▀ ▀▀▀ ░▀ ░▀ ░░▀ ▀ ▀ ▀▀▀ ░░",
];
type test_0 = Expect<Equal<test_0_actual, test_0_expected>>;

type test_1_actual = ToAsciiArt<"  Happy new  \n  * : * : * Year * : * : *  ">;
type test_1_expected = [
    "░░█ █ █▀█ █▀█ █▀█ █ █ ░█▄░█ █▀▀ █ ░░█ ░░",
    "░░█▀█ █▀█ █▀▀ █▀▀ ▀█▀ ░█ ▀█ █▀▀ █▄▀▄█ ░░",
    "░░▀ ▀ ▀ ▀ ▀ ░░▀ ░░░▀ ░░▀ ░▀ ▀▀▀ ▀ ░ ▀ ░░",
    "░░░░#░░░#░░░█ █ █▀▀ █▀█ █▀█ ░░░#░░░#░░░░",
    "░░#░░░#░░░#░▀█▀ █▀▀ █▀█ ██▀ ░#░░░#░░░#░░",
    "░░░░#░░░#░░░░▀ ░▀▀▀ ▀ ▀ ▀ ▀ ░░░#░░░#░░░░",
];
type test_1 = Expect<Equal<test_1_actual, test_1_expected>>;

type test_2_actual = ToAsciiArt<"  * : * : * : * : * : * \n  Trash  \n  * : * : * : * : * : * ">;
type test_2_expected = [
    "░░░░#░░░#░░░#░░░#░░░#░░░",
    "░░#░░░#░░░#░░░#░░░#░░░#░",
    "░░░░#░░░#░░░#░░░#░░░#░░░",
    "░░▀█▀ █▀█ █▀█ █▀▀ █ █ ░░",
    "░░░█ ░██▀ █▀█ ▀▀█ █▀█ ░░",
    "░░░▀ ░▀ ▀ ▀ ▀ ▀▀▀ ▀ ▀ ░░",
    "░░░░#░░░#░░░#░░░#░░░#░░░",
    "░░#░░░#░░░#░░░#░░░#░░░#░",
    "░░░░#░░░#░░░#░░░#░░░#░░░",
];
type test_2 = Expect<Equal<test_2_actual, test_2_expected>>;

type test_3_actual = ToAsciiArt<"  : * : * : * : * : * : * : \n  Ecyrbe  \n  : * : * : * : * : * : * : ">;
type test_3_expected = [
    "░░#░░░#░░░#░░░#░░░#░░░#░░░#░",
    "░░░░#░░░#░░░#░░░#░░░#░░░#░░░",
    "░░#░░░#░░░#░░░#░░░#░░░#░░░#░",
    "░░█▀▀ █▀▀ █ █ █▀█ █▀▄ █▀▀ ░░",
    "░░█▀▀ █ ░░▀█▀ ██▀ █▀▄ █▀▀ ░░",
    "░░▀▀▀ ▀▀▀ ░▀ ░▀ ▀ ▀▀  ▀▀▀ ░░",
    "░░#░░░#░░░#░░░#░░░#░░░#░░░#░",
    "░░░░#░░░#░░░#░░░#░░░#░░░#░░░",
    "░░#░░░#░░░#░░░#░░░#░░░#░░░#░",
];
type test_3 = Expect<Equal<test_3_actual, test_3_expected>>;
```