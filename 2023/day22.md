# Судоку оленей

## 🇺🇸 Описание

Santa's reindeer sure do like to cause trouble! This time they've decided to make a game out of 
arranging themselves into a Sudoku board.

Before arranging themselves in this configuration, the reindeer left Santa a foreboding message:

> SaNtA.... yOu MuSt ImPleMeNt ThE Validate TyPe To DeTerMinE WhEThEr OuR SuDokU ConFiGuRaTiOn Is vALid

Oh.. and what's that... also Vixen seems to have left a separate note

> make sure Validate is a predicate 
> 
> —Vixen

Well that's sorta condescending. Vixen seems to be assuming we already know that a "predicate" 
is just a fancy computer science term for a function that returns `true` or `false`. Oh well. That's Vixen for you.

If you're not already familiar: Sudoku is a logic-based number placement puzzle. Here are the basic rules:

* Grid Structure: The game is played on a 9x9 grid, divided into nine 3x3 subgrids or "regions."; 
* Number Placement: The objective is to fill the grid with numbers from 1 to 9; 
* Row Constraint: Every row must contain each number from 1 to 9 without repeating; 
* Column Constraint: Every column must also contain each number from 1 to 9 without repeating; 
* Region Constraint: Each of the nine 3x3 regions must contain each number from 1 to 9, again without repetition;

Normally you solve the puzzle by logically deducing the numbers for the empty cells, ensuring that all rows, 
columns, and 3x3 regions have numbers from 1 to 9 according to the rules. However, in this case the cells are 
all already filled in and your mission is to instead determine whether the configuration follows the rules of Sudoku.

## 🇷🇺 Описание

Олени Санты, как всегда, любят создавать хаос! На этот раз они решили устроить игру, 
организовав себя в форме доски Судоку.

Прежде чем выстроить себя в такую конфигурацию, олени оставили Санте предвещающее сообщение:

> СаНтА... тЫ ДоЛжЕн РеАлиЗоВатЬ TyPe `Validate`, ЧтОбЫ ОпРеДеЛиТь, СоОтВеТсТвУеТ Ли Наша КоНфИгУрАцИя ПравИлАм СуДоКу

Ой... а это что? Похоже, Вихрь (Vixen) оставил отдельную записку:

> "Убедись, что `Validate` является предикатом"
> 
> — Вихрь

Ну, это как-то снисходительно. Похоже, Вихрь предполагает, что мы уже знаем, что "предикат" — это просто умный термин 
из информатики, обозначающий функцию, которая возвращает `true` или `false`. Ну, что поделать. Это так похоже на Вихря.

Если вы еще не знакомы: Судоку — это головоломка, основанная на размещении чисел. Вот основные правила:

* Структура сетки: Игра ведется на сетке 9x9, разделенной на девять подгридов 3x3, которые также называют "регионами";
* Размещение чисел: Цель игры — заполнить сетку числами от 1 до 9;
* Ограничение строк: Каждая строка должна содержать все числа от 1 до 9 без повторений;
* Ограничение столбцов: Каждый столбец также должен содержать все числа от 1 до 9 без повторений;
* Ограничение регионов: Каждый из девяти подгридов 3x3 должен содержать все числа от 1 до 9 без повторений.

Обычно Судоку решается путем логического вывода чисел для пустых ячеек, чтобы строки, столбцы и регионы 
соответствовали правилам. Однако в данном случае все ячейки уже заполнены, и ваша задача — определить, 
соблюдает ли эта конфигурация правила Судоку.

## 💻 Решение

Для решения данной задачи реализуем дополнительные типы для нахождения уникальных элементах в массиве.

```typescript
import { Equal, Expect } from 'type-testing';

// Решение
/** because "dashing" implies speed */
type Dasher = '💨';

/** representing dancing or grace */
type Dancer = '💃';

/** a deer, prancing */
type Prancer = '🦌';

/** a star for the dazzling, slightly mischievous Vixen */
type Vixen = '🌟';

/** for the celestial body that shares its name */
type Comet = '☄️';

/** symbolizing love, as Cupid is the god of love */
type Cupid = '❤️';

/** representing thunder, as "Donner" means thunder in German */
type Donner = '🌩️';

/** meaning lightning in German, hence the lightning bolt */
type Blitzen = '⚡';

/** for his famous red nose */
type Rudolph = '🔴';

type Reindeer = Dasher | Dancer | Prancer | Vixen | Comet | Cupid | Donner | Blitzen | Rudolph;

// Решение
type CheckAnyExtends<T, Arr extends any[]> =
    Arr extends [infer First, ...infer Rest]
        ? T extends First
            ? true
            : CheckAnyExtends<T, Rest>
        : false;

type Unique<T extends any[], Result extends any[] = []> =
    T extends [infer First, ...infer Rest]
        ? CheckAnyExtends<First, Result> extends true
            ? Unique<Rest, Result>
            : Unique<Rest, [...Result, First]>
        : Result;

type IsUnique<T extends any[]> = T["length"] extends Unique<T>["length"]
    ? true
    : false;

type Validate<T extends Reindeer[][][]> =
    IsUnique<[...T[0][0], ...T[0][1], ...T[0][2]]> extends false ? false :
    IsUnique<[...T[1][0], ...T[1][1], ...T[1][2]]> extends false ? false :
    IsUnique<[...T[2][0], ...T[2][1], ...T[2][2]]> extends false ? false :
    IsUnique<[...T[3][0], ...T[3][1], ...T[3][2]]> extends false ? false :
    IsUnique<[...T[4][0], ...T[4][1], ...T[4][2]]> extends false ? false :
    IsUnique<[...T[5][0], ...T[5][1], ...T[5][2]]> extends false ? false :
    IsUnique<[...T[6][0], ...T[6][1], ...T[6][2]]> extends false ? false :
    IsUnique<[...T[7][0], ...T[7][1], ...T[7][2]]> extends false ? false :
    IsUnique<[...T[8][0], ...T[8][1], ...T[8][2]]> extends false ? false :
    IsUnique<[T[0][0][0], T[1][0][0], T[2][0][0], T[3][0][0], T[4][0][0], T[5][0][0], T[6][0][0], T[7][0][0], T[8][0][0]]> extends false ? false :
    IsUnique<[T[0][0][1], T[1][0][1], T[2][0][1], T[3][0][1], T[4][0][1], T[5][0][1], T[6][0][1], T[7][0][1], T[8][0][1]]> extends false ? false :
    IsUnique<[T[0][0][2], T[1][0][2], T[2][0][2], T[3][0][2], T[4][0][2], T[5][0][2], T[6][0][2], T[7][0][2], T[8][0][2]]> extends false ? false :
    IsUnique<[T[0][1][0], T[1][1][0], T[2][1][0], T[3][1][0], T[4][1][0], T[5][1][0], T[6][1][0], T[7][1][0], T[8][1][0]]> extends false ? false :
    IsUnique<[T[0][1][1], T[1][1][1], T[2][1][1], T[3][1][1], T[4][1][1], T[5][1][1], T[6][1][1], T[7][1][1], T[8][1][1]]> extends false ? false :
    IsUnique<[T[0][1][2], T[1][1][2], T[2][1][2], T[3][1][2], T[4][1][2], T[5][1][2], T[6][1][2], T[7][1][2], T[8][1][2]]> extends false ? false :
    IsUnique<[T[0][2][0], T[1][2][0], T[2][2][0], T[3][2][0], T[4][2][0], T[5][2][0], T[6][2][0], T[7][2][0], T[8][2][0]]> extends false ? false :
    IsUnique<[T[0][2][1], T[1][2][1], T[2][2][1], T[3][2][1], T[4][2][1], T[5][2][1], T[6][2][1], T[7][2][1], T[8][2][1]]> extends false ? false :
    IsUnique<[T[0][2][2], T[1][2][2], T[2][2][2], T[3][2][2], T[4][2][2], T[5][2][2], T[6][2][2], T[7][2][2], T[8][2][2]]> extends false ? false :
    IsUnique<[...T[0][0], ...T[1][0], ...T[2][0]]> extends false ? false :
    IsUnique<[...T[0][1], ...T[1][1], ...T[2][1]]> extends false ? false :
    IsUnique<[...T[0][2], ...T[1][2], ...T[2][2]]> extends false ? false :
    IsUnique<[...T[3][0], ...T[4][0], ...T[5][0]]> extends false ? false :
    IsUnique<[...T[3][1], ...T[4][1], ...T[5][1]]> extends false ? false :
    IsUnique<[...T[3][2], ...T[4][2], ...T[5][2]]> extends false ? false :
    IsUnique<[...T[6][0], ...T[7][0], ...T[8][0]]> extends false ? false :
    IsUnique<[...T[6][1], ...T[7][1], ...T[8][1]]> extends false ? false :
    IsUnique<[...T[6][2], ...T[7][2], ...T[8][2]]> extends false ? false :
    true;

// Тесты
type test_sudoku_1_actual = Validate<[
  [['💨', '💃', '🦌'], ['☄️', '❤️', '🌩️'], ['🌟', '⚡', '🔴']],
  [['🌟', '⚡', '🔴'], ['💨', '💃', '🦌'], ['☄️', '❤️', '🌩️']],
  [['☄️', '❤️', '🌩️'], ['🌟', '⚡', '🔴'], ['💨', '💃', '🦌']],
  [['🦌', '💨', '💃'], ['⚡', '☄️', '❤️'], ['🔴', '🌩️', '🌟']],
  [['🌩️', '🔴', '🌟'], ['🦌', '💨', '💃'], ['⚡', '☄️', '❤️']],
  [['⚡', '☄️', '❤️'], ['🌩️', '🔴', '🌟'], ['🦌', '💨', '💃']],
  [['💃', '🦌', '💨'], ['❤️', '🌟', '☄️'], ['🌩️', '🔴', '⚡']],
  [['🔴', '🌩️', '⚡'], ['💃', '🦌', '💨'], ['❤️', '🌟', '☄️']],
  [['❤️', '🌟', '☄️'], ['🔴', '🌩️', '⚡'], ['💃', '🦌', '💨']]
]>;
type test_sudoku_1 = Expect<Equal<test_sudoku_1_actual, true>>;

type test_sudoku_2_actual = Validate<[
  [['🌩️', '💨', '☄️'], ['🌟', '🦌', '⚡'], ['❤️', '🔴', '💃']],
  [['🌟', '⚡', '❤️'], ['🔴', '💃', '☄️'], ['🌩️', '💨', '🦌']],
  [['🔴', '🦌', '💃'], ['💨', '❤️', '🌩️'], ['🌟', '⚡', '☄️']],
  [['❤️', '☄️', '🌩️'], ['💃', '⚡', '🔴'], ['💨', '🦌', '🌟']],
  [['🦌', '💃', '⚡'], ['🌩️', '🌟', '💨'], ['🔴', '☄️', '❤️']],
  [['💨', '🌟', '🔴'], ['🦌', '☄️', '❤️'], ['⚡', '💃', '🌩️']],
  [['☄️', '🔴', '💨'], ['❤️', '🌩️', '🦌'], ['💃', '🌟', '⚡']],
  [['💃', '❤️', '🦌'], ['⚡', '🔴', '🌟'], ['☄️', '🌩️', '💨']],
  [['⚡', '🌩️', '🌟'], ['☄️', '💨', '💃'], ['🦌', '❤️', '🔴']]
]>;
type test_sudoku_2 = Expect<Equal<test_sudoku_2_actual, true>>;

type test_sudoku_3_actual = Validate<[
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🌟', '⚡', '💨'], ['❤️', '💃', '🔴'], ['☄️', '🌩️', '🦌']],
  [['☄️', '🌩️', '❤️'], ['⚡', '🌟', '🦌'], ['💃', '🔴', '💨']],
  [['🌩️', '💃', '🔴'], ['🦌', '💨', '⚡'], ['🌟', '☄️', '❤️']],
  [['❤️', '☄️', '⚡'], ['💃', '🌩️', '🌟'], ['🦌', '💨', '🔴']],
  [['💨', '🌟', '🦌'], ['☄️', '🔴', '❤️'], ['🌩️', '💃', '⚡']],
  [['💃', '💨', '🌟'], ['🔴', '🦌', '☄️'], ['❤️', '⚡', '🌩️']],
  [['🔴', '❤️', '☄️'], ['🌟', '⚡', '🌩️'], ['💨', '🦌', '💃']],
  [['⚡', '🦌', '🌩️'], ['💨', '❤️', '💃'], ['🔴', '🌟', '☄️']]
]>;
type test_sudoku_3 = Expect<Equal<test_sudoku_3_actual, true>>;

type test_sudoku_4_actual = Validate<[
  [['💨', '💃', '🦌'], ['☄️', '❤️', '🌩️'], ['🌟', '⚡', '🔴']],
  [['🌟', '⚡', '🔴'], ['💨', '💃', '🦌'], ['☄️', '❤️', '🌩️']],
  [['☄️', '❤️', '🌩️'], ['🌟', '⚡', '🔴'], ['💨', '💃', '🦌']],
  [['🦌', '💨', '💃'], ['⚡', '☄️', '❤️'], ['🔴', '🌩️', '🌟']],
  [['🌩️', '🔴', '🌟'], ['🦌', '💨', '💃'], ['⚡', '☄️', '❤️']],
  [['⚡', '☄️', '❤️'], ['🌩️', '🔴', '🌟'], ['🦌', '💨', '💃']],
  [['💃', '🦌', '💨'], ['❤️', '🌟', '☄️'], ['⚡', '🔴', '🌟']],
  [['🔴', '🌩️', '⚡'], ['💃', '🦌', '💨'], ['❤️', '🌟', '☄️']],
  [['❤️', '🌟', '☄️'], ['🔴', '🌩️', '⚡'], ['💃', '🦌', '💨']]
]>;
type test_sudoku_4 = Expect<Equal<test_sudoku_4_actual, false>>;

type test_sudoku_5_actual = Validate<[
  [['🌩️', '💨', '☄️'], ['🌟', '🦌', '⚡'], ['❤️', '🔴', '💃']],
  [['🌟', '⚡', '❤️'], ['🔴', '💃', '☄️'], ['🌩️', '💨', '🦌']],
  [['🔴', '🦌', '💃'], ['💨', '❤️', '🌩️'], ['🌟', '⚡', '☄️']],
  [['❤️', '☄️', '🌩️'], ['💃', '⚡', '🔴'], ['💨', '🦌', '🌟']],
  [['🦌', '💃', '⚡'], ['🌩️', '🌟', '💨'], ['🔴', '☄️', '❤️']],
  [['💨', '🌟', '🔴'], ['🦌', '☄️', '❤️'], ['⚡', '💃', '🌩️']],
  [['☄️', '🔴', '💨'], ['❤️', '💃', '🦌'], ['💃', '🌟', '⚡']],
  [['💃', '❤️', '🦌'], ['⚡', '🔴', '🌟'], ['☄️', '🌩️', '💨']],
  [['⚡', '🌩️', '🌟'], ['☄️', '💨', '💃'], ['🦌', '❤️', '🔴']]
]>;
type test_sudoku_5 = Expect<Equal<test_sudoku_5_actual, false>>;

type test_sudoku_6_actual = Validate<[
  [['⚡', '🔴', '🌩️'], ['🦌', '❤️', '💨'], ['💨', '🌟', '☄️']],
  [['❤️', '🦌', '🌟'], ['💨', '🌟', '🔴'], ['💃', '⚡', '🌩️']],
  [['💨', '💃', '🌟'], ['☄️', '⚡', '🌩️'], ['🔴', '❤️', '🦌']],
  [['🦌', '⚡', '🔴'], ['❤️', '💃', '💨'], ['☄️', '🌩️', '🌟']],
  [['🌟', '🌩️', '💃'], ['⚡', '🔴', '☄️'], ['❤️', '🦌', '💨']],
  [['☄️', '💨', '❤️'], ['🌟', '🌩️', '🦌'], ['⚡', '💃', '🔴']],
  [['🌩️', '☄️', '💨'], ['💃', '🦌', '⚡'], ['🌟', '🔴', '❤️']],
  [['🔴', '❤️', '⚡'], ['🌩️', '☄️', '🌟'], ['🦌', '💨', '💃']],
  [['💃', '🌟', '🦌'], ['🔴', '💨', '❤️'], ['🌩️', '☄️', '⚡']]
]>;
type test_sudoku_6 = Expect<Equal<test_sudoku_6_actual, false>>;

type test_sudoku_7_actual = Validate<[
  [['💨', '💃', '🦌'], ['☄️', '❤️', '🌩️'], ['🌟', '⚡', '🔴']],
  [['💃', '🦌', '☄️'], ['❤️', '🌩️', '🌟'], ['⚡', '🔴', '💨']],
  [['🦌', '☄️', '❤️'], ['🌩️', '🌟', '⚡'], ['🔴', '💨', '💃']],
  [['☄️', '❤️', '🌩️'], ['🌟', '⚡', '🔴'], ['💨', '💃', '🦌']],
  [['❤️', '🌩️', '🌟'], ['⚡', '🔴', '💨'], ['💃', '🦌', '☄️']],
  [['🌩️', '🌟', '⚡'], ['🔴', '💨', '💃'], ['🦌', '☄️', '❤️']],
  [['🌟', '⚡', '🔴'], ['💨', '💃', '🦌'], ['☄️', '❤️', '🌩️']],
  [['⚡', '🔴', '💨'], ['💃', '🦌', '☄️'], ['❤️', '🌩️', '🌟']],
  [['🔴', '💨', '💃'], ['🦌', '☄️', '❤️'], ['🌩️', '🌟', '⚡']]
]>;
type test_sudoku_7 = Expect<Equal<test_sudoku_7_actual, false>>;

type test_sudoku_8_actual = Validate<[
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']],
  [['🦌', '🔴', '💃'], ['🌩️', '☄️', '💨'], ['⚡', '❤️', '🌟']]
]>;
type test_sudoku_8 = Expect<Equal<test_sudoku_8_actual, false>>;
```