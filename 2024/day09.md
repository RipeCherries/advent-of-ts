# Вихрь против Джейми

## 🇺🇸 Описание

> [🪩 Jamie] You're lying. You didn't see anything because nothing was happening. I haven't done anything wrong!
>
> [🌟 Vixen] I literally saw you and 💋 Crystal with my own two eyes.
> Would you like me to describe the unicorn tattoo on your left cheek? Nice one, by the way.
>
> [🪩 Jamie, sweating] Damn it. You did see. What are you going to do?
>
> [🌟 Vixen] 🪩 Jamie Glitterglum. It's not what I am going to do to you but rather what you are going to do for me.
>
> [🪩 Jamie] Huh?
>
> [🌟 Vixen] We've been arguing with Santa about our pay.
> You seem like someone uniquely positioned to help get him to change his mind.
>
> [🪩 Jamie] You absolute jerk! You're blackmailing me!
>
> [🌟 Vixen] Look, we all have our own goals. Get 💋 Crystal to convince him to pay us what we're worth.
> We literally haven't had a pay raise since 2009. This shouldn't be so hard, but you know 🎅 Santa.
> He could care less about the rest of us.
>
> [🪩 Jamie] Well how am I supposed to do that?
>
> [🌟 Vixen] There's an `npm` I've been working on, `santas-special-list`.
> Honestly it’s a mess of untyped spaghetti code and patched dependencies, so good luck with that.
> Let's start there. It needs types. I was supposed to do it, but I need some cover so
> I can discuss all this with the other reindeer. You make the types for me so I can slip away for a few hours.
> I'll find you tomorrow and let you know what you need to do next.
>
> [🪩 Jamie] I hate you. I have always hated you. Now I hate you more.
>
> [🌟 Vixen] After what you're about to do for me, I think we're going to be great friends. 🖤

🪩 Jamie sure is in trouble. Help 🪩 Jamie do 🌟 Vixen's job by making types for the `santas-special-list` package.
Things are really heating up in the North Pole!

## 🇷🇺 Описание

> [🪩 Джейми] Ты лжешь. Ты ничего не видела, потому что ничего не происходило. Я ничего не сделал!
>
> [🌟 Вихрь] Я буквально видела тебя и 💋 Кристал своими собственными глазами.
> Хочешь, я опишу татуировку с единорогом на твоей левой щеке? Кстати, она классная.
>
> [🪩 Джейми, потеет] Чёрт. Ты действительно всё видела. Что ты собираешься делать?
>
> [🌟 Вихрь] 🪩 Джейми Блестяшка. Вопрос не в том, что я сделаю с тобой, а в том, что ты сделаешь для меня.
>
> [🪩 Джейми] Что?
>
> [🌟 Вихрь] Мы уже долго спорим с 🎅 Сантой насчет нашей зарплаты. Кажется, ты — тот самый,
> кто способен помочь ему изменить своё мнение.
>
> [🪩 Джейми] Ты абсолютный мерзавец! Ты меня шантажируешь!
>
> [🌟 Вихрь] Послушай, у всех свои цели. Уговори 💋 Кристал убедить его платить нам столько, сколько мы заслуживаем.
> Мы буквально не получали повышения с 2009 года. Это не должно быть так сложно, но ты знаешь 🎅 Санту.
> Ему совершенно плевать на остальных.
>
> [🪩 Джейми] Ну и как я должен это сделать?
>
> [🌟 Вихрь] Есть `npm-пакет`, над которым я работаю, `santas-special-list`. Честно говоря, это какой-то хаос
> из нетипизированного спагетти-кода и залатанных зависимостей, так что удачи тебе. Начнём с этого.
> Ему нужны типы. Я должна была этим заняться, но мне нужно время, чтобы обсудить всё это с остальными оленями.
> Ты сделаешь типизацию вместо меня, чтобы я могла исчезнуть на несколько часов.
> Завтра я тебя найду и скажу, что нужно делать дальше.
>
> [🪩 Джейми] Ненавижу тебя. Всегда ненавидел. Теперь ненавижу ещё больше.
>
> [🌟 Вихрь] После того, что ты сделаешь для меня, я думаю, мы станем отличными друзьями. 🖤

🪩 Джейми точно в беде. Помогите 🪩 Джейми сделать работу 🌟 Вихря, создав типы для пакета `santas-special-list`.
На Северном полюсе становится горячо!

## 💻 Решение

Для решения задачи декларируем модуль `'santas-special-list'`, добавив в него нужные типы.

```typescript
import type { Expect, Equal } from 'type-testing';
import type { Status, Child, List } from 'santas-special-list';

// Решение
declare module 'santas-special-list' {
    type Status = 'naughty' | 'nice';

    type Child = {
        name: string;
        status: Status;
    };

    type List = Child[];
}

// Тесты
type t0_actual = Status;
type t0_expected = 'naughty' | 'nice';
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = Child;
type t1_expected = {
    name: string,
    status: Status
};
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = List;
type t2_expected = Child[];
type t2 = Expect<Equal<t2_actual, t2_expected>>;
```