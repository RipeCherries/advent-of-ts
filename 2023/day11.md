# Защита списка

## 🇺🇸 Описание

*[a conversation overhead two elves, Wunorse and Alabaster, at Santa's workshop on a monday
morning after all the elves were forced to work all weekend]*

> [🧝 Wurnose] The world will not know peace until every consultant involved in pushing SOC2 is dead and in the ground.
>
> [🧝 Alabaster] Rough weekend?
>
> [🧝 Wurnose] Yes. And now I've just gotten word from the higher-ups that I have to make our
> entire codebase's types readonly. DEEP readonly.
>
> [🧝 Alabaster] Ouch. But at least it'll be better because something something functional programming superiority
> something something..
>
> [🧝 Wurnose] That's not the point. The point is that there IS NO POINT. They're saying this is a requirement of SOC2.
> This doesn't have the slightest thing to do with SOC2. It's literally security theater at this point.
> And we're the puppets.
>
> [🧝 Alabaster] Let's try to use this as an excuse to port the codebase to Rust where everything's immutable by default.
>
> [🧝 Wurnose] I already tried that. They said Rust is too new and untested. They're considering Go, though.
>
> [🧝 Alabaster] Lol dafuq? Rust is older than Go by like 3 years. Ok. Sorry I asked. Let's implement DeepReadonly then!

`SantaListProtector` takes in an arbitrary type as an input and modifies that type to be readonly.
The trick here is that it must also work for any nested objects.

## 🇷🇺 Описание

*[разговор двух эльфов, Вунорса и Алебастра, в мастерской Санты в понедельник утром,
после того как все эльфы были вынуждены работать все выходные]*

> [🧝 Вунорс] Мир не будет знать покоя, пока каждый консультант, участвовавший в продвижении SOC2,
> не будет мертв и лежать в земле.
>
> [🧝 Алабастер] Тяжёлые выходные?
>
> [🧝 Вунорс] Да. А теперь я только что узнал от начальства, что мне нужно сделать все типы в нашем коде
> только для чтения. Причём ГЛУБОКО только для чтения.
>
> [🧝 Алабастер] Больно, но хотя бы будет лучше, потому что что-то там про
> превосходство функционального программирования...
>
> [🧝 Вунорс] Не в этом суть. Суть в том, что её вовсе нет. Они говорят, что это требование SOC2.
> Это не имеет никакого отношения к SOC2. Это буквально "театр безопасности". А мы — марионетки.
>
> [🧝 Алабастер] Давай попробуем использовать это как повод переписать кодовую базу на Rust,
> где всё неизменно по умолчанию.
>
> [🧝 Вунорс] Я уже пытался. Они сказали, что Rust слишком новый и непроверенный. Но они рассматривают Go.
>
> [🧝 Алабастер] Лол, что? Rust старше Go на три года. Ладно, жаль, что спросил. Давай тогда реализуем DeepReadonly!

`SantaListProtector` принимает произвольный тип в качестве входного и модифицирует его, делая только для чтения.
Хитрость заключается в том, что это должно работать и для любых вложенных объектов.

## 💻 Решение

Для преобразование любого объекта или массива в версию с модификатором readonly используем условные типы и рекурсию.

```typescript
import { Expect, Equal } from 'type-testing';

// Решение
type SantaListProtector<T> = T extends Record<string, unknown> | Array<unknown>
    ? { readonly [K in keyof T]: SantaListProtector<T[K]> }
    : T;

// Тесты
type test_0_actual = SantaListProtector<{
    hacksore: () => 'naughty';
    trash: string;
    elliot: {
        penny: boolean;
        candace: {
            address: {
                street: {
                    name: 'candy cane way';
                    num: number;
                };
                k: 'hello';
            };
            children: [
                'harry',
                {
                    saying: ['hey'];
                },
            ];
        };
    };
}>;
type test_0_expected = {
    readonly hacksore: () => 'naughty';
    readonly trash: string;
    readonly elliot: {
        readonly penny: boolean;
        readonly candace: {
            readonly address: {
                readonly street: {
                    readonly name: 'candy cane way';
                    readonly num: number;
                };
                readonly k: 'hello';
            };
            readonly children: readonly [
                'harry',
                {
                    readonly saying: readonly ['hey'];
                },
            ];
        };
    };
};
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type test_1_actual = SantaListProtector<{
    theo: () => 'naughty';
    prime: string;
    netflix: {
        isChill: boolean;
    };
}>;
type test_1_expected = {
    readonly theo: () => 'naughty';
    readonly prime: string;
    readonly netflix: {
        readonly isChill: boolean;
    };
};
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;
```