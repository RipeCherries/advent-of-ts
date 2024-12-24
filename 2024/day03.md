# Санта хочет данные о жилье

## 🇺🇸 Описание

> [🎅 Santa and 🎩 Bernard (the head elf) arguing about the fairness of the reindeer demanding a raise]
>
> [🎅 Santa] ABSO-FRICKIN-LOUTLEY NOT!!
>
> [🎩 Bernard] Look. They literally are starving out there.
> You haven't raised the minimum wage at the North Pole since 2009 and they're beginning to realize that
> if you take inflation into account they're making half of what they did then.
> Then if you consider how much housing has gone up over the same time it all adds up to a..
>
> [🎅 Santa] I don't want to hear another word of this! NOT ONE MORE WORD! If I could,
> I'd send some of them to prison for their piss poor job performance!!
>
> [🎩 Bernard] It's not gonna be simple to keep this going.
>
> [🎅 Santa] I don’t have time to crunch numbers, Bernard! I’m trying to figure out if eggnog futures are still a thing.
>
> [🎩 Bernard] Alright, you know what. Because we go way back, you and me,
> I'll do you a favor and make you a program that will calculate their cost of living for a given year since 2009.
>
> [🎅 Santa] GOOD! Because this operation is hanging by a thread!
> The elves are threatening to unionize, the reindeer are planning a hunger strike, and Mrs. Claus wants a Peloton.

The function 🎩 Bernard created works for numbers, but it also accepts other data types like strings,
booleans, arrays, and objects. That's not quite what we want! TypeScript can help us here.

## 🇷🇺 Описание

> [🎅 Санта и 🎩 Бернард (главный эльф) спорят о справедливости требований оленей о повышении зарплаты.]
>
> [🎩 Бернард] Послушай. Они буквально голодают. Ты не повышал минимальную зарплату на Северном полюсе с 2009 года,
> и они начали понимать, что с учётом инфляции они зарабатывают вдвое меньше, чем тогда.
> А если учесть, насколько подорожало жильё за это же время, всё сводится к...
>
> [🎅 Санта] Я не хочу слышать больше ни слова об этом! НИ ЕДИНОГО СЛОВА!
> Если бы мог, я бы отправил некоторых из них в тюрьму за их отвратительное выполнение работы!
>
> [🎩 Бернард] Продолжать так дальше будет непросто.
>
> [🎅 Санта] У меня нет времени заниматься расчётами, Бернард!
> Я пытаюсь выяснить, остались ли ещё фьючерсы на гоголь-моголь.
>
> [🎩 Бернард] Хорошо, знаешь что. Потому что мы с тобой давно знакомы, я сделаю тебе одолжение и напишу программу,
> которая будет рассчитывать их стоимость жизни за любой год, начиная с 2009.
>
> [🎅 Санта] ОТЛИЧНО! Потому что всё это предприятие висит на волоске! Эльфы угрожают создать профсоюз,
> олени планируют голодовку, а миссис Клаус хочет себе велотренажёр Peloton.

Функция, которую 🎩 Бернард написал, работает с числами, но также принимает другие типы данных,
такие как строки, логические значения, массивы и объекты. Это не совсем то, что нам нужно!
TypeScript может помочь нам с этим.

## 💻 Решение

Типизируем параметр `input` как `number`, чтобы функция могла принимать только числа.

```typescript
// Решение
const survivalRatio = (input: number) => {
    const data = annualData[input];
    if (!data) {
        throw new Error("Data not found");
    }
    return data.housingIndex / data.minimumWage;
}

type AnnualData = {
    [key: string]: {
        housingIndex: number;
        minimumWage: number;
    };
}

const annualData: AnnualData = {
    2009: {housingIndex: 159.50891, minimumWage: 92.85234},
    2010: {housingIndex: 143.02079, minimumWage: 100.50286},
    2011: {housingIndex: 134.38007, minimumWage: 98.68833},
    2012: {housingIndex: 128.14281, minimumWage: 96.31795},
    2013: {housingIndex: 129.07457, minimumWage: 94.94066},
    2014: {housingIndex: 133.94671, minimumWage: 93.72707},
    2015: {housingIndex: 143.30408, minimumWage: 93.59833},
    2016: {housingIndex: 150.21623, minimumWage: 92.9189},
    2017: {housingIndex: 154.90384, minimumWage: 91.06557},
    2018: {housingIndex: 161.67095, minimumWage: 89.39745},
    2019: {housingIndex: 167.71417, minimumWage: 88.11883},
    2020: {housingIndex: 173.5093, minimumWage: 86.81302},
    2021: {housingIndex: 182.89259, minimumWage: 85.10033},
    2022: {housingIndex: 199.43678, minimumWage: 79.80175},
    2023: {housingIndex: 205.8372, minimumWage: 75.95666},
    2024: {housingIndex: 214.9681, minimumWage: 73.98181},
}

// Тесты
export const reportForSanta = {
    2009: survivalRatio(2009),
    2010: survivalRatio(2010),
    2011: survivalRatio(2011),
    2012: survivalRatio(2012),
    2013: survivalRatio(2013),
    2014: survivalRatio(2014),
    2015: survivalRatio(2015),
    2016: survivalRatio(2016),
    2017: survivalRatio(2017),
    2018: survivalRatio(2018),
    2019: survivalRatio(2019),
    2020: survivalRatio(2020),
    2021: survivalRatio(2021),
    2022: survivalRatio(2022),
    2023: survivalRatio(2023),
}

// @ts-expect-error
survivalRatio('1');

// @ts-expect-error
survivalRatio(true);

// @ts-expect-error
survivalRatio([1]);

// @ts-expect-error
survivalRatio({1: 1});
```