# Бернард представляет данные о жилье

## 🇺🇸 Описание

> [🎩 Bernard (the head elf) presenting data to 🎅 Santa showing how much harder it is for the
> Reindeer to make a living with their wages not changing since 2009 at the same time as housing
> prices rising dramatically AND high inflation over that period.]
>
> [🎅 Santa] FAKE DATA!! THIS IS FAKE DATA!!!!! A FAKE DATA WITCH HUNT!!!
>
> [🎩 Bernard] Oh, sure, 🎅 Santa, I spent all night cooking the books just to screw over
> a guy who still thinks Excel is a type of sleigh polish.
>
> [🎅 Santa] I've been around the block a few times, 🎩 Bernard.
> I know the trick you're pulling here. We're a seasonal business.
> You only gave me numbers for Q1 of every year.
> But that's when our economy is always depressed after the height of the holiday season!
>
> [🎩 Bernard] I can't believe they let you run this place... 🎅 Santa.. 🎅 SANTA!!...
> We maintain a huge budgetary deficit that literally only ever seems to get larger and larger.
> These numbers are not affected by the seasonality of our business.
>
> [🎅 Santa] I want to be able to give quarterly numbers. And since 1975.
>
> [🎩 Bernard] This is the last time I'm doing this for you. You're going to need to face the music.. and pretty soon!

When 🎅 Santa uses this function, sometimes he'll want to pass a number, like `survivalRatio(2009)`
and if he wants a specific quarter he'll pass `survivalRatio('2009 Q2')`.
Our function needs to be able to handle both of these data types, not just numbers.

## 🇷🇺 Описание

> [🎩 Бернард (главный эльф) показывает 🎅 Санте данные, доказывающие, насколько сложнее стало оленям жить на
> ту же зарплату, которая не менялась с 2009 года, в то время как цены на жильё выросли драматически,
> а инфляция также была высокой в этот период.]
>
> [🎅 Санта] ПОДДЕЛЬНЫЕ ДАННЫЕ!! ЭТО ПОДДЕЛЬНЫЕ ДАННЫЕ!!! ОХОТА НА ВЕДЬМ С ПОДДЕЛЬНЫМИ ДАННЫМИ!!!
>
> [🎩 Бернард] Ну конечно, 🎅 Санта, я потратил всю ночь, подтасовывая цифры, только чтобы насолить парню,
> который до сих пор считает, что Excel — это разновидность полироли для саней.
>
> [🎅 Санта] Я многое повидал в своей жизни, 🎩 Бернард. Я знаю, какой трюк ты тут пытаешься провернуть.
> У нас сезонный бизнес. Ты дал мне только данные за первый квартал каждого года.
> А ведь это период, когда наша экономика всегда находится в упадке после пика праздничного сезона!
>
> [🎩 Бернард] Не могу поверить, что тебе вообще позволили управлять этим местом... 🎅 Санта... 🎅 САНТА!!
> Мы держимся на огромном бюджетном дефиците, который буквально становится всё больше и больше.
> Эти цифры никак не связаны с сезонностью нашего бизнеса.
>
> [🎅 Санта] Я хочу видеть данные по кварталам. И начиная с 1975 года.
>
> [🎩 Бернард] Это последний раз, когда я делаю это для тебя.
> Тебе придётся взглянуть правде в глаза... и довольно скоро!

Когда 🎅 Санта будет использовать эту функцию, иногда он захочет передать число, например, `survivalRatio(2009)`.
А если ему нужен определённый квартал, он передаст `survivalRatio('2009 Q2')`.
Наша функция должна уметь работать с обоими типами данных, а не только с числами.

## 💻 Решение

Для решения задачи типизируем `input` как объединение `string | number`.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
const survivalRatio = (input: string | number) => {
    const quarter = typeof input === 'string' ? input : `${input} Q1`;
    const data = quarterlyData[quarter];
    if (!data) {
        throw new Error("Data not found");
    }
    return data.housingIndex / data.minimumWage;
}

type QuarterlyData = {
    [key: string]: {
        housingIndex: number;
        minimumWage: number;
    };
}

const quarterlyData: QuarterlyData = {
    "1975 Q1": {housingIndex: 100, minimumWage: 100},
    "1975 Q2": {housingIndex: 100.08193, minimumWage: 98.79568},
    "1975 Q3": {housingIndex: 98.21265, minimumWage: 96.98526},
    "1975 Q4": {housingIndex: 98.33523, minimumWage: 95.38534},
    ...
}

// Тесты
const start = survivalRatio(2009);
type t0_actual = typeof start;
type t0_expected = number;
type t0 = Expect<Equal<t0_actual, t0_expected>>;

const now = survivalRatio(2024);
type t1_actual = typeof now;
type t1_expected = number;
type t1 = Expect<Equal<t1_actual, t1_expected>>;

const q1_2009 = survivalRatio('2009 Q1');
type t2_actual = typeof q1_2009;
type t2_expected = number;
type t2 = Expect<Equal<t2_actual, t2_expected>>;

const q2_2024 = survivalRatio('2024 Q2');
type t3_actual = typeof q2_2024;
type t3_expected = number;
type t3 = Expect<Equal<t3_actual, t3_expected>>;

// @ts-expect-error
survivalRatio(true);

// @ts-expect-error
survivalRatio([1]);

// @ts-expect-error
survivalRatio({1: 1});
```