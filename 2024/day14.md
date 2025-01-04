# Санта прячется за обзором производительности

## 🇺🇸 Описание

> [🎅 Santa] Sadly, she's no longer a 10.
>
> [🎩 Bernard] Are we talking about the same thing??
>
> [🎅 Santa] Huh? Oh. Right, you were talking about the reindeer.
>
> [🎩 Bernard] Yeah, what were you talking about?
>
> [🎅 Santa] Nothing, nothing. So just tell them they'll get their raise when
> they can show some dedication to this business.
>
> [🎩 Bernard] The thing is they feel like they've more than accomplished that over the last 15 years
> since they last got a pay increase.
>
> [🎅 Santa] Tell them we'll review it during the next performance review.
>
> [🎩 Bernard, facepalming] And when is that?
>
> [🎅 Santa] When their performance improves! You think I just give raises to anyone who pulls a sleigh?
> This isn’t charity.

We need a type for a function that represents the performance review cycle. With this function in hand,
perhaps 🎩 Bernard will be able to convince 🎅 Santa to take the reindeer's demands more seriously.

## 🇷🇺 Описание

> [🎅 Санта] К сожалению, она больше не "десятка".
>
> [🎩 Бернард] Мы точно об одном и том же говорим??
>
> [🎅 Санта] Что? Ой. Да, ты же говорил о северных оленях.
>
> [🎩 Бернард] А ты о чем говорил?
>
> [🎅 Санта] Ни о чем, ни о чем. Скажи им, что они получат повышение, когда смогут проявить преданность этому делу.
>
> [🎩 Бернард] Проблема в том, что они считают, что уже доказали это за последние 15 лет с тех пор,
> как в последний раз получили повышение зарплаты.
>
> [🎅 Санта] Скажи им, что мы рассмотрим это на следующем обзоре производительности.
>
> [🎩 Бернард, хлопая себя по лбу] А когда это будет?
>
> [🎅 Санта] Когда их производительность улучшится! Ты думаешь, я просто так даю повышение каждому,
> кто везет сани? Это тебе не благотворительность.

Нам нужен тип для функции, представляющей цикл обзора производительности.
Имея такую функцию, возможно, 🎩 Бернард сможет убедить 🎅 Санту отнестись к требованиям северных оленей более серьезно.

## 💻 Решение

Для решения задачи нужно создать тип `PerfReview`, который извлекает объединение всех возможных значений,
возвращаемых с помощью `yield` в асинхронном генераторе.

Функции `numberAsyncGenerator` и `stringAsyncGenerator` возвращают объекты типа `AsyncGenerator`,
которые имеют структуру `AsyncGenerator<Y, R, N>`, где:

* `Y` — тип значений, возвращаемых через `yield`;
* `R` — тип значения, возвращаемого с помощью `return`;
* `N` — тип значений, принимаемых через `next`.

Нас интересует только `Y` — тип значений, которые выдаёт генератор.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
type PerfReview<T> = T extends AsyncGenerator<infer Params, any, any> ? Params : never;

// Тесты
async function* numberAsyncGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

type t0_type = typeof numberAsyncGenerator;
type t0_actual = PerfReview<ReturnType<t0_type>>;
type t0_expected = 1 | 2 | 3;
type t0 = Expect<Equal<t0_actual, t0_expected>>;

async function* stringAsyncGenerator() {
    yield '1%';
    yield '2%';
}

type t1_type = typeof stringAsyncGenerator;
type t1_actual = PerfReview<ReturnType<t1_type>>;
type t1_expected = "1%" | "2%";
type t1 = Expect<Equal<t1_actual, t1_expected>>;
```