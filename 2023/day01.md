# Рождественское печенье

## 🇺🇸 Описание

It's December 1st! That means it's almost time for the big day!
Santa has a preparation regimen that involves, of course,
eating lots of delicious cookies.

Santa's elves have provided Santa an API whereby Santa can submit
his favorite cookie flavors. This year his favorites are:

* `ginger-bread`
* `chocolate-chip`

But the elves have some kind of fancy code-gen build step
(built in Rust, of course), so all Santa needs to do is update
the `SantasFavoriteCookies` type to accept either of his favorite cookies.

Can you help?

## 🇷🇺 Описание

Сегодня 1 декабря! А это значит, что знаменательный день почти настал!
У Санты есть особый план подготовки, который, конечно же, включает
поедание множества вкусного печенья.

Эльфы Санты предоставили ему API, с помощью которого он может отправлять
свои любимые вкусы печенья. В этом году его фавориты:

* `ginger-bread` (имбирное печенье);
* `chocolate-chip` (печенье с шоколадной крошкой).

Но у эльфов есть какой-то хитрый процесс генерации кода
(конечно, написанный на Rust), поэтому всё, что нужно сделать Санте,
это обновить тип `SantasFavoriteCookies`, чтобы он принимал любой
из его любимых вкусов печенья.

Ты можешь помочь?

## 💻 Решение

Для решения задачи определяем тип `SantasFavoriteCookies` как объединение строковых литералов 
`'ginger-bread' | 'chocolate-chip'`.

```typescript
import {Expect, Equal} from 'type-testing';

// Решение
type SantasFavoriteCookies = 'ginger-bread' | 'chocolate-chip';

// Тесты
type test_0_actual = SantasFavoriteCookies;
type test_0_expected = 'ginger-bread' | 'chocolate-chip';

type test_0 = Expect<Equal<test_0_actual, test_0_expected>>;
```