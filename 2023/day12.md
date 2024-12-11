# Найти Санту

## 🇺🇸 Описание

Strange as it may sound.. Santa went to college with someone that works at a big silicon valley networking company.
They've been buddies for years. So much so that in 2023 Santa pushed the workshop's board until they approved budget
to get WiFi on the entire campus. That way Santa can browse TikTok as he walks from building
to building across the campus.

But after all that doomscrolling, Santa realized he has lost himself in a Christmas tree forest!
A search team of elves has been deployed to find him, but he needs to give them more information about where
he is among the trees.

`FindSanta` is a type that takes a tuple as its only argument and returns the index where Santa is located.
Let's help Santa get back to the thing he's best at: inspiring leadership.

> **note**: `never` is returned if Santa cannot be found among the trees

## 🇷🇺 Описание

Как это ни странно звучит, Санта учился в колледже с человеком, который сейчас работает в крупной компании
из Кремниевой долины, занимающейся сетевыми технологиями. Они дружат уже много лет. Настолько, что в 2023 году Санта
убедил совет мастерской утвердить бюджет на установку WiFi на всей территории кампуса. Теперь Санта может
скроллить TikTok, переходя из здания в здание.

Но после всего этого Санта понял, что заблудился в елочном лесу! На его поиски отправилась команда эльфов,
но ему нужно дать им больше информации о том, где он находится среди деревьев.

`FindSanta` — это тип, который принимает кортеж в качестве аргумента и возвращает индекс, где находится Санта.
Давайте поможем Санте вернуться к тому, что он умеет лучше всего: вдохновляющему лидерству.

> **Примечание**: Если Санта не может быть найден среди деревьев, тип должен вернуть `never`.

## 💻 Решение

Для реализации типа `FindSanta` используем условные типы и рекурсию.
Тип перебирает элементы кортежа, сравнивать их со смайлом Санты, и возвращать индекс, если символ найден.
Если Санта отсутствует в массиве, возвращает `never`.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type FindSanta<T, Index extends any[] = []> = T extends [infer Head, ...infer Tail]
    ? Head extends '🎅🏼'
        ? Index['length']
        : FindSanta<Tail, [...Index, any]>
    : never;

// Тесты
type Forest0 = ['🎅🏼', '🎄', '🎄', '🎄'];
type test_0_actual = FindSanta<Forest0>;
type test_0_expected = 0;
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type Forest1 = ['🎄', '🎅🏼', '🎄', '🎄', '🎄', '🎄'];
type test_1_actual = FindSanta<Forest1>;
type test_1_expected = 1;
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type Forest2 = ['🎄', '🎄', '🎅🏼', '🎄'];
type test_2_actual = FindSanta<Forest2>;
type test_2_expected = 2;
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;

type Forest3 = ['🎄', '🎄', '🎄', '🎅🏼', '🎄'];
type test_3_actual = FindSanta<Forest3>;
type test_3_expected = 3;
type test_3 = Expect<Equal<test_3_expected, test_3_actual>>;

type Forest4 = ['🎄', '🎄', '🎄', '🎄'];
type test_4_actual = FindSanta<Forest4>;
type test_4_expected = never;
type test_4 = Expect<Equal<test_4_expected, test_4_actual>>;

```