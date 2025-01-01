# Отговорки, отговорки

## 🇺🇸 Описание

> [Last night, while the reindeer plotted on how to leverage 💋 Crystal Claus with their knowledge
> of her affair with 🪩 Jamie Glitterglum, 💋 Crystal and 🪩 Jamie met up in a panic.
> Today, 🎅 Santa is beginning to suspect something]
>
> [🎅 Santa] Dear.. Where were you last night?
>
> [💋 Crystal, turning bright red in embarrassment] Oh. Uh, I was out doing karaoke with some friends.
> Kendrick Lamar and Opeth both released albums on the same day last month,
> so we've been learning all the songs to both. [coughing nervously] Oh [cough] and also we were trying
> out a new drink one of the elves came up with. It's a bit like a mojito but instead of mint it's..
> well it's a whole thing. Then we also
>
> [🎅 Santa] You seem nervous.
>
> [💋 Crystal, sweating] Oh, no, not at all! Not at all! It's just so hard to remember, you can understand!
>
> [Later that night 💋 Crystal and 🪩 Jamie met again.]
>
> [💋 Crystal] We're in BIG trouble. What are we gonna do!!? We need a new excuse,
> and we need to add something to it. But... what??
>
> [🪩 Jamie] Talk to him again, but this time be ready to add to your excuse! Just bury him in details.
> 🎅 Santa tunes out if you say anything longer than two sentences about non-cookie-related topics.
> Think of a few different excuses, just in case.

We already have a `Excuse` that 💋 Crystal provided 🎅 Santa, but now we need to extend it in some way.
We need it to be general-purpose, so 💋 Crystal can try a few different ideas out.

## 🇷🇺 Описание

> [Прошлой ночью, пока северные олени строили планы, как шантажировать 💋 Кристалл Клаус
> её романом с 🪩 Джейми Глиттерглумом, 💋 Кристалл и 🪩 Джейми в панике встретились.
> Сегодня 🎅 Санта начинает что-то подозревать.]
>
> [🎅 Санта] Дорогая... Где ты была прошлой ночью?
>
> [💋 Кристалл, краснея от смущения] Ох. Эм, я была на караоке с друзьями. В прошлом месяце Кендрик Ламар
> и Opeth выпустили альбомы в один день, и мы все учили песни с обоих альбомов. [нервно кашляет] Ах, да [кашель],
> мы ещё пробовали новый напиток, который придумал один из эльфов. Это что-то вроде мохито, но вместо мяты там...
> ну, это целая история. А потом мы ещё...
>
> [🎅 Санта] Ты выглядишь нервной.
>
> [💋 Кристалл, покрываясь потом] О, нет, совсем нет! Совсем нет! Просто так трудно всё вспомнить, ты же понимаешь!
>
> [Позже той же ночью 💋 Кристалл и 🪩 Джейми снова встретились.]
>
> [💋 Кристалл] У нас ОГРОМНЫЕ неприятности. Что же нам делать!!?
> Нам нужна новая отговорка, и её нужно чем-то дополнить. Но чем??
>
> [🪩 Джейми] Поговори с ним снова, но на этот раз будь готова дополнить свою отговорку!
> Просто завали его деталями. 🎅 Санта отключается, если ты говоришь больше двух предложений о чём-то,
> что не связано с печеньем. Придумай несколько вариантов отговорок, на всякий случай.

У нас уже есть `Excuse` (отговорка), которую 💋 Кристалл дала 🎅 Санте, но теперь её нужно как-то расширить.
Она должна быть универсальной, чтобы 💋 Кристалл могла попробовать разные идеи.

## 💻 Решение

На вход типа `Excuse` принимает generic `T`, ограниченный как `Record<string, string>`.
Далее, мы типизируем метод `new`. Конструктор принимает на вход объект `input` с типом `T`, и возвращает строку:
`{keyof T & string}: ${T[keyof T]}`. Первая часть сужает ключи `T` до строковых.
Вторая составляет строку на основе всех существующих значений переданного объекта.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
type Excuse<T extends Record<string, string>> = {
    new(input: T): `${keyof T & string}: ${T[keyof T]}`;
};

// Тесты
const existingExcuses = {
    karaoke: ['Kendrick Lamar, Opeth'],
    margarita: 'Peppermint-Jalapeño Mojito',
}

const helpingTheReindeer = {
    helping: 'the reindeer',
} as const;
declare const Excuse0: Excuse<typeof helpingTheReindeer>;
const excuse0 = new Excuse0({
    ...existingExcuses,
    ...helpingTheReindeer,
});
type t0_actual = typeof excuse0;
type t0_expected = "helping: the reindeer";
type t0 = Expect<Equal<t0_actual, t0_expected>>;

const eatingFudge = {
    eating: 'fudge',
} as const;
declare const Excuse1: Excuse<typeof eatingFudge>;
const excuse1 = new Excuse1({
    ...existingExcuses,
    ...eatingFudge,
});
type t1_actual = typeof excuse1;
type t1_expected = "eating: fudge";
type t1 = Expect<Equal<t1_actual, t1_expected>>;
```