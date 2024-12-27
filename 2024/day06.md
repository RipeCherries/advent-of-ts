# Санта отказывается использовать ветки обсуждений

## 🇺🇸 Описание

> [Conversation in the `#bugs` company Slack channel.]
>
> [🎅 Santa] How do you hoofed halfwits manage to screw up something a toddler with a crayon could figure out?
> I asked for the simplest thing I could imagine and you still couldn't do it!
> If you had two brain cells to rub together you'd understand that only numbers or strings are valid route identifiers!
>
> [🌩️ Donner] Please make a thead in this Slack channel, otherwise it gets too noisy and hard to follow.
> Also our Linear thing just made 3 tickets for each of your messages.
>
> [🎅 Santa] I am old school, I prefer IRC-style chatter.
> I do not value these "thread" things and find them difficult to use. I'll never do it, myself.
>
> [🌟 Vixen, in a thread] This isn’t 1997, Santa. Nobody cares about your IRC war stories.
> This is a company support channel with integrations designed to run the company better.
> How are you seriously against that??
>
> [🎅 Santa] I'm not against that, I'm just against using threads in chat apps.

Clearly we need to constrain our generic type a bit to only allow specific data types.
🎅 Santa will probably also complain about that, but so it goes.

## 🇷🇺 Описание

> [Разговор в `#bugs` Slack-канале компании].
>
> [🎅 Санта] Как вы, копытные недоумки, умудряетесь облажаться в том, с чем даже ребенок с карандашом бы справился?
> Я попросил самое простое, что только мог представить, а вы все равно не смогли это сделать!
> Если бы у вас было хотя бы две работающие извилины, вы бы поняли, что в качестве идентификаторов маршрутов
> допустимы только числа или строки!
>
> [🌩️ Доннер] Пожалуйста, создавайте ветки обсуждений в этом канале Slack, иначе здесь становится слишком шумно,
> и сложно что-либо понять. К тому же, наша система Linear только что создала три тикета на каждое ваше сообщение.
>
> [🎅 Санта] Я старой закалки, предпочитаю общение в стиле IRC.
> Эти ваши "ветки обсуждений" мне не нравятся, и я нахожу их неудобными. Никогда не буду их использовать.
>
> [🌟 Виксен, в ветке обсуждения] На дворе не 1997-й год, Санта. Никого не волнуют твои байки про IRC.
> Это канал поддержки компании с интеграциями, которые помогают нам лучше управлять процессами.
> Ты серьезно против этого?
>
> [🎅 Санта] Я не против этого, я просто против использования веток в чат-приложениях.

Очевидно, нам нужно ограничить наш универсальный тип, чтобы разрешить только определенные типы данных.
🎅 Санта, вероятно, будет жаловаться и на это, но что поделаешь.

## 💻 Решение

Для решения задачи ограничим `Route` с помощью `extends string | number`.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
const createRoute = <Route extends string | number>(author: string, route: Route) => {
    console.log(`[createRoute] route created by ${author} at ${Date.now()}`);
    return route;
}

// Тесты
const oneMill = createRoute('🌩️Donner', 100_000_000);
type t0_actual = typeof oneMill;
type t0_expected = 100_000_000;
type t0 = Expect<Equal<t0_actual, t0_expected>>;

const two = createRoute('🔴Rudolph', 2);
type t1_actual = typeof two;
type t1_expected = 2;
type t1 = Expect<Equal<t1_actual, t1_expected>>;

const three = createRoute('💨Dasher', '3');
type t2_actual = typeof three;
type t2_expected = '3';
type t2 = Expect<Equal<t2_actual, t2_expected>>;

// @ts-expect-error
createRoute('🌟Vixen', true);

// @ts-expect-error
createRoute('💃Dancer', [1]);

// @ts-expect-error
createRoute('☄️Comet', {1: 1});
```