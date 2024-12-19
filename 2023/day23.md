# Connect 4, но на TypeScript типах

## 🇺🇸 Описание

Your goal for this challenge is to implement Connect 4 in TypeScript types.

Each cell in the game can contain `🔴` or `🟡` or be empty (` `).
You're provided with a rough layout of how to organize the board in the EmptyBoard type.
The game state is represented by an object with a board property and a state property
(which keeps track of which player is next up to play).

In case you haven't played it before: Connect 4 is a game in which the players choose a color and then take
turns dropping colored tokens into a six-row, seven-column vertically suspended grid. The pieces fall straight down,
occupying the lowest available space within the column. The objective of the game is to be the first to form a
horizontal, vertical, or diagonal line of four of one's own tokens.

> **fun fact**: Connect 4 is also known as Connect Four, Four Up, Plot Four, Find Four, Captain's Mistress,
> Four in a Row, Drop Four, and Gravitrips in the Soviet Union

> **another fun fact**: Connect 4 was "solved" by James Allen and Victor Allis (independently from one another..
> like two weeks apart!) in 1988. They couldn't do a full brute-force proof at the time, but 7 years
> later John Tromp in the Netherlands did it with a database on a Sun Microsystems and
> Silicon Graphics International worksations (for a combined total of 40,000 computation hours!).

## 🇷🇺 Описание

Ваша цель в этом задании — реализовать игру Connect 4 с использованием типов TypeScript.

Каждая клетка в игре может содержать `🔴` или `🟡` или быть пустой (` `).
В типе `EmptyBoard` вам предоставляется примерная схема организации игрового поля.
Состояние игры представлено объектом со свойством `board` и свойством `state `
(которое отслеживает, кто из игроков будет играть следующим).

Если вы не играли раньше: Connect 4 — это игра, в которой игроки выбирают цвет и поочередно бросают
фишки выбранного цвета в вертикальную решетку размером шесть рядов и семь столбцов.
Фишки падают вертикально вниз, занимая самое низкое доступное пространство в столбце.
Цель игры — первым сформировать горизонтальную, вертикальную или диагональную линию из четырех своих фишек.

> **Забавный факт**: Connect 4 также известна под названиями Connect Four, Four Up, Plot Four, Find Four,
> Captain's Mistress, Four in a Row, Drop Four, и Gravitrips в Советском Союзе.

> **Еще один забавный факт**: Connect 4 была "решена" Джеймсом Алленом и Виктором Аллисом
> (независимо друг от друга, с разницей всего в две недели!) в 1988 году.
> Полное доказательство перебором тогда сделать не удалось, но через семь лет Джон Тромп
> из Нидерландов смог это сделать, используя базу данных на рабочих станциях Sun Microsystems и
> Silicon Graphics International (в общей сложности затратив 40 000 часов вычислений!).

## 💻 Решение

```typescript
import { Expect, Equal } from "type-testing";

// Решение
type Connect4Empty = "  ";
type Connect4Chips = "🔴" | "🟡";
type Connect4Cell = Connect4Chips | Connect4Empty;
type Connect4Board = Connect4Cell[][];

type EmptyBoard = [
    ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
    ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
    ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
    ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
    ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
    ["  ", "  ", "  ", "  ", "  ", "  ", "  "]
];

type NewGame = {
    board: EmptyBoard;
    state: "🟡";
};

type Connect4Game = { board: Connect4Board; state: Connect4Chips };

type Connect4<
    T extends Connect4Game,
    C extends number,
    R extends number = FindIndex2D<T["board"], C>,
    B extends Connect4Board = Replace2D<T["board"], R, C, T["state"]>
> =
    CheckRows<B, T['state']> extends true ? { board: B, state: `${T['state']} Won` } :
        CheckCols<B, T['state'], C> extends true ? { board: B, state: `${T['state']} Won` } :
            CheckDiagLeftToRight<B, T['state']> extends true ? { board: B, state: `${T['state']} Won` } :
                CheckDiagRightToLeft<B, T['state']> extends true ? { board: B, state: `${T['state']} Won` } :
                    CheckDraw<B> extends true ? { board: B, state: "Draw" } :
                        { board: B, state: { "🔴": "🟡"; "🟡": "🔴" }[T['state']] };

type CheckDraw<T extends any[][]> =
    T extends [infer First extends any[], ...infer Rest extends any[][]]
        ? HasEmpty1D<First> extends true
            ? false
            : CheckDraw<Rest>
        : true;

type HasEmpty1D<T extends any[]> =
    T extends [infer First, ...infer Rest]
        ? First extends Connect4Empty
            ? true
            : HasEmpty1D<Rest>
        : false

type FindIndex2D<T extends Connect4Board, P extends number, Acc extends 0[] = []> =
    T extends [...infer Rest extends any[][], infer Last extends any[]]
        ? Last[P] extends "  "
            ? Acc["length"]
            : FindIndex2D<Rest, P, [0, ...Acc]>
        : never;

type Replace2D<T extends any[][], IR extends number, IC extends number, V extends any, Acc extends any[][] = []> =
    T extends [...infer Rest extends any[][], infer Last extends any[]]
        ? Acc["length"] extends IR
            ? [...Rest, Replace1D<Last, IC, V>, ...Acc]
            : Replace2D<Rest, IR, IC, V, [Last, ...Acc]>
        : T;

type Replace1D<T extends any[], I extends number, V extends any, Acc extends any[] = []> =
    T extends [infer First, ...infer Rest]
        ? Acc["length"] extends I
            ? [...Acc, V, ...Rest]
            : Replace1D<Rest, I, V, [...Acc, First]>
        : T;

type CheckRows<T extends any[][], V extends Connect4Chips> =
    T extends [infer First extends any[], ...infer Rest extends any[][]]
        ? CheckWin1D<First, V> extends true
            ? true
            : CheckRows<Rest, V>
        : false;

type CheckCols<
    T extends any[][],
    V extends Connect4Chips,
    P extends number
> = T extends [
        infer R1 extends any[],
        infer R2 extends any[],
        infer R3 extends any[],
        infer R4 extends any[],
        ...infer Rest extends any[][]
    ]
    ? [R1[P], R2[P], R3[P], R4[P]] extends [V, V, V, V]
        ? true
        : CheckCols<[R2, R3, R4, ...Rest], V, P>
    : false;

type CheckDiagLeftToRight<
    T extends any[][],
    V extends Connect4Chips
> = T extends [
        infer R1 extends any[],
        infer R2 extends any[],
        infer R3 extends any[],
        infer R4 extends any[],
        ...infer Rest extends any[][]
    ]
    ? R2 extends [any, ...infer R2Rest]
        ? R3 extends [any, any, ...infer R3Rest]
            ? R4 extends [any, any, any, ...infer R4Rest]
                ? CheckQuadRow<R1, R2Rest, R3Rest, R4Rest, V> extends true
                    ? true
                    : CheckDiagLeftToRight<[R2, R3, R4, ...Rest], V>
                : false
            : false
        : false
    : false;

type CheckDiagRightToLeft<
    T extends any[][],
    V extends Connect4Chips
> = T extends [
        infer R1 extends any[],
        infer R2 extends any[],
        infer R3 extends any[],
        infer R4 extends any[],
        ...infer Rest extends any[][]
    ]
    ? R3 extends [any, ...infer R3Rest]
        ? R2 extends [any, any, ...infer R2Rest]
            ? R1 extends [any, any, any, ...infer R1Rest]
                ? CheckQuadRow<R1Rest, R2Rest, R3Rest, R4, V> extends true
                    ? true
                    : CheckDiagRightToLeft<[R2, R3, R4, ...Rest], V>
                : false
            : false
        : false
    : false;

type CheckQuadRow<
    R1 extends any[],
    R2 extends any[],
    R3 extends any[],
    R4 extends any[],
    V extends any,
    Acc extends 0[] = [],
    i extends number = Acc["length"]
> = R1["length"] extends i
    ? false
    : [R1[i], R2[i], R3[i], R4[i]] extends [V, V, V, V]
        ? true
        : CheckQuadRow<R1, R2, R3, R4, V, [...Acc, 0]>;

type CheckWin1D<T extends any[], V extends any> =
    T extends [infer I1, infer I2, infer I3, infer I4, ...infer Rest]
        ? [I1, I2, I3, I4] extends [V, V, V, V]
            ? true
            : CheckWin1D<[I2, I3, I4, ...Rest], V>
        : false;

// Тесты
type test_move1_actual = Connect4<NewGame, 0>;
type test_move1_expected = {
    board: [
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
    ];
    state: "🔴";
};
type test_move1 = Expect<Equal<test_move1_actual, test_move1_expected>>;

type test_move2_actual = Connect4<test_move1_actual, 0>;
type test_move2_expected = {
    board: [
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🔴", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
    ];
    state: "🟡";
};
type test_move2 = Expect<Equal<test_move2_actual, test_move2_expected>>;

type test_move3_actual = Connect4<test_move2_actual, 0>;
type test_move3_expected = {
    board: [
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🔴", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
    ];
    state: "🔴";
};
type test_move3 = Expect<Equal<test_move3_actual, test_move3_expected>>;

type test_move4_actual = Connect4<test_move3_actual, 1>;
type test_move4_expected = {
    board: [
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🔴", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "🔴", "  ", "  ", "  ", "  ", "  "],
    ];
    state: "🟡";
};
type test_move4 = Expect<Equal<test_move4_actual, test_move4_expected>>;

type test_move5_actual = Connect4<test_move4_actual, 2>;
type test_move5_expected = {
    board: [
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🔴", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "🔴", "🟡", "  ", "  ", "  ", "  "],
    ];
    state: "🔴";
};
type test_move5 = Expect<Equal<test_move5_actual, test_move5_expected>>;

type test_move6_actual = Connect4<test_move5_actual, 1>;
type test_move6_expected = {
    board: [
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["  ", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "  ", "  ", "  ", "  ", "  ", "  "],
        ["🔴", "🔴", "  ", "  ", "  ", "  ", "  "],
        ["🟡", "🔴", "🟡", "  ", "  ", "  ", "  "],
    ];
    state: "🟡";
};
type test_move6 = Expect<Equal<test_move6_actual, test_move6_expected>>;

type test_red_win_actual = Connect4<
    {
        board: [
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['🟡', '  ', '  ', '  ', '  ', '  ', '  '],
            ['🟡', '  ', '  ', '  ', '  ', '  ', '  '],
            ['🔴', '🔴', '🔴', '  ', '  ', '  ', '  '],
            ['🟡', '🔴', '🟡', '🟡', '  ', '  ', '  ']
        ];
        state: '🔴';
    }
>;

type test_red_win_expected = {
    board: [
        ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
        ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
        ['🟡', '  ', '  ', '  ', '  ', '  ', '  '],
        ['🟡', '  ', '  ', '  ', '  ', '  ', '  '],
        ['🔴', '🔴', '🔴', '🔴', '  ', '  ', '  '],
        ['🟡', '🔴', '🟡', '🟡', '  ', '  ', '  ']
    ];
    state: '🔴 Won';
};
type test_red_win = Expect<Equal<test_red_win_actual, test_red_win_expected>>;

type test_yellow_win_actual = Connect4<
    {
        board: [
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['🔴', '  ', '  ', '  ', '  ', '  ', '  '],
            ['🟡', '  ', '  ', '  ', '  ', '  ', '  '],
            ['🔴', '  ', '🔴', '🔴', '  ', '  ', '  '],
            ['🟡', '  ', '🟡', '🟡', '  ', '  ', '  ']
        ];
        state: '🟡';
    }
>;

type test_yellow_win_expected = {
    board: [
        ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
        ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
        ['🔴', '  ', '  ', '  ', '  ', '  ', '  '],
        ['🟡', '  ', '  ', '  ', '  ', '  ', '  '],
        ['🔴', '  ', '🔴', '🔴', '  ', '  ', '  '],
        ['🟡', '🟡', '🟡', '🟡', '  ', '  ', '  ']
    ];
    state: '🟡 Won';
};
type test_yellow_win = Expect<Equal<test_yellow_win_actual, test_yellow_win_expected>>;

type test_diagonal_yellow_win_actual = Connect4<
    {
        board: [
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
            ['  ', '  ', '🟡', '🔴', '  ', '  ', '  '],
            ['🔴', '🟡', '🔴', '🔴', '  ', '  ', '  '],
            ['🟡', '🔴', '🟡', '🟡', '  ', '  ', '  ']
        ];
        state: '🟡';
    }
>;

type test_diagonal_yellow_win_expected = {
    board: [
        ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
        ['  ', '  ', '  ', '  ', '  ', '  ', '  '],
        ['  ', '  ', '  ', '🟡', '  ', '  ', '  '],
        ['  ', '  ', '🟡', '🔴', '  ', '  ', '  '],
        ['🔴', '🟡', '🔴', '🔴', '  ', '  ', '  '],
        ['🟡', '🔴', '🟡', '🟡', '  ', '  ', '  ']
    ];
    state: '🟡 Won';
};

type test_diagonal_yellow_win = Expect<Equal<test_diagonal_yellow_win_actual, test_diagonal_yellow_win_expected>>;

type test_draw_actual = Connect4<
    {
        board: [
            ['🟡', '🔴', '🔴', '🟡', '🟡', '🔴', '  '],
            ['🔴', '🟡', '🟡', '🔴', '🔴', '🟡', '🔴'],
            ['🟡', '🔴', '🔴', '🟡', '🟡', '🔴', '🟡'],
            ['🔴', '🟡', '🟡', '🔴', '🔴', '🟡', '🔴'],
            ['🟡', '🔴', '🔴', '🟡', '🟡', '🔴', '🟡'],
            ['🔴', '🟡', '🟡', '🔴', '🔴', '🟡', '🔴']
        ];
        state: '🟡';
    }
>;

type test_draw_expected = {
    board: [
        ['🟡', '🔴', '🔴', '🟡', '🟡', '🔴', '🟡'],
        ['🔴', '🟡', '🟡', '🔴', '🔴', '🟡', '🔴'],
        ['🟡', '🔴', '🔴', '🟡', '🟡', '🔴', '🟡'],
        ['🔴', '🟡', '🟡', '🔴', '🔴', '🟡', '🔴'],
        ['🟡', '🔴', '🔴', '🟡', '🟡', '🔴', '🟡'],
        ['🔴', '🟡', '🟡', '🔴', '🔴', '🟡', '🔴']
    ];
    state: 'Draw';
};
type test_draw = Expect<Equal<test_draw_actual, test_draw_expected>>;
```