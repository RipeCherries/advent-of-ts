# Упаковщик подарков

## 🇺🇸 Описание

Did you know that there's also monetary inflation at the North Pole? You betcha, there is. 
And after 200+ years without a pay raise, Santa's elves are beginning to discuss a general strike.

December 3rd is just about the worst time imaginable for such a strike, and Santa's desperate 
to calm the elves down. If he can just wrap a few presents, maybe the elves will forget 
that they're being paid well below market rate (don't worry: the North Pole's actually 
still a Deleware-based startup so therefore it's ok).

There's a `GiftWrapper` type to help keep the wrapping process organized, but it needs something... 
it needs some way to be parameterized. What we have so far is nice as a generic starting point... 
but it needs some way to provide specific values for `Present`, `From`, and `To` at the type layer..

Please help! Otherwise the reindeer might catch wind of this and start a strike of their own 
in solidarity with the elves!

## 🇷🇺 Описание

Знаете ли вы, что на Северном полюсе тоже существует денежная инфляция? Еще бы. И после 200 с 
лишним лет без повышения зарплаты эльфы Санты начинают обсуждать возможность всеобщей забастовки.

3 декабря - самое неподходящее время для такой забастовки, и Санта отчаянно пытается 
успокоить эльфов. Если он сможет завернуть несколько подарков, возможно, эльфы забудут о том, 
что им платят намного меньше рыночной ставки (не волнуйтесь: Северный полюс - это все еще стартап, 
основанный компанией Deleware, поэтому все в порядке).

Есть тип `GiftWrapper`, который помогает организовать процесс упаковки, но ему нужно что-то... 
ему нужно как-то параметризироваться. То, что мы имеем на данный момент, хорошо подходит в качестве 
общей отправной точки... но ему нужен какой-то способ предоставить конкретные значения 
для `Present`, `From` и `To` на уровне типа...

Пожалуйста, помоги! Иначе о ситуации могут узнать олени и устроить 
свою собственную забастовку в поддержку эльфов!

## 💻 Решение

Необходимо создать обобщенный (generic) тип. Он принимает три параметра-типa и возвращает объект, где:

* ключ `present` соответствует типу `Present`.
* ключ `from` соответствует типу `From`.
* ключ `to` соответствует типу `To`.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type GiftWrapper<Present, From, To> = {
    present: Present;
    from: From;
    to: To;
}

// Тесты
type test_SantaToTrash_actual = GiftWrapper<'Car', 'Santa', 'Trash'>;
type test_SantaToTrash_expected = { present: 'Car', from: 'Santa', to: 'Trash' };
type test_SantaToTrash = Expect<Equal<test_SantaToTrash_actual, test_SantaToTrash_expected>>;

type test_TrashToPrime_actual = GiftWrapper<'vscode', 'Trash', 'Prime'>;
type test_TrashToPrime_expected = { present: 'vscode', from: 'Trash', to: 'Prime' };
type test_TrashToPrime = Expect<Equal<test_TrashToPrime_actual, test_TrashToPrime_expected>>;

type test_DanToEvan_actual = GiftWrapper<'javascript', 'Dan', 'Evan'>;
type test_DanToEvan_expected = { present: 'javascript', from: 'Dan', to: 'Evan' };
type test_DanToEvan = Expect<Equal<test_DanToEvan_actual, test_DanToEvan_expected>>;
```