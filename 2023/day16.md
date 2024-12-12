# Найти Санту (часть 2)

## 🇺🇸 Описание

Since the episode with him getting lost on Tuesday (Day 12), the elves have started to get concerned about
Santa getting lost again, but deeper in the forest. Since Santa's college buddy got WiFi installed in the whole
property, Santa just wanders around scrolling TikTok without looking where he's going. Santa claimed that the reason
the whole campus needed WiFi (even the forest) was to "future-proof the business" and "attract top talent" but it's
beginning to seem like it was so he could personally get better phone service (cell reception in the north pole
isn't great and without 116th H.R.7302, neither is the rural internet speed).

Sure enough. It happened again. Santa got lost, again, but this time much deeper in the forest.

This time we have to search columns as well as rows to find him.

The `FindSanta` takes only one argument, the forest (an array of arrays), and returns the `[Row, Column]` indices
where Santa is located. Then an elf search team can be deployed to retrieve him.

## 🇷🇺 Описание

После того, как Санта потерялся во вторник (12-й день), эльфы начали всерьёз беспокоиться, что он снова может
заблудиться, но уже глубже в лесу. С тех пор как его приятель из колледжа установил WiFi по всей территории,
Санта бродит повсюду, не глядя под ноги, и бесконечно листает TikTok. Санта утверждал, что причина установки WiFi
на всей территории (даже в лесу) заключалась в том, чтобы "подготовить бизнес к будущему" и "привлечь лучших
специалистов", но кажется, что он сделал это просто для того, чтобы у него был лучше мобильный интернет.

И, конечно же, это снова произошло. Санта снова заблудился, но на этот раз гораздо глубже в лесу.

Теперь нам нужно искать его не только по строкам, но и по столбцам.

Тип `FindSanta` принимает только один аргумент — лес (массив массивов) — и возвращает индексы `[Row, Column]`,
где находится Санта. После этого команда спасателей из эльфов может быть отправлена, чтобы его вытащить.

## 💻 Решение

Используем рекурсию для обхода двумерного массива. Длина массивов `R` и `C` отслеживает текущие индексы
строки и столбца. Мы проверяем каждую ячейку, пока не найдем Санту.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type FindSanta<
    T extends string[][],
    R extends number[] = [],
    C extends number[] = [],
> = T[R["length"]][C["length"]] extends "🎅🏼"
    ? [R["length"], C["length"]]
    : C["length"] extends T[0]["length"]
        ? FindSanta<T, [...R, 1], []>
        : FindSanta<T, R, [...C, 1]>;

// Тесты
type Forest0 = [
    ['🎅🏼', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
];
type test_0_actual = FindSanta<Forest0>;
type test_0_expected = [0, 0];
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type Forest1 = [
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎅🏼', '🎄', '🎄'],
];
type test_1_actual = FindSanta<Forest1>;
type test_1_expected = [3, 1];
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type Forest2 = [
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎅🏼', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
];
type test_2_actual = FindSanta<Forest2>;
type test_2_expected = [2, 2];
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;

type Forest3 = [
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎅🏼', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
];
type test_3_actual = FindSanta<Forest3>;
type test_3_expected = [2, 1];
type test_3 = Expect<Equal<test_3_expected, test_3_actual>>;

type Forest4 = [
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎅🏼', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
    ['🎄', '🎄', '🎄', '🎄'],
];
type test_4_actual = FindSanta<Forest4>;
type test_4_expected = [1, 2];
type test_4 = Expect<Equal<test_4_expected, test_4_actual>>;
```