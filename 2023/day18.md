# Оставшиеся доставки Санты

## 🇺🇸 Описание

Santa needs your help to count the number of presents he has to deliver!
He's got all kinds of presents, from video game consoles (🎮), stuffed animals (🧸),
toy cars (🏎️), books (📚), and more!

We need a general purpose type that can take a tuple of items as its first arguemnt
and an item to search for as the second argument. It should return a count of the item specified.

For example:

```typescript
Count<['👟', '👟', '💻', '🎸', '🧩', '👟', '🧸'], '👟'>;
```

should return `3` because there are three `👟`.

## 🇷🇺 Описание

Санте нужна ваша помощь, чтобы подсчитать количество подарков, которые он должен доставить!
У него есть всевозможные подарки: игровые приставки (🎮), мягкие игрушки (🧸),
игрушечные машинки (🏎️), книги (📚) и многое другое!

Нам нужен универсальный тип, который принимает кортеж элементов в качестве первого аргумента и элемент
для поиска в качестве второго аргумента. Тип должен возвращать количество указанных элементов.

Например:

```typescript
Count<['👟', '👟', '💻', '🎸', '🧩', '👟', '🧸'], '👟'>;
```

должен вернуть `3`, так как в массиве три элемента `👟`.

## 💻 Решение

Для решения используем рекурсию, проходя по элементам массива и подсчитывая количество вхождений искомого элемента.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type Count<Toys, Toy, Result extends 1[] = []> =
    Toys extends [infer Head, ...infer Tail]
        ? Head extends Toy
            ? Count<Tail, Toy, [1, ...Result]>
            : Count<Tail, Toy, Result>
        : Result['length'];

// Тесты
type ToySack = [
    '🎸', '🎧', '👟', '👟', '💻', '🪀', '🧩', '🎮',
    '🎨', '🕹️', '📱', '🧩', '🧸', '🎧', '👟', '🚲',
    '📚', '⌚', '🎨', '👟', '🎸', '🧸', '👟', '🎸',
    '📱', '🎧', '🎮', '🎒', '📱', '🧩', '🧩', '🚲',
    '🕹️', '🧵', '📱', '🕹️', '🕰️', '🧢', '🕹️', '👟',
    '🧸', '📚', '🧁', '🧩', '🎸', '🎮', '🧁', '📚',
    '💻', '⌚', '🛹', '🧁', '🧣', '🪁', '🎸', '🧸',
    '🧸', '🧸', '🧩', '🪁', '🏎️', '🏎️', '🧁', '📚',
    '🧸', '🕶️', '💻', '⌚', '⌚', '🕶️', '🎧', '🎧',
    '🎧', '💻', '👟', '🎸', '💻', '🪐', '📚', '🎨',
    '📱', '🎧', '📱', '🎸', '🏎️', '👟', '🚲', '📱',
    '🚲', '🎸'
];

type test_0_actual = Count<ToySack, '👟'>;
type test_0_expected = 8;
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = Count<ToySack, '🧦'>;
type test_1_expected = 0;
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type test_2_actual = Count<ToySack, '🧩'>;
type test_2_expected = 6;
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;

type test_3_actual = Count<ToySack, '🛹'>;
type test_3_expected = 1;
type test_3 = Expect<Equal<test_3_expected, test_3_actual>>;

type test_4_actual = Count<ToySack, '🏎️'>;
type test_4_expected = 3;
type test_4 = Expect<Equal<test_4_expected, test_4_actual>>;

type test_5_actual = Count<ToySack, '📚'>;
type test_5_expected = 5;
type test_5 = Expect<Equal<test_5_expected, test_5_actual>>;
```