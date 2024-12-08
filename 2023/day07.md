# Фильтрация детей (часть 2)

## 🇺🇸 Описание

*[transcript of a slack conversation at 11:23pm between Santa and Chipper
(one of the elves that worked on the filtering code from yesterday)]*

> [🎅🏻 Santa] We've got a big problem. That code that you gave me yesterday doesn't work!
>
> [🧝 Chipper] what doesn't work about it?
>
> [🎅🏻 Santa] It turns out I need the data formatted in a completely different way!
> The inputs and outputs all need to be different.
>
> [🧝 Chipper] ok, so it sounds like the requirements have changed. did you ask my manager?
> it's late and I'm relaxing. I'm in the middle of a game of League of Legends.
>
> [🎅🏻 Santa] Is that like RuneScape? Well anyway, would you mind helping me out in a pinch?
> Think of this as paying your dues for a better position later!
>
> [🧝 Chipper] ok. I guess.
>
> [🎅🏻 Santa] Wonderful! Here are the inputs and outputs! That oughta be plenty for you!
> Ok, I gotta get some rest for a golf game I'm having tomorrow. Signing off!

As you can see, sometimes leadership like Santa manage to convince themselves they have
fantastic product vision, you'll get little more than basic inputs and outputs, and you'll
have to figure out the behavior from there. Don't be flustered. Take a look at the tests
and try to figure out what the behavior is supposed to be.

Start by identifying the inputs for our `AppendGood` type. Ask yourself if there
should be any generic type constraints on the inputs (there may not need to be,
or at least right away).

Then try to set up a scaffold that will at least return the same values for each property.
Your next step is to transform the properties somehow..

## 🇷🇺 Описание

*[Переписка в Slack в 23:23 между Сантой и Чиппером (одним из эльфов, который работал
над кодом фильтрации вчера)]*

> [🎅🏻 Санта] У нас большая проблема. Тот код, который ты дал мне вчера, не работает!
>
> [🧝 Чиппер] Что именно не работает?
>
> [🎅🏻 Санта] Оказалось, что мне нужно, чтобы данные были отформатированы совершенно по-другому!
> Входные и выходные данные должны быть совершенно иными.
>
> [🧝 Чиппер] Окей, похоже, что требования изменились. Ты спросил моего менеджера? Сейчас поздно,
> и я отдыхаю. Я посреди игры в League of Legends.
>
> [🎅🏻 Санта] Это как RuneScape? Ну, в любом случае, не мог бы ты помочь мне прямо сейчас?
> Подумай об этом как о вкладе в твоё продвижение в будущем!
>
> [🧝 Чиппер] Ладно. Думаю, да.
>
> [🎅🏻 Санта] Замечательно! Вот входные и выходные данные! Этого должно быть достаточно.
> Окей, я пошёл отдыхать, завтра у меня игра в гольф. Отключаюсь!

Как видите, иногда руководству, подобно Санте, удается убедить себя в том, что у них
фантастическое видение продукта, и вы получите не более чем базовые входные и выходные данные.
Тебе предстоит самостоятельно понять, как должна работать логика.

Посмотрите на тесты и попытайтесь понять, каким должно быть поведение.

Начните с определения входных данных для типа `AppendGood`. Спросите себя, должны ли быть
какие-либо общие ограничения типа на входные данные (возможно, они не нужны, по крайней мере, сразу).

Затем настройте каркас, который будет хотя бы возвращать те же значения для каждого свойства.
На следующем этапе тебе нужно как-то трансформировать свойства.

## 💻 Решение

Используем `mapped types`, чтобы обрабатывать каждый ключ объекта, добавляя к нему префикс `good_`.
Используем `as` для переименования ключа и используем `K & string`,
чтобы явно указать, что `K` является строкой.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type AppendGood<T> = {
    [K in keyof T as `good_${K & string}`]: T[K];
};

// Тесты
type WellBehavedList = {
    tom: { address: '1 candy cane lane' };
    timmy: { address: '43 chocolate dr' };
    trash: { address: '637 starlight way' };
    candace: { address: '12 aurora' };
};
type test_wellBehaved_actual = AppendGood<WellBehavedList>;
type test_wellBehaved_expected = {
    good_tom: { address: '1 candy cane lane' };
    good_timmy: { address: '43 chocolate dr' };
    good_trash: { address: '637 starlight way' };
    good_candace: { address: '12 aurora' };
};
type test_wellBehaved = Expect<Equal<test_wellBehaved_expected, test_wellBehaved_actual>>;

type Unrelated = {
    dont: 'cheat';
    play: 'fair';
};
type test_Unrelated_actual = AppendGood<Unrelated>;
type test_Unrelated_expected = {
    good_dont: 'cheat';
    good_play: 'fair';
};
type test_Unrelated = Expect<Equal<test_Unrelated_expected, test_Unrelated_actual>>;
```