# Олени планируют нападение

## 🇺🇸 Описание

> [❤️ Cupid] WHEN I"M DONE WITH HIM THERE WON"T BE NOTHIN" LEFT!!!
>
> [💨 Dasher] Bro, I want the same things you do but you gotta take a chill pill.
>
> [❤️ Cupid] I"M READY TO EXPLODE ON HIS CHIMNEY-CLIMBIN" ASS.
>
> [💃 Dancer] Look, ❤️ Cupid. Here's the thing. We are close. We have 💋 Crystal Right where we want her.
> We can use her to get what we want. And, after all, we'll be doing her a favor because she's also getting
> what she wants - to keep things about her and 🪩 Jamie quiet.
>
> [❤️ Cupid] OUR TERMS BETTER BE BULLETPROOF BECAUSE THAT
> COOKIE-STEALING MANIAC IS GONNA FIND A WAY TO WEASLE OUT OF IT.
>
> [💃 Dancer] Yes. Absolutely. Let's make sure no term can conflict with any other.
> Think of it like designing a GraphQL Schema - no room for innovation.

The reindeer are almost ready to make their demands. Help the reindeer make terms for
their demands in such a way that no term can be conflated with any other term.

## 🇷🇺 Описание

> [❤️ Купидон] КОГДА Я С НИМ ПОКОНЧУ, ОТ НЕГО НИЧЕГО НЕ ОСТАНЕТСЯ!!!
>
> [💨 Вихрь] Бро, я хочу того же, что и ты, но тебе надо немного остудить свой пыл.
>
> [❤️ Купидон] Я ГОТОВ ВЗОРВАТЬСЯ НА ЕГО ЗАДНИЦЕ, КАРАБКАЮЩЕЙСЯ ПО ТРУБАМ.
>
> [💃 Танцор] Послушай, ❤️ Купидон. Ситуация такова: мы близки к успеху. У нас есть 💋 Кристалл,
> ровно там, где нужно. Мы можем использовать её, чтобы получить желаемое. И в итоге мы ей даже поможем,
> потому что она тоже получит то, что хочет — сохранить всё в тайне про неё и 🪩 Джейми.
>
> [❤️ Купидон] НАШИ УСЛОВИЯ ДОЛЖНЫ БЫТЬ ПУЛЕНЕПРОБИВАЕМЫМИ, ПОТОМУ ЧТО ЭТОТ МАНЬЯК, ВОРУЮЩИЙ ПЕЧЕНЬЕ,
> НАЙДЕТ СПОСОБ ВЫКРУТИТЬСЯ.
>
> [💃 Танцор] Да. Без сомнений. Давайте убедимся, что ни одно условие не конфликтует с другими.
> Подумайте об этом как о создании схемы GraphQL — никаких лазеек.

Олени почти готовы выдвинуть свои требования. Помогите оленям составить термины для своих требований
таким образом, чтобы ни один термин не противоречил другому.

## 💻 Решение

Для решения задачи создадим инвариантный generic тип, чтобы он не мог принять никакой другой тип, кроме него самого.
Для этого воспользуемся модификаторами:

* `in` — делает generic контравариантным;
* `out` — делает generic ковариантным.

```typescript
import type { Equal, Expect } from 'type-testing';

// Решение
type Demand<in out T> = {
    demand: T;
}

// Тесты
declare let demand1: Demand<unknown>;
declare let demand2: Demand<string>;
declare let demand3: Demand<'Immediate 4% Pay Increase'>;
declare let demand4: Demand<'3 Days Paid Time Off Per Year'>;

type t1 = Expect<Equal<typeof demand1, { demand: unknown }>>
demand1 = demand1;
// @ts-expect-error
demand1 = demand2;
// @ts-expect-error
demand1 = demand3;
// @ts-expect-error
demand1 = demand4;

type t2 = Expect<Equal<typeof demand2, { demand: string }>>
// @ts-expect-error
demand2 = demand1;
demand2 = demand2;
// @ts-expect-error
demand2 = demand3;
// @ts-expect-error
demand2 = demand4;

type t3 = Expect<Equal<typeof demand3, { demand: 'Immediate 4% Pay Increase' }>>
// @ts-expect-error
demand3 = demand1;
// @ts-expect-error
demand3 = demand2;
demand3 = demand3;
// @ts-expect-error
demand3 = demand4;

type t4 = Expect<Equal<typeof demand4, { demand: '3 Days Paid Time Off Per Year' }>>
// @ts-expect-error
demand4 = demand1;
// @ts-expect-error
demand4 = demand2;
// @ts-expect-error
demand4 = demand3;
demand4 = demand4;
```