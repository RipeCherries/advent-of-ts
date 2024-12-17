# Что такое Крестики-нолики?

## 🇺🇸 Описание

Tic-Tac-Toe is a two-player game where players alternate marking `❌`s and `⭕`s in a 3x3 grid,
aiming to get three in a row.

> **fun fact**: Did you know that tic tac toe is widely considered to be the first computer video game ever created?!
> That's right! A S Douglas implemented it all the way back in 1952,
> the same year as the coronation of Queen Elizabeth II.

Your goal for this challenge is to use TypeScript types to encode the game logic of Tic Tac Toe. Eventually,
every game will end with one of the players winning or a draw.

## 🇷🇺 Описание

Крестики-нолики — это игра для двух игроков, в которой участники по очереди ставят `❌` и `⭕` на поле размером 3x3,
стремясь составить линию из трех своих символов.

> **Занимательный факт**: Знали ли вы, что крестики-нолики считаются первой компьютерной видеоигрой,
> когда-либо созданной? Это правда! А. С. Дуглас реализовал её еще в далеком 1952 году, в том же году,
> когда состоялась коронация королевы Елизаветы II.

Ваша цель в этом испытании — использовать типы TypeScript для кодирования логики игры в крестики-нолики.
В конечном итоге каждая игра закончится либо победой одного из игроков, либо ничьей.

## 💻 Решение

Для решения данной задачи реализуем типы:

* `Retrieve` для получения значения конкретной ячейки доски на основе её позиции;
* `IsWinning` для проверки, есть ли у заданного игрока победная комбинация;
* `IsDraw` для проверки заполнены ли все ячейки (нет пустых), если победитель не определён;
* `NextState` для определения следующего состояния (чья-то победа, ничья или ход следующего игрока);
* `Put` для обновления состояния доски, добавляя фишку в указанную ячейку;
* `TicTacToe` для обработки хода игрока.

```typescript
import { Equal, Expect } from 'type-testing';

// Решение
type TicTacToeChip = '❌' | '⭕';
type TicTacToeEndState = '❌ Won' | '⭕ Won' | 'Draw';
type TicTacToeState = TicTacToeChip | TicTacToeEndState;
type TicTacToeEmptyCell = '  '
type TicTacToeCell = TicTacToeChip | TicTacToeEmptyCell;
type TicTacToeYPositions = 'top' | 'middle' | 'bottom';
type TicTacToeXPositions = 'left' | 'center' | 'right';
type TicTacToePositions = `${TicTacToeYPositions}-${TicTacToeXPositions}`;
type TicTacToeBoard = TicTacToeCell[][];
type TicTacToeGame = {
    board: TicTacToeBoard;
    state: TicTacToeState;
};

type EmptyBoard = [
    ['  ', '  ', '  '],
    ['  ', '  ', '  '],
    ['  ', '  ', '  ']
];

type NewGame = {
    board: EmptyBoard;
    state: '❌';
};

type ToIndex = {
    top: 0;
    middle: 1;
    bottom: 2;
    left: 0;
    center: 1;
    right: 2;
}

type Retrive<Board extends TicTacToeBoard, Position extends TicTacToePositions> =
    Position extends `${infer L extends TicTacToeYPositions}-${infer R extends TicTacToeXPositions}`
        ? Board[ToIndex[L]][ToIndex[R]]
        : never;

type IsWinning<Board extends TicTacToeBoard, Chip extends TicTacToeChip> =
    [Board[0][0], Board[0][1], Board[0][2]] extends [Chip, Chip, Chip] ? true :
        [Board[1][0], Board[1][1], Board[1][2]] extends [Chip, Chip, Chip] ? true :
            [Board[2][0], Board[2][1], Board[2][2]] extends [Chip, Chip, Chip] ? true :
                [Board[0][0], Board[1][0], Board[2][0]] extends [Chip, Chip, Chip] ? true :
                    [Board[0][1], Board[1][1], Board[2][1]] extends [Chip, Chip, Chip] ? true :
                        [Board[0][2], Board[1][2], Board[2][2]] extends [Chip, Chip, Chip] ? true :
                            [Board[0][0], Board[1][1], Board[2][2]] extends [Chip, Chip, Chip] ? true :
                                [Board[0][2], Board[1][1], Board[2][0]] extends [Chip, Chip, Chip] ? true :
                                    false;

type IsDraw<Board extends any[]> =
    Board extends [infer First, ...infer Rest]
        ? First extends TicTacToeEmptyCell
            ? false
            : IsDraw<Rest>
        : true

type NextState<Board extends TicTacToeBoard, Chip extends TicTacToeChip> =
    IsWinning<Board, Chip> extends true
        ? { board: Board, state: `${Chip} Won` }
        : IsDraw<[...Board[0], ...Board[1], ...Board[2]]> extends true
            ? { board: Board, state: 'Draw' }
            : { board: Board, state: { '❌': '⭕', '⭕': '❌' }[Chip] }

type Put<Board extends TicTacToeBoard, Position extends TicTacToePositions, Cell extends TicTacToeCell> =
    Position extends 'top-left'
        ? [[Cell, Board[0][1], Board[0][2]], Board[1], Board[2]]
        : Position extends 'top-center'
            ? [[Board[0][0], Cell, Board[0][2]], Board[1], Board[2]]
            : Position extends 'top-right'
                ? [[Board[0][0], Board[0][1], Cell], Board[1], Board[2]]
                : Position extends 'middle-left'
                    ? [Board[0], [Cell, Board[1][1], Board[1][2]], Board[2]]
                    : Position extends 'middle-center'
                        ? [Board[0], [Board[1][0], Cell, Board[1][2]], Board[2]]
                        : Position extends 'middle-right'
                            ? [Board[0], [Board[1][0], Board[1][1], Cell], Board[2]]
                            : Position extends 'bottom-left'
                                ? [Board[0], Board[1], [Cell, Board[2][1], Board[2][2]]]
                                : Position extends 'bottom-center'
                                    ? [Board[0], Board[1], [Board[2][0], Cell, Board[2][2]]]
                                    : Position extends 'bottom-right'
                                        ? [Board[0], Board[1], [Board[2][0], Board[2][1], Cell]]
                                        : never;

type TicTacToe<CurrentGame extends TicTacToeGame, Position extends TicTacToePositions> =
    Retrive<CurrentGame['board'], Position> extends TicTacToeEmptyCell
        ? CurrentGame['state'] extends TicTacToeChip
            ? NextState<Put<CurrentGame['board'], Position, CurrentGame['state']>, CurrentGame['state']>
            : never
        : CurrentGame;

// Тесты
type test_move1_actual = TicTacToe<NewGame, 'top-center'>;
type test_move1_expected = {
    board: [
        ['  ', '❌', '  '],
        ['  ', '  ', '  '],
        ['  ', '  ', '  ']
    ];
    state: '⭕';
};
type test_move1 = Expect<Equal<test_move1_actual, test_move1_expected>>;

type test_move2_actual = TicTacToe<test_move1_actual, 'top-left'>;
type test_move2_expected = {
    board: [
        ['⭕', '❌', '  '],
        ['  ', '  ', '  '],
        ['  ', '  ', '  ']];
    state: '❌';
}
type test_move2 = Expect<Equal<test_move2_actual, test_move2_expected>>;

type test_move3_actual = TicTacToe<test_move2_actual, 'middle-center'>;
type test_move3_expected = {
    board: [
        ['⭕', '❌', '  '],
        ['  ', '❌', '  '],
        ['  ', '  ', '  ']
    ];
    state: '⭕';
};
type test_move3 = Expect<Equal<test_move3_actual, test_move3_expected>>;

type test_move4_actual = TicTacToe<test_move3_actual, 'bottom-left'>;
type test_move4_expected = {
    board: [
        ['⭕', '❌', '  '],
        ['  ', '❌', '  '],
        ['⭕', '  ', '  ']
    ];
    state: '❌';
};
type test_move4 = Expect<Equal<test_move4_actual, test_move4_expected>>;

type test_x_win_actual = TicTacToe<test_move4_actual, 'bottom-center'>;
type test_x_win_expected = {
    board: [
        ['⭕', '❌', '  '],
        ['  ', '❌', '  '],
        ['⭕', '❌', '  ']
    ];
    state: '❌ Won';
};
type test_x_win = Expect<Equal<test_x_win_actual, test_x_win_expected>>;

type type_move5_actual = TicTacToe<test_move4_actual, 'bottom-right'>;
type type_move5_expected = {
    board: [
        ['⭕', '❌', '  '],
        ['  ', '❌', '  '],
        ['⭕', '  ', '❌']
    ];
    state: '⭕';
};
type test_move5 = Expect<Equal<type_move5_actual, type_move5_expected>>;

type test_o_win_actual = TicTacToe<type_move5_actual, 'middle-left'>;
type test_o_win_expected = {
    board: [
        ['⭕', '❌', '  '],
        ['⭕', '❌', '  '],
        ['⭕', '  ', '❌']
    ];
    state: '⭕ Won';
};

// invalid move don't change the board and state
type test_invalid_actual = TicTacToe<test_move1_actual, 'top-center'>;
type test_invalid_expected = {
    board: [
        ['  ', '❌', '  '],
        ['  ', '  ', '  '],
        ['  ', '  ', '  ']
    ];
    state: '⭕';
};
type test_invalid = Expect<Equal<test_invalid_actual, test_invalid_expected>>;

type test_before_draw = {
    board: [
        ['⭕', '❌', '⭕'],
        ['⭕', '❌', '❌'],
        ['❌', '⭕', '  ']];
    state: '⭕';
}
type test_draw_actual = TicTacToe<test_before_draw, 'bottom-right'>;
type test_draw_expected = {
    board: [
        ['⭕', '❌', '⭕'],
        ['⭕', '❌', '❌'],
        ['❌', '⭕', '⭕']];
    state: 'Draw';
}
type test_draw = Expect<Equal<test_draw_actual, test_draw_expected>>;
```