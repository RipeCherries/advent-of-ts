# Тестер окончания названий улиц на Рождество

## 🇺🇸 Описание

It's a little known fact that Santa's reindeer are orienteering experts.
They're very particular, actually.

To do this work well, they need to do some basic validation on the addresses.
There were hopes among some reindeer to introduce a validation library this year,
but there was simply too much infighting. It's kindof a mess. You see..

* Comet and Vixen want to use Zod because they heard from a YouTuber
  they like that it's the best (they haven't actually looked ito anything else);
* Cupid and Rudolph are simply too used to JSON Schema with AJV.
  They don't want to learn a new thing. They both had popular webpack plugins in the past
  that are no longer used by anyone and now they're a little bitter about change (in general);
* Then you have Prancer. Prancer doesn't see the point in validation.
  Prance feels that validating inputs pollutes the code with type gymnastics that add
  ever so little joy to the development experience. Yep. Even the reindeer have one of these
  on their team in 2023;
* Meanwhile, Blitzen is pushing hard for typia because it's so fast (naturally);
* Dancer and Donner don't seem to ever be able to articulate their opinions and usually
  just follow the rest of the group.

So, for this year.. nothing fancy. We'll have to just write a `StreetSuffixTester` from scratch.

This type will take two generic arguments. The first is for the street, and the second is
for the suffix we're testing against.

If the street ends with the suffix then the type should return `true` (otherwise, `false)`.

## 🇷🇺 Описание

Мало кто знает, но олени Санты — настоящие эксперты по ориентированию.
На самом деле, они очень точны.

Чтобы хорошо выполнять эту работу, им необходимо проводить базовую проверку адресов.
Некоторые олени надеялись внедрить библиотеку проверки в этом году, но в результате возникло
слишком много разногласий. Получилась какая-то неразбериха. Давай разберёмся:

* Комета и Виксен хотят использовать Zod, потому что какой-то YouTuber, которого они любят,
  сказал, что это лучшая библиотека (на самом деле, они больше ничего не изучали);
* Купидон и Рудольф привыкли к JSON Schema с AJV. Они не хотят учить что-то новое.
  Раньше у них были популярные плагины для webpack, которые больше никто не использует,
  и теперь они немного горько относятся к переменам (в целом);
* Прэнсер не видит смысла в валидации. Прэнсер считает, что валидация загрязняет код
  "типовыми гимнастиками", которые почти не приносят радости в процесс разработки. Да, даже у
  оленей в 2023 году есть такой в команде;
* Блитцен активно продвигает typia, потому что она очень быстрая (естественно);
* Танцор и Доннер не могут чётко выразить своё мнение и обычно просто следуют за остальными.

Итак, в этом году... ничего сложного. Нам придётся написать `StreetSuffixTester` с нуля.

Этот тип должен принимать два обобщённых аргумента: первый для названия улицы,
второй — для суффикса, который мы проверяем.

Если название улицы заканчивается на указанный суффикс, тип должен возвращать `true`
(в противном случае — `false`).

## 💻 Решение

Для проверки конца строки в TypeScript используем шаблонные литералы и условные типы.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type StreetSuffixTester<
    Street extends string,
    Suffix extends string
> = Street extends `${infer _Start}${Suffix}` ? true : false;

// Тесты
type test_0_actual = StreetSuffixTester<'Candy Cane Way', 'Way'>;
type test_0_expected = true;
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = StreetSuffixTester<'Chocalate Drive', 'Drive'>;
type test_1_expected = true;
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type test_2_actual = StreetSuffixTester<'Sugar Lane', 'Drive'>;
type test_2_expected = false;
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;

type test_3_actual = StreetSuffixTester<'Fifth Dimensional Nebulo 9', 'invalid'>;
type test_3_expected = false;
type test_3 = Expect<Equal<test_3_expected, test_3_actual>>;
```