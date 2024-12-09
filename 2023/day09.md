# Санта страдает дислексией?

## 🇺🇸 Описание

*[it's early Saturday morning and the team has been working overtime.
Santa storms into the factory floor shouting..]*

> [🎅🏻 Santa] Don't you elves take any pride in your work?!?!
> Others would love to have your job for much less pay! I asked for a simple type that will
> reverse strings!! How hard is that?!? What do we even pay you for??

*[unfortunately, Santa is conveniently forgetting that the Reverse type
was cut from the sprint (which... of course... he agreed to)]*

> [🧝 floor manager] Ok. We never got acceptance criteria for that ticket.
>
> [🎅🏻 Santa] How difficult is it to understand what Reverse does!?
> `'rehsaD'` should be transformed into `'Dasher'`,
> `'recnaD'` should be transformed into `'Dancer'`,
> `'recnarP'` should be transformed into `'Prancer'`..
> DO I NEED TO KEEP GOING?
>
> [🧝 floor manager] Well you might be surprised. For example, what should happen
> to multi-codepoint unicode characters?
>
> [🎅🏻 Santa] What are you on about with all that accessibility stuff again!
>
> [🧝 floor manager] Accessibility is important, sir.
>
> [🎅🏻 Santa] Look, this is just an MVP. We can add accessibility later.
> Just get me my Reverse type! I'm having a hard time reading this stuff otherwise!

## 🇷🇺 Описание

*[Раннее субботнее утро, команда работает сверхурочно. Санта врывается в заводской цех с криками]*

> [🎅🏻 Санта] Неужели вы, эльфы, не гордитесь своей работой?!?!! Другие хотели бы получить вашу работу
> за гораздо меньшую плату! Я попросил простой тип, который будет разворачивать струны!!!
> Разве это так сложно?!? За что мы вообще вам платим???

*[К сожалению, Санта удобно забыл, что тип `Reverse` был вырезан из спринта
(на что он, конечно, согласился)]*

> [🧝 менеджер смены] Ладно. Мы так и не получили критериев приемки для этого тикета.
>
> [🎅🏻 Санта] Неужели так сложно понять, что делает Reverse?
> `'rehsaD'` должно быть преобразовано в `'Dasher'`, `'recnaD'` должно быть преобразовано в `'Dancer'`,
> `'recnarP'` должно быть преобразовано в `'Prancer'`... МНЕ ПРОДОЛЖАТЬ?
>
> [🧝 менеджер смены] Ну, вы можете удивиться. Например, что должно происходить
> с многокодовыми Unicode-символами?
>
> [🎅🏻 Санта] Что вы опять затеяли со всей этой ерундой про доступность!
>
> [🧝 менеджер смены] Доступность важна, сэр.
>
> [🎅🏻 Санта] Слушай, это всего лишь MVP. Мы добавим доступность позже.
> Просто сделайте мне этот тип Reverse! Мне тяжело читать это всё в нынешнем виде!

## 💻 Решение

Для решения задачи будем использовать рекурсию и шаблонные литералы.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type Reverse<T extends string, Acc extends string = ''> = T extends `${infer First}${infer Rest}`
    ? Reverse<Rest, `${First}${Acc}`>
    : Acc;

// Тесты
type test_0_actual = Reverse<'rehsaD'>;
type test_0_expected = 'Dasher';
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = Reverse<'recnaD'>;
type test_1_expected = 'Dancer';
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type test_2_actual = Reverse<'recnarP'>;
type test_2_expected = 'Prancer';
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;

type test_3_actual = Reverse<'nexiV'>;
type test_3_expected = 'Vixen';
type test_3 = Expect<Equal<test_3_expected, test_3_actual>>;

type test_4_actual = Reverse<'temoC'>;
type test_4_expected = 'Comet';
type test_4 = Expect<Equal<test_4_expected, test_4_actual>>;

type test_5_actual = Reverse<'dipuC'>;
type test_5_expected = 'Cupid';
type test_5 = Expect<Equal<test_5_expected, test_5_actual>>;

type test_6_actual = Reverse<'rennoD'>;
type test_6_expected = 'Donner';
type test_6 = Expect<Equal<test_6_expected, test_6_actual>>;

type test_7_actual = Reverse<'neztilB'>;
type test_7_expected = 'Blitzen';
type test_7 = Expect<Equal<test_7_expected, test_7_actual>>;

type test_8_actual = Reverse<'hploduR'>;
type test_8_expected = 'Rudolph';
type test_8 = Expect<Equal<test_8_expected, test_8_actual>>;
```