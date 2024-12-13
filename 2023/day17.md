# Камень, Ножницы, Бумага

## 🇺🇸 Описание

It's Sunday and there's one week to go before the big day (Christmas Eve) when the elfs'
work for the year will finally be complete. For the last 20 years the only game the elves have had to play
together is StarCraft. They're looking for a fresh game to play.

So, they get the idea to try a Rock, Paper, Scissors tournament.

But the elves are sorta nerdy so they want to accomplish this using TypeScript types.
The `WhoWins` should type to correctly determine the winner in a Rock-Paper-Scissors game.
The first argument is the opponent and the second argument is you!

In case you haven't played it before, basically:

* it's a two player game where each player picks one of three options: Rock (👊), Paper (✋), and Scissors (✌️);
* game rules:
    * Rock crushes Scissors (Rock wins);
    * Scissors cuts Paper (Scissors wins);
    * Paper covers Rock (Paper wins);
    * otherwise, a draw.

## 🇷🇺 Описание

Сегодня воскресенье, и до знаменательного дня (кануна Рождества), когда работа эльфов за год будет наконец
завершена, осталась одна неделя. За последние 20 лет единственной игрой, в которую эльфам приходилось играть вместе,
была StarCraft. Им хочется поиграть во что-нибудь новенькое.

И вот им приходит идея устроить турнир «Камень, ножницы, бумага».

Но, будучи немного заучками, они решили реализовать это с помощью типов TypeScript.
Тип `WhoWins` должен правильно определять победителя в игре "Камень, Ножницы, Бумага".
Первый аргумент — это ход соперника, а второй аргумент — ваш ход.

Если вы никогда не играли, вот суть игры:

* Это игра для двух игроков, где каждый выбирает один из трёх вариантов: Камень (👊), Бумага (✋) или Ножницы (✌️);
* Правила игры:
    * Камень бьёт Ножницы (побеждает Камень);
    * Ножницы режут Бумагу (побеждают Ножницы);
    * Бумага накрывает Камень (побеждает Бумага);
    * Если оба выбрали одно и то же, это ничья.

## 💻 Решение

Для создания типа `WhoWins` используем маппинг через условные проверки (`extends`).

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type RockPaperScissors = "👊" | "✋" | "✌️";

type WhoWins<
    TOpponent extends RockPaperScissors,
    TYou extends RockPaperScissors
> = TOpponent extends TYou
    ? "draw"
    : TOpponent extends "👊"
        ? TYou extends "✋"
            ? "win"
            : "lose"
        : TOpponent extends "✋"
            ? TYou extends "👊"
                ? "lose"
                : "win"
            : TYou extends "👊"
                ? "win"
                : "lose";

// Тесты
type test_0_actual = WhoWins<'👊', '✋'>;
type test_0_expected = 'win';
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = WhoWins<'👊', '✌️'>;
type test_1_expected = 'lose';
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type test_2_actual = WhoWins<'👊', '👊'>;
type test_2_expected = 'draw';
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;

type test_3_actual = WhoWins<'✋', '👊'>;
type test_3_expected = 'lose';
type test_3 = Expect<Equal<test_3_expected, test_3_actual>>;

type test_4_actual = WhoWins<'✋', '✌️'>;
type test_4_expected = 'win';
type test_4 = Expect<Equal<test_4_expected, test_4_actual>>;

type test_5_actual = WhoWins<'✋', '✋'>;
type test_5_expected = 'draw';
type test_5 = Expect<Equal<test_5_expected, test_5_actual>>;

type test_6_actual = WhoWins<'✌️', '👊'>;
type test_6_expected = 'win';
type test_6 = Expect<Equal<test_6_expected, test_6_actual>>;

type test_7_actual = WhoWins<'✌️', '✌️'>;
type test_7_expected = 'draw';
type test_7 = Expect<Equal<test_7_expected, test_7_actual>>;

type test_8_actual = WhoWins<'✌️', '✋'>;
type test_8_expected = 'lose';
type test_8 = Expect<Equal<test_8_expected, test_8_actual>>;
```