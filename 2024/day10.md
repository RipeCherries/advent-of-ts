# Олени строят план

## 🇺🇸 Описание

> [🌟 Vixen tells the other reindeer of his plan to use Mrs. Claus' affair 
> with 🪩 Jamie as leverage to get the reindeer fair pay.]
>
> [🦌 Prancer] Holy crapy, 🌟 Vixen, this is going to change everything for us!
>
> [☄️ Comet] It ain't gonna change a damn thing if we don't get these priority flags sorted out!
>
> [🔴 Rudolph] Why is it that every time we have work to do you all come up with some 
> crazy plan like this instead of doing work??
>
> [🌟 Vixen] This one is huge, though! This is the motherload!!
>
> [🔴 Rudolph] We're gonna be in a motherload of pain if we can't finish writing this enum!
>
> [🌟 Vixen] Ok ok ok ok. Let's just get it done. What's the rules we got from the elves? 
> Something about bitwise operators? I swear, those little jerks just invent rules to mess with us.

Most TypeScript users and experts agree that you should avoid using TypeScript Enums. 
But there is one very specific thing they're really valuable for: flags!

The staff at the North Pole use bitwise enum flags to organize the different gift categories. 
Here are the rules to the logic:

* `Coal`, `Train`, `Bicycle`, `SuccessorToTheNintendoSwitch`, `TikTokPremium` `Vape` are all unique gifts;
* `Traditional` can be either `Train` or `Bycicle`;
* `OnTheMove` can be any of `Coal`, `Bicycle`, `TikTokPremium`, or `Vape`;
* `OnTheCouch` is like `OnTheMove` except instead of `Bicycle` it's got `SuccessorToTheNintendoSwitch`.

> **Note**: DO NOT simply copy the literal values for each flag from the tests. 
> Instead, think about the rules and create a system of bitwise enum flags that will satisfy the tests.

## 🇷🇺 Описание

> [🌟 Вихрь рассказывает другим оленям свой план использовать роман миссис Клаус с 🪩 Джейми как рычаг
> для получения справедливой зарплаты для оленей.]
>
> [🦌 Прыгун] Вот это да, 🌟 Вихрь, это все для нас изменит!
>
> [☄️ Комета] Это ничего не изменит, если мы не разберемся с этими приоритетными флагами!
>
> [🔴 Рудольф] Почему каждый раз, когда у нас есть работа, вы придумываете какую-то
> безумную идею вместо того, чтобы работать??
>
> [🌟 Вихрь] Но это нечто огромное! Это настоящий джекпот!!
>
> [🔴 Рудольф] Нас ждет настоящий джекпот боли, если мы не закончим писать этот перечисляемый тип!
>
> [🌟 Вихрь] Ладно, ладно, ладно, давайте просто сделаем это. Какие там правила от эльфов?
> Что-то про побитовые операторы? Клянусь, эти маленькие проныры придумывают правила только чтобы нас запутать.

Большинство пользователей и экспертов TypeScript согласны, что следует избегать использования
перечислений (Enums) в TypeScript. Но есть одна конкретная область, где они действительно полезны: флаги!

Сотрудники на Северном полюсе используют флаги на основе побитовых перечислений для организации
различных категорий подарков. Вот правила логики:

* `Coal`, `Train`, `Bicycle`, `SuccessorToTheNintendoSwitch`, `TikTokPremium`, и `Vape` — это все уникальные подарки;
* `Traditional` может быть либо `Train`, либо `Bicycle`;
* `OnTheMove` может включать любой из следующих подарков: `Coal`, `Bicycle`, `TikTokPremium` или `Vape`;
* `OnTheCouch` похоже на `OnTheMove`, за исключением того, что вместо `Bicycle` там
  используется `SuccessorToTheNintendoSwitch`.

> **Примечание**: НЕ копируйте буквально значения каждого флага из тестов. 
> Вместо этого подумайте над правилами и создайте систему побитовых флагов с использованием перечислений, 
> которая будет соответствовать тестам.

## 💻 Решение

Для решения задачи используем оператор побитового сдвига влево `<<`, 
чтобы создать 'базовые' подарки (`x << n = x * 2^n`). Остальные типы подарков опишем с помощью побитового
оператора ИЛИ `|`.

```typescript
// Решение
enum Gift {
  Coal = 0,
  Train = 1 << 0,
  Bicycle = 1 << 1,
  SuccessorToTheNintendoSwitch = 1 << 2,
  TikTokPremium = 1 << 3,
  Vape = 1 << 4,
  Traditional = Train | Bicycle,
  OnTheMove = Coal | Bicycle | TikTokPremium | Vape,
  OnTheCouch = Coal | SuccessorToTheNintendoSwitch | TikTokPremium | Vape,
};

// Тесты
const test = <F extends Gift>(flag: F) => flag;

test<Gift.Coal>(0);
test<Gift.Train>(1);
test<Gift.Bicycle>(2);
test<Gift.SuccessorToTheNintendoSwitch>(4);
test<Gift.TikTokPremium>(8);
test<Gift.Vape>(16);
test<Gift.Traditional>(3);
test<Gift.OnTheMove>(26);
test<Gift.OnTheCouch>(28);

// @ts-expect-error
test<Gift.Coal>(10);
// @ts-expect-error
test<Gift.Train>(11);
// @ts-expect-error
test<Gift.Bicycle>(12);
// @ts-expect-error
test<Gift.SuccessorToTheNintendoSwitch>(14);
// @ts-expect-error
test<Gift.TikTokPremium>(18);
// @ts-expect-error
test<Gift.Vape>(116);
// @ts-expect-error
test<Gift.Traditional>(13);
// @ts-expect-error
test<Gift.OnTheMove>(126);
// @ts-expect-error
test<Gift.OnTheCouch>(124);
```