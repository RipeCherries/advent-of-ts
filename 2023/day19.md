# Помогите Санте отмыть деньги

## 🇺🇸 Описание

The shady WiFi installment by Santa's college buddy in Days 12 and 16 aren't the only questionable business
dealing Santa is involved in. Another of Santa's friends from college, Tod, is a partial owner of the X Games
(an "extreme sports" version of the Olympics). In recent years, Santa realized that he can use his position of
power at the toy factory to embezzle funds through a shell corporation that he started with Tod.
The shell corporation, Icecap Assets Management, Inc., recently acquired a skateboard and scooter manufacturer,
SkateScoot Syndicate. It's perfect timing because in 2022 Icecap had acquired another company that makes surfboards
and bmx bikes, RideWave Dynamics.

Now, all that's left to do is make sure that every child gets a skateboard or a scooter!
Then the funds will be laundered to Icecap via SkateScoot and RideWave, after which Santa and Tod can then take
total control of the funds.

Santa made himself a list like this:

```typescript
type List = [2, 1, 3, 3, 1, 2, 2, 1];
```

And since Santa doesn't want to raise suspicion (by giving the same thing to every kid)
he figures he'll alternate like this:

* '🛹' (skateboard);
* '🚲' (bmx bike);
* '🛴' (scooter);
* '🏄' (surfboard);
* (loop back to skateboard).

```typescript
type Result = [
    '🛹', '🛹',
    '🚲',
    '🛴', '🛴', '🛴',
    '🏄', '🏄', '🏄',
    '🛹',
    '🚲', '🚲',
    '🛴', '🛴',
    '🏄',
]
```

## 🇷🇺 Описание

Сомнительная установка Wi-Fi от приятеля Санты из колледжа в Дни 12 и 16 — далеко не единственное сомнительное дело,
в котором замешан Санта. Еще один его приятель из колледжа, Тод, частично владеет X Games
(это "экстремальная спортивная" версия Олимпийских игр). В последние годы Санта понял, что может использовать свое
влияние на фабрике игрушек, чтобы отмывать деньги через фиктивную корпорацию, которую он основал вместе с Тодом.
Эта корпорация, Icecap Assets Management, Inc., недавно приобрела производителя скейтбордов и самокатов под названием
SkateScoot Syndicate. Это как раз вовремя, потому что в 2022 году Icecap уже приобрела другую компанию, производящую
серфборды и велосипеды BMX, под названием RideWave Dynamics.

Теперь осталось только убедиться, что каждый ребенок получит скейтборд или самокат! Затем средства будут отмыты через
SkateScoot и RideWave, после чего Санта и Тод смогут полностью завладеть деньгами.

Санта составил себе такой список:

```typescript
type List = [2, 1, 3, 3, 1, 2, 2, 1];
```

И поскольку Санта не хочет привлекать подозрения (раздавая всем одно и то же),
он решил чередовать подарки следующим образом:

* '🛹' (скейтборд);
* '🚲' (велосипед BMX);
* '🛴' (самокат);
* '🏄' (серфборд);
* (и затем снова с начала).

Результат будет выглядеть так:

```typescript
type Result = [
    '🛹', '🛹',
    '🚲',
    '🛴', '🛴', '🛴',
    '🏄', '🏄', '🏄',
    '🛹',
    '🚲', '🚲',
    '🛴', '🛴',
    '🏄',
];
```

## 💻 Решение

Используем рекурсию для обработки массива и вспомогательный тип для смены индекса подарков.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type Gifts = ["🛹", "🚲", "🛴", "🏄"];
type GiftLooper<T extends number> = [1, 2, 3, 0][T];

type Rebuild<
    T extends Array<number>,
    I extends number = 0,
    Acc extends Array<Gifts[number]> = [],
    Cur extends Array<Gifts[number]> = [],
> = T extends [infer Q extends number, ...infer Rest extends number[]]
    ? Q extends Cur["length"]
        ? Rebuild<Rest, GiftLooper<I>, [...Acc, ...Cur]>
        : Rebuild<T, I, Acc, [...Cur, Gifts[I]]>
    : Acc;

// Тесты
type test_0_actual = Rebuild<[2, 1, 3, 3, 1, 1, 2]>;
type test_0_expected = [
    '🛹', '🛹',
    '🚲',
    '🛴', '🛴', '🛴',
    '🏄', '🏄', '🏄',
    '🛹',
    '🚲',
    '🛴', '🛴',
];
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = Rebuild<[3, 3, 2, 1, 2, 1, 2]>;
type test_1_expected = [
    '🛹', '🛹', '🛹',
    '🚲', '🚲', '🚲',
    '🛴', '🛴',
    '🏄',
    '🛹', '🛹',
    '🚲',
    '🛴', '🛴'
];
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type test_2_actual = Rebuild<[2, 3, 3, 5, 1, 1, 2]>;
type test_2_expected = [
    '🛹', '🛹',
    '🚲', '🚲', '🚲',
    '🛴', '🛴', '🛴',
    '🏄', '🏄', '🏄', '🏄', '🏄',
    '🛹',
    '🚲',
    '🛴', '🛴',
];
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;
```