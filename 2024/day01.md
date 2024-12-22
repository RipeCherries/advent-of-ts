# Олени больше не выдерживают

## 🇺🇸 Описание

> [🎅 Santa's reindeer, ☄️ Comet and 💨 Dasher, have a secret meeting with 🎩 Bernard, the head of the elves]
>
> [☄️ Comet] It's the economy, stupid!
>
> [💨 Dasher] ☄️ Comet! I'm sick of your shit. Read a book or something, wouldja??
> Our productivity far outpaced North Pole's inflation like two decades ago.
> Our wage hasn't changed since 2009 and if you take inflation into account we make half of what we made then!
> We could have the best economy of the last 500 years and it still wouldn't change anything.
>
> [🎩 Bernard] Boys, boys! calm down. We'll fix this. We can fix this.
>
> [☄️ Comet] Oh, I’m sorry, Mr. Bernard the Keynesian scholar. Maybe you can explain why we’re still getting paid
> in candy canes like it’s 1947. Apparently we're dealing with reindeer here [glares at 💨 Dasher]
> that don't even know the difference between `any` and `unknown`.
>
> [🎩 Bernard] Boys, boys, calm the hell down! You’re not the only ones with problems.
> I’ve got 600 elves in the workshop huffing reindeer wax and unionizing over bathroom breaks.
> You just need to come up with a number for our `Demand`.
>
> [💨 Dasher] What kind of number?
>
> [🎩 Bernard] At this point, any number will do. But we need to start somewhere.
>
> [💨 Dasher] Why not go all in? Write down ‘infinity carrots’ and tell him it’s non-negotiable.
> What’s he gonna do, hire reindeer scabs?
>
> [🎩 Bernard] Do you know how hard it is to get Santa to focus these days? Half the time, he’s passed out in
> his workshop muttering about crypto and Mrs. Claus’s book club drama. ANY number will be fine as a starting point.

What you just read above is part of the story for this year's challenge. This year picks up from last year's story.
If it interests you, you can quickly read all of last year's stories on the AoT 2023 site.
In these stories, we get to know Santa in a much more... personal way than what you might be used to from picture
books and cartoons.

The story will often give you a lot of context about the changes you need to make to the TypeScript code.
In today's example, we see that we're supposed to modify `Demand` (the TypeScript type) to be a `number`.

## 🇷🇺 Описание

> [🎅 Санта-Клаусовские олени ☄️ Комета и 💨 Вихрь устраивают тайную встречу с 🎩 Бернардом, главным эльфом.]
>
> [☄️ Комета]: Это всё экономика, тупица!
>
> [💨 Вихрь]: ☄️ Комета! Я сыт по горло твоей чушью. Почитай книгу или что-нибудь, а?
> Наша производительность обогнала инфляцию Северного полюса ещё два десятилетия назад.
> Наша зарплата не менялась с 2009 года, и если учитывать инфляцию, мы зарабатываем вдвое меньше, чем тогда!
> У нас могла бы быть лучшая экономика за последние 500 лет, и всё равно ничего бы не изменилось.
>
> [🎩 Бернард]: Парни, парни! Успокойтесь. Мы это исправим. Мы сможем всё исправить.
>
> [☄️ Комета]: О, простите, мистер Бернард, кейнсианский учёный. Может, вы объясните, почему нас до сих пор платят
> леденцами, как в 1947 году? Похоже, здесь у нас олени [бросает взгляд на 💨 Вихря],
> которые даже не знают разницы между `any` и `unknown`.
>
> [🎩 Бернард]: Парни, парни, остыньте! У вас не одних проблемы.
> У меня 600 эльфов в мастерской нюхают олений воск и создают профсоюз из-за перерывов на туалет.
> Вам просто нужно назвать число для нашего требования.
>
> [💨 Вихрь]: Какое ещё число?
>
> [🎩 Бернард]: На данном этапе подойдёт любое число. Но нам нужно с чего-то начать.
>
> [💨 Вихрь]: Почему бы не взять по максимуму? Напишите «бесконечность морковок» и скажите, что это не подлежит
> обсуждению. Что он сделает, наймёт оленьих штрейкбрехеров?
>
> [🎩 Бернард]: Вы хоть представляете, как сложно заставить Санту сосредоточиться в наши дни?
> Половину времени он валяется в своей мастерской, бормоча что-то о криптовалюте и драмах книжного клуба миссис Клаус.
> ЛЮБОЕ число подойдёт, чтобы начать переговоры.

То, что вы только что прочитали, — это часть истории для челленджа этого года.
События развиваются после истории прошлого года. Если вам интересно, вы можете быстро ознакомиться со всеми
историями прошлого года на сайте AoT 2023. В этих рассказах мы узнаём Санту гораздо...
более личным, чем в книжках с картинками и мультфильмах.

История часто даёт много контекста о том, какие изменения нужно внести в код TypeScript.
Например, сегодня мы видим, что нужно изменить тип `Demand` (требование) на `number`.

Правда в том, что читать истории необязательно. Если хотите, можете сразу перейти к тестам, чтобы понять,
как должен работать ваш тип. Мы обычно даём вам стартовый код, но он всегда будет неполным.
Посмотрите тесты и измените код так, чтобы они проходили.

## 💻 Решение

Для решения задачи определяем тип `Demand` как примитив `number`.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
type Demand = number;

// Тесты
type t0_actual = Demand;
type t0_expected = number;
type t0 = Expect<Equal<t0_actual, t0_expected>>;
```