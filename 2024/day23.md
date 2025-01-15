# Апгрейд конвейерной линии

## 🇺🇸 Описание

The North Pole's toy assembly line is getting a major upgrade! To streamline production, the elves need to build
a type-safe conveyor belt system that can standardize their operations and make them reusable.
The system should introduce an `Apply` type that can be used to apply operations to values.

The system should support the following operations:

* `Push`: Add an item to a tuple;
* `Extends`: Check if a type extends another type;
* `Filter`: Filter items in a tuple based on any criteria;
* `ApplyAll`: Apply an operation to all items in a tuple;
* `Cap`: Capitalize the first letter of a string.

Example usage of the conveyor belt system:

```typescript
type Station1 = Apply<Cap, "robot">; // "Robot"
type Station2 = Apply<Apply<Push, Station1>, ["Tablet", "teddy bear"]>; // ["Tablet", "teddy bear", "Robot"]
type Station3 = Apply<
    Apply<Filter, Apply<Extends, Apply<Cap, string>>>,
    Station2
>; // ["Tablet", "Robot"]
```

## 🇷🇺 Описание

Конвейерная линия по производству игрушек на Северном полюсе проходит серьезный апгрейд!
Чтобы оптимизировать производство, эльфам нужно создать типобезопасную систему конвейерной ленты,
которая сможет стандартизировать их операции и сделать их переиспользуемыми. Система должна внедрить тип `Apply`,
который позволит применять операции к значениям.

Система должна поддерживать следующие операции:

* `Push`: Добавить элемент в кортеж;
* `Extends`: Проверить, является ли один тип производным от другого;
* `Filter`: Отфильтровать элементы кортежа на основе заданного критерия;
* `ApplyAll`: Применить операцию ко всем элементам кортежа;
* `Cap`: Преобразовать первую букву строки в заглавную.

Пример использования системы конвейерной ленты:

```typescript
type Station1 = Apply<Cap, "robot">; // "Robot"
type Station2 = Apply<Apply<Push, Station1>, ["Tablet", "teddy bear"]>; // ["Tablet", "teddy bear", "Robot"]
type Station3 = Apply<
    Apply<Filter, Apply<Extends, Apply<Cap, string>>>,
    Station2
>; // ["Tablet", "Robot"]
```

## 💻 Решение

```typescript
import type { Expect, Equal } from "type-testing";

// Решение
declare const arg: unique symbol;
type Arg = typeof arg;

interface Fn {
    [arg]: unknown;
    return: unknown;
}

type Apply<TFn extends Fn, TArg> = (TFn & {
    [arg]: TArg;
})["return"];

interface Cap extends Fn {
    return: Capitalize<this[Arg] & string>;
}

interface Push extends Fn {
    return: PushFunc<this[Arg]>;
}

interface PushFunc<T> extends Fn {
    return: this[Arg] extends unknown[] ? [...this[Arg], T] : never;
}

interface ApplyAll extends Fn {
    return: this[Arg] extends Fn ? ApplyAllFunc<this[Arg]> : never;
}

interface ApplyAllFunc<TFn extends Fn, U extends unknown[] = []> extends Fn {
    impl: U extends [infer F, ...infer R] ? [Apply<TFn, F>, ...ApplyAllFunc<TFn, R>["impl"]] : [];
    return: this[Arg] extends unknown[] ? ApplyAllFunc<TFn, this[Arg]>["impl"] : never;
}

interface Extends extends Fn {
    return: ExtendsFunc<this[Arg]>;
}

interface ExtendsFunc<T> extends Fn {
    return: this[Arg] extends T ? true : false;
}

interface Filter extends Fn {
    return: this[Arg] extends Fn ? FilterFunc<this[Arg]> : never;
}

interface FilterFunc<TFn extends Fn, U extends unknown[] = []> extends Fn {
    impl: U extends [infer F, ...infer R]
        ? Apply<TFn, F> extends true
            ? [F, ...FilterFunc<TFn, R>["impl"]]
            : FilterFunc<TFn, R>["impl"]
        : [];
    return: this[Arg] extends unknown[] ? FilterFunc<TFn, this[Arg]>["impl"] : never;
}

// Тесты
type t0_actual = Apply<Cap, "hello">;
type t0_expected = "Hello";
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = Apply<
    Apply<Push, "world">,
    ["hello"]
>;
type t1_expected = ["hello", "world"];
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = Apply<
    Apply<ApplyAll, Cap>,
    Apply<Apply<Push, "world">, ["hello"]>
>;
type t2_expected = ["Hello", "World"];
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = Apply<
    Apply<Filter, Apply<Extends, number>>,
    [1, "foo", 2, 3, "bar", true]
>;
type t3_expected = [1, 2, 3];
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type Station1 = Apply<Cap, "robot">;
type Station2 = Apply<Apply<Push, Station1>, ["Tablet", "teddy bear"]>;
type Station3 = Apply<
    Apply<Filter, Apply<Extends, Apply<Cap, string>>>,
    Station2
>;
type t4_actual = Station3;
type t4_expected = ["Tablet", "Robot"];
type t4 = Expect<Equal<t4_actual, t4_expected>>;
```