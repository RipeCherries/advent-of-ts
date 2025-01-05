# Магическое карри и управление разумом: Внедрение идеи

## 🇺🇸 Описание

> [The reindeer have made it clear: they will expose 💋 Crystal Claus for her 
> affair with 🪩 Jamie Glittergum to 🎅 Santa if 💋 Crystal can't convince 🎅 Santa how to give the reindeer a raise.]
>
> [🎅 Santa] Across the whole internet, all I see is adult children! WHAT'S IT GOING TO 
> TAKE TO GET PIPELINE OPERATORS INTO THIS FORSAKEN LANGUAGE!?!
>
> [💋 Crystal] Dear, stop reading the Node.JS release notes. 
> The TC-39 is gonna keep pipeline operators at stage 2 for the next 100 years at this point. Come. 
> Have some some dinner. I've made curry.
>
> [The two eat dinner together, but 🎅 Santa starts to feel a little strange..]
>
> [💋 Crystal] Are you feeling alright, Dear?
>
> [🎅 Santa] No, in fact.. I'm not. I'm feeling a bit fuzzy.
>
> [💋 Crystal] That's great to hear.
>
> [🎅 Santa, eyelids getting heavy] What?
>
> [💋 Crystal] I had a friend of mine, 🪩 Jamie Glitterglum, brew a magic memory implanting glitter potion. 
> This will all be over soon, Dear.
>
> [🎅 Santa, fading] How could you... do... that...
>
> [💋 Crystal] You'll fall asleep, and then I'm going to have a little talk with your subconscious mind. 
> Christopher Nolan can't hold a candle to what 🪩 Jamie can do.

Well that took a dark turn. You're an accessory to the crime, now. Congrats. 
Now you have no choice but to help 💋 Crystal with the coverup.

The `originalCurry` had to have some special ingredients added to it, but 💋 Crystal 
couldn't be sure if 🎅 Santa would see her cooking. So, to avoid raising suspicion, 
she had to find a way to be able to add the ingredients at different times.

## 🇷🇺 Описание

> [Олени ясно дали понять: они расскажут 🎅 Санте об интрижке 💋 Кристалл Клаус с 🪩 Джейми Глиттергамом, 
> если 💋 Кристалл не убедит 🎅 Санту повысить зарплату оленям.]
> 
> [🎅 Санта] Во всём интернете я вижу только взрослых детей! 
> ЧТО НУЖНО СДЕЛАТЬ, ЧТОБЫ ЗАСТАВИТЬ ОПЕРАТОРОВ ПАЙПЛАЙНОВ РАЗОБРАТЬСЯ С ЭТИМ ПРОКЛЯТЫМ ЯЗЫКОМ?!
> 
> [💋 Кристалл] Дорогой, перестань читать заметки к выпуску Node.js. 
> TC-39 оставит операторы пайплайнов на втором этапе ещё лет на сто. Давай. Ужин готов. Я приготовила карри.
> 
> [Они ужинают вместе, но 🎅 Санта начинает чувствовать себя странно...]
> 
> [💋 Кристалл] Ты в порядке, дорогой?
> 
> [🎅 Санта] Нет, вообще-то... не в порядке. Как будто в голове туман.
> 
> [💋 Кристалл] Это замечательно.
>
> [🎅 Санта, с тяжелеющими веками] Что?
> 
> [💋 Кристалл] Мой друг 🪩 Джейми Глиттергам приготовил волшебное зелье с памятью из блёсток. 
> Всё скоро закончится, дорогой.
> 
> [🎅 Санта, ослабевая] Как ты могла... это сделать...
> 
> [💋 Кристалл] Ты уснёшь, а потом я немного побеседую с твоим подсознанием. 
> Даже Кристофер Нолан не сравнится с тем, на что способен 🪩 Джейми.

Такой поворот! Теперь ты соучастник преступления. Поздравляю. 
У тебя нет выбора, кроме как помочь 💋 Кристалл скрыть всё это.

`originalCurry` пришлось немного доработать специальными ингредиентами, но 💋 Кристал не была уверена, 
что 🎅 Санта заметит её готовку. Поэтому, чтобы не вызывать подозрений, она должна была найти способ 
добавлять ингредиенты в разное время.

## 💻 Решение

Для решения задачи объявляем рекурсивный тип `DynamicParamsCurrying` который на каждом этапе частичного 
применения аргументов возвращает функцию, ожидающую оставшиеся аргументы. Если количество 
переданных аргументов `Args['length']` совпадает с количеством аргументов исходной функции `Parameters<T>['length']`, 
мы возвращаем результат вызова функции `ReturnType<T>`. Иначе рекурсия продолжается.

`DropFirst:` утилита для удаления первых элементов массива типов, чтобы обновить список оставшихся аргументов. 
Это позволяет корректно обрабатывать частичные вызовы с неполными аргументами.

```typescript
// Решение
type DropFirst<AllArgs extends any[], ProvidedArgs extends any[]> =
    ProvidedArgs['length'] extends 0
        ? AllArgs
        : AllArgs extends [infer First, ...infer Rest]
            ? ProvidedArgs extends [First, ...infer RestProvided]
                ? DropFirst<Rest, RestProvided>
                : never
            : never;

type DynamicParamsCurrying<Func extends (...args: any[]) => any> =
    <ProvidedArgs extends Partial<Parameters<Func>>>(...args: ProvidedArgs) =>
        ProvidedArgs['length'] extends Parameters<Func>['length']
            ? ReturnType<Func>
            : DynamicParamsCurrying<(...remainingArgs: DropFirst<Parameters<Func>, ProvidedArgs>) => ReturnType<Func>>

declare function DynamicParamsCurrying<Func extends (...args: any[]) => any>(func: Func): DynamicParamsCurrying<Func>;

// Тесты
const originalCurry = (
    ingredient1: number,
    ingredient2: string,
    ingredient3: boolean,
    ingredient4: Date,
) => true;

const spikedCurry = DynamicParamsCurrying(originalCurry);

// Direct call
const t0 = spikedCurry(0, "Ziltoid", true, new Date());

// Partially applied
const t1 = spikedCurry(1)("The", false, new Date());

// Another partial
const t2 = spikedCurry(0, "Omniscient", true)(new Date());

// You can keep callin' until the cows come home: it'll wait for the last argument
const t3 = spikedCurry()()()()(0, "Captain", true)()()()(new Date());

// currying is ok
const t4 = spikedCurry(0, "Spectacular", true);

// @ts-expect-error arguments provided in the wrong order
const e0 = spikedCurry("Nebulo9", 0, true)(new Date());
```