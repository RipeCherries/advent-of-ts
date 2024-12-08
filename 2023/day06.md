# Фильтрация детей (часть 1)

## 🇺🇸 Описание

As you may be aware, kids that are naughty get a lump of coal and kids that are nice get a toy.
Santa's sorta controlling, honestly, and he likes being able to manipulate the data by filtering
out the naughty kids on some days, and filtering out the nice kids on other days.

So, Santa walks over to the (open floorplan) office where the engineering team sits.
Although he's just addressing the engineers, the rest of the office is distracted because
they can clearly hear him since there are no walls.

> [🎅🏻 Santa] You know, this job is a great opportunity for you elves, even without high pay!
> You're gaining valuable experience, which is more important than money!
>
> [🧝 elves] grumble grumble ok. cool. do you need something?
>
> [🎅🏻 Santa] YES! So glad you asked! I'd like an idea of how many kids have been nice.
> I want to be able to filter out all the naughty kids sometimes, and filter out the nice
> kids other times.
>
> [🧝 elf manager] ok. that's fine. we'll add a ticket for the next sprint..
>
> [🎅🏻 Santa] Oh! No time for story points and JIRA tickets, I'm afraid! I need this done pronto!
>
> [🧝 elf manager] we use Linear, but ok sure. we'll drop everything we're doing.

Can you help?
You're an engineer too. Can you help the elf team?

The first parameter of FilterChildrenBy is just a union of all the children's naughty/nice status. The second parameter
is the thing we want to exclude from the final resulting type.

Take a look at the tests. There, you'll see some examples.

> **Note**: the engineering team manager triple checked with Santa because this seems like
> a strange way to keep track of naughty and nice children, but Santa ensured the manager
> that this is exactly what he wants.

## 🇷🇺 Описание

Как Вы, наверное, знаете, непослушные дети получают кусок угля, а хорошие - игрушку.
Санта, честно говоря, контролирует ситуацию, и ему нравится иметь возможность манипулировать данными,
отсеивая непослушных детей в одни дни и отсеивая хороших детей в другие дни.

Итак, Санта подходит к офису (с открытой планировкой), где сидит команда инженеров.
Хотя он обращается только к инженерам, остальные члены офиса отвлекаются, потому что могут
отчетливо слышать его, так как здесь нет стен.

> [🎅🏻 Санта] Знаете, эта работа - отличная возможность для вас, эльфов, даже без высокой зарплаты!
> Вы получаете ценный опыт, который важнее денег!
>
> [🧝 Эльфы] *бурчат* Ага, круто. Вам что-то нужно?
>
> [🎅🏻 Санта] ДА! Так рад, что вы спросили! Я хотел бы знать, сколько детей вели себя хорошо.
> Я хочу иметь возможность иногда отсеивать всех непослушных детей, а в другое время - хороших.
>
> [🧝 Менеджер эльфов] Хорошо. Это прекрасно. Мы добавим тикет на следующий спринт.
>
> [🎅🏻 Санта] О! У нас нет времени на story points и задачи в JIRA! Это нужно сделать прямо сейчас!
>
> [🧝 Менеджер эльфов] Мы используем Linear, но, конечно, хорошо. Мы бросим все, что делаем.

Вы можете помочь?
Вы тоже инженер. Вы можете помочь команде эльфов?

Первый параметр `FilterChildrenBy` - это просто объединение всех статусов детей «непослушный/хороший».
Второй параметр - это то, что мы хотим исключить из конечного результирующего типа.

Взгляните на тесты. Там вы увидите несколько примеров.

> **Примечание**: менеджер инженерной группы трижды переспросил Санту, потому что это кажется
> странным способом отслеживать непослушных и хороших детей, но Санта заверил менеджера,
> что это именно то, что ему нужно.

## 💻 Решение

Для решения задачи используем утилитный тип TypeScript `Exclude`. Он принимает два параметра:

* унион значений;
* тип, который нужно исключить из униона.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type FilterChildrenBy<U, Excluded> = Exclude<U, Excluded>;

// Тесты
type test_0_actual = FilterChildrenBy<'nice' | 'nice' | 'nice', 'naughty'>;
type test_0_expected = 'nice';
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = FilterChildrenBy<'nice' | 'naughty' | 'naughty', 'naughty'>;
type test_1_expected = 'nice';
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;

type test_2_actual = FilterChildrenBy<string | number | (() => void), Function>;
type test_2_expected = string | number;
type test_2 = Expect<Equal<test_2_expected, test_2_actual>>;
```