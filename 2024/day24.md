# Парсер-комбинаторы: Последний рубеж

## 🇺🇸 Описание

> [🎅 Santa] First the elves needed a code parser, then we had that JSON situation...
> I'm starting to see a pattern here.
>
> [🎩 Bernard] We keep building one-off parsers for everything! Maybe we should create a reusable parsing system?
>
> [🎅 Santa] Like some sort of parser factory? That's brilliant!
> We could use it for everything - toy specs, route planning... uh... everything!
>
> [🎩 Bernard] Right... I'll get started on the design.

The elves need your help building a type-safe parser combinator system! Instead of creating individual parsers
for each format, you'll create building blocks that can be combined to parse anything.

Here's how the elves want to use it:

```typescript
// define simple parsers
type DigitParser = Parse<Just, Digit>;

// combine into more complex parsers
type IntParser = Parse<
    MapResult,
    [Parse<Many1, DigitParser>, Join, StringToNumber]
>;

// parse!
type Parsed = Parse<IntParser, "123.4ff">["data"]; // 123
```

Implement all of these core parser combinators:

* `Choice` - Tries each parser in order until one succeeds;
* `EOF` - Matches the end of input;
* `Just` - Matches exactly one character/token;
* `Left` - Matches the left parser;
* `Many0` - Matches zero or more of the parser;
* `Many1` - Matches one or more of the parser;
* `MapResult` - Maps the parsed data using the provided mapper;
* `Mapper` - A mapper is a function that transforms the parsed data;
* `Maybe` - Matches zero or one of the parser;
* `MaybeResult` - A result type for parsers that may fail;
* `NoneOf` - Matches none of the characters/tokens;
* `Pair` - Matches two parsers in sequence;
* `Parse` - The core parser type;
* `Parser` - A parser is a function that attempts to parse an input string;
* `Right` - Matches the right parser;
* `SepBy0` - Matches zero or more of the parser separated by the separator parser;
* `Seq` - Matches two parsers in sequence.

Each parser should return a result type containing:

* `success`: Whether the parse succeeded
* `data`: The parsed data
* `rest`: The remaining unparsed input

## 🇷🇺 Описание

> [🎅 Санта] Сначала эльфам понадобился парсер для кода, потом был случай с JSON...
> Я начинаю видеть здесь закономерность.
>
> [🎩 Бернард] Мы всё время пишем одноразовые парсеры для каждого случая! Может быть,
> стоит создать переиспользуемую систему парсинга?
>
> [🎅 Санта] Как своего рода фабрику парсеров? Это гениально!
> Мы могли бы использовать её для всего — спецификаций игрушек, планирования маршрутов... э-э... для всего!
>
> [🎩 Бернард] Верно... Я займусь проектированием.

Эльфы нуждаются в вашей помощи, чтобы создать безопасную с точки зрения типов систему парсер-комбинаторов!
Вместо создания отдельных парсеров для каждого формата, вам нужно создать строительные блоки,
которые можно комбинировать для парсинга чего угодно.

Вот как эльфы хотят это использовать:

```typescript
// определяем простые парсеры
type DigitParser = Parse<Just, Digit>;

// комбинируем их в более сложные парсеры
type IntParser = Parse<
    MapResult,
    [Parse<Many1, DigitParser>, Join, StringToNumber]
>;

// парсим!
type Parsed = Parse<IntParser, "123.4ff">["data"]; // 123
```

Вам нужно реализовать следующие основные парсер-комбинаторы:

* `Choice` - Пробует каждый парсер по порядку, пока один не сработает;
* `EOF` - Совпадает с концом ввода;
* `Just` - Совпадает ровно с одним символом/токеном;
* `Left` - Совпадает с левым парсером;
* `Many0` - Совпадает с нулем или более экземплярами парсера;
* `Many1` - Совпадает с одним или более экземплярами парсера;
* `MapResult` - Преобразует данные парсинга с помощью указанного маппера;
* `Mapper` - Маппер — это функция, которая преобразует данные парсинга;
* `Maybe` - Совпадает с нулем или одним экземпляром парсера;
* `MaybeResult` - Тип результата для парсеров, которые могут завершиться неудачей;
* `NoneOf` - Совпадает с отсутствием указанных символов/токенов;
* `Pair` - Совпадает с двумя парсерами последовательно;
* `Parse` - Основной тип парсера;
* `Parser` - Парсер — это функция, которая пытается обработать строку ввода;
* `Right` - Совпадает с правым парсером;
* `SepBy0` - Совпадает с нулем или более экземплярами парсера, разделенными указанным разделителем;
* `Seq` - Совпадает с двумя парсерами последовательно.

Каждый парсер должен возвращать тип результата, содержащий:

* `success`: Удалось ли выполнить парсинг;
* `data`: Распарсенные данные;
* `rest`: Оставшийся необработанный ввод.

## 💻 Решение

```typescript
import type { Expect, Equal } from "type-testing";

// Решение
declare const _: unique symbol

type _ = typeof _

type ParserFn = (...args: never[]) => unknown

type ParserInput<T extends Parser> = T extends ParserHKT ? ParserInputHelper<T> : any

type ParserInputHelper<T extends ParserHKT> = Parameters<T['fn']>[0]

type ParserResult<Data = never, Rest extends string = string> = Omit<
    ([Data] extends [never]
        ? { success: false }
        : {
            success: true
            data: Data
        }) & { rest: Rest },
    never
>

abstract class ParserHKT {
    [_]: unknown
    abstract fn: ParserFn
}

interface Just extends ParserHKT {
    fn: (item: Cast<this[_], string>) => JustImpl<typeof item>
}

interface JustImpl<Item extends string> extends ParserHKT {
    fn: (
        data: Cast<this[_], string>
    ) => typeof data extends `${infer Data extends Item}${infer Rest}`
        ? ParserResult<Data, Rest>
        : ParserResult<never, typeof data>
}

interface NoneOf extends ParserHKT {
    fn: (chars: Cast<this[_], string>) => NoneOfImpl<typeof chars>
}

interface NoneOfImpl<Chars extends string> extends ParserHKT {
    fn: (
        data: Cast<this[_], string>
    ) => typeof data extends `${infer Head}${infer Rest}`
        ? Head extends Chars
            ? ParserResult<never, typeof data>
            : ParserResult<Head, Rest>
        : ParserResult<never, typeof data>
}

interface EOF extends ParserHKT {
    fn: (
        data: Cast<this[_], string>
    ) => '' extends typeof data ? ParserResult<'', typeof data> : ParserResult<never, typeof data>
}

type MaybeResult<Data = unknown> = { success: true; data: Data } | { success: false }

interface Maybe extends ParserHKT {
    fn: (parser: Cast<this[_], Parser>) => MaybeImpl<typeof parser>
}

interface MaybeImpl<P extends Parser> extends ParserHKT {
    fn: (
        data: Cast<this[_], ParserInput<P>>
    ) => Parse<P, typeof data> extends infer R extends ParserResult<unknown, string>
        ? true extends R['success']
            ? ParserResult<{ success: true; data: R['data'] }, R['rest']>
            : ParserResult<{ success: false }, typeof data>
        : never
}

interface Left extends ParserHKT {
    fn: (
        parsers: Cast<this[_], [Parser, Parser]>
    ) => LeftImpl<(typeof parsers)[0], (typeof parsers)[1]>
}

interface LeftImpl<L extends Parser, R extends Parser> extends ParserHKT {
    fn: (
        data: Cast<this[_], ParserInput<L>>
    ) => $Pair<L, R, typeof data> extends ParserResult<
            infer Data extends [unknown, unknown],
            infer Rest
        >
        ? ParserResult<Data[0], Rest>
        : never
}

interface Right extends ParserHKT {
    fn: (
        parsers: Cast<this[_], [Parser, Parser]>
    ) => RightImpl<(typeof parsers)[0], (typeof parsers)[1]>
}

interface RightImpl<L extends Parser, R extends Parser> extends ParserHKT {
    fn: (
        data: Cast<this[_], ParserInput<L>>
    ) => $Pair<L, R, typeof data> extends ParserResult<
            infer Data extends [unknown, unknown],
            infer Rest
        >
        ? ParserResult<Data[1], Rest>
        : never
}

interface Pair extends ParserHKT {
    fn: (
        parsers: Cast<this[_], [Parser, Parser]>
    ) => PairImpl<(typeof parsers)[0], (typeof parsers)[1]>
}

interface PairImpl<L extends Parser, R extends Parser> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<L>>) => $Pair<L, R, typeof data>
}

type $Pair<L extends Parser, R extends Parser, Data extends ParserInput<L>> =
    Parse<L, Data> extends infer LRes extends ParserResult<unknown, string>
        ? false extends LRes['success']
            ? ParserResult<never, Data>
            : Parse<R, Cast<LRes['rest'], ParserInput<R>>> extends infer RRes extends ParserResult<
                    unknown,
                    string
                >
                ? false extends RRes['success']
                    ? ParserResult<never, Data>
                    : ParserResult<[LRes['data'], RRes['data']], RRes['rest']>
                : never
        : never

interface Seq extends ParserHKT {
    fn: (parsers: Cast<this[_], Array<Parser>>) => SeqImpl<typeof parsers>
}

interface SeqImpl<P extends Array<Parser>> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<P[number]>>) => $Seq<P, typeof data>
}

type $Seq<
    P extends Array<Parser>,
    Data extends ParserInput<P[number]>,
    OriginalData extends ParserInput<P[number]> = Data,
    Acc extends unknown[] = [],
> = P extends [infer Head extends Parser, ...infer Rest extends Array<Parser>]
    ? Parse<Head, Data> extends infer R extends ParserResult<unknown, string>
        ? false extends R['success']
            ? ParserResult<never, OriginalData>
            : $Seq<Rest, Cast<R['rest'], ParserInput<P[number]>>, OriginalData, [...Acc, R['data']]>
        : never
    : ParserResult<Acc, Data>

interface Choice extends ParserHKT {
    fn: (parsers: Cast<this[_], Array<Parser>>) => ChoiceImpl<typeof parsers>
}

interface ChoiceImpl<P extends Array<Parser>> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<P[number]>>) => $Choice<P, typeof data>
}

type $Choice<P extends Array<Parser>, Data extends ParserInput<P[number]>> = P extends [
        infer Head extends Parser,
        ...infer Rest extends Array<Parser>,
    ]
    ? Parse<Head, Data> extends infer R extends ParserResult<unknown, string>
        ? true extends R['success']
            ? R
            : $Choice<Rest, Data>
        : never
    : ParserResult<never, Data>

interface Many0 extends ParserHKT {
    fn: (parser: Cast<this[_], Parser>) => Many0Impl<typeof parser>
}

interface Many0Impl<P extends Parser> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<P>>) => $Many0<P, typeof data>
}

type $Many0<P extends Parser, Data extends ParserInput<P>, Acc extends readonly unknown[] = []> =
    Parse<P, Data> extends infer R extends ParserResult<unknown, string>
        ? true extends R['success']
            ? $Many0<P, Cast<R['rest'], ParserInput<P>>, [...Acc, R['data']]>
            : ParserResult<Acc, Data>
        : never

interface Many1 extends ParserHKT {
    fn: (parser: Cast<this[_], Parser>) => Many1Impl<typeof parser>
}

interface Many1Impl<P extends Parser> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<P>>) => $Many1<P, typeof data>
}

type $Many1<P extends Parser, Data extends ParserInput<P>> =
    $Many0<P, Data> extends infer R extends ParserResult<unknown[], string>
        ? [] extends R['data']
            ? ParserResult<never, Data>
            : R
        : never

interface SepBy0 extends ParserHKT {
    fn: (input: Cast<this[_], [Parser, Parser]>) => SepBy0Impl<(typeof input)[0], (typeof input)[1]>
}

interface SepBy0Impl<P extends Parser, Sep extends Parser> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<P>>) => $SepBy0<P, Sep, typeof data>
}

type $SepBy0<P extends Parser, Sep extends Parser, Data extends ParserInput<P>> =
    Parse<P, Data> extends infer R extends ParserResult<unknown, string>
        ? false extends R['success']
            ? ParserResult<[], Data>
            : Parse<Right, [Parse<Many0, Sep>, P]> extends infer Rec extends ParserHKT
                ? SepBy0Recurse<Rec, Cast<R['rest'], ParserInput<Rec>>> extends infer RR extends
                        ParserResult<unknown[], string>
                    ? ParserResult<[R['data'], ...RR['data']], RR['rest']>
                    : never
                : never
        : never

type SepBy0Recurse<P extends ParserHKT, Data extends ParserInput<P>, Acc extends unknown[] = []> =
    Parse<P, Data> extends infer R extends ParserResult<unknown, string>
        ? false extends R['success']
            ? ParserResult<Acc, Data>
            : SepBy0Recurse<P, Cast<R['rest'], ParserInput<P>>, [...Acc, R['data']]>
        : ParserResult<Acc, Data>

interface SepBy1 extends ParserHKT {
    fn: (input: Cast<this[_], [Parser, Parser]>) => SepBy1Impl<(typeof input)[0], (typeof input)[1]>
}

interface SepBy1Impl<P extends Parser, Sep extends Parser> extends ParserHKT {
    fn: (data: Cast<this[_], ParserInput<P>>) => $SepBy1<P, Sep, typeof data>
}

type $SepBy1<P extends Parser, Sep extends Parser, Data extends ParserInput<P>> =
    $SepBy0<P, Sep, Data> extends infer R extends ParserResult<unknown, string>
        ? [] extends R['data']
            ? ParserResult<never, Data>
            : R
        : never

interface TryParseAll extends ParserHKT {
    fn: (data: Cast<this[_], Parser>) => TryParseAllImpl<typeof data>
}

interface TryParseAllImpl<P extends Parser> extends ParserHKT {
    fn: (data: Cast<this[_], string>) => $TryParseAll<typeof data, P>
}

type $TryParseAll<T extends string, P extends Parser, Acc extends unknown[] = []> = '' extends T
    ? ParserResult<Acc, T>
    : Parse<P, Cast<T, ParserInput<P>>> extends infer R extends ParserResult<any, string>
        ? R['success'] extends true
            ? $TryParseAll<R['rest'], P, [...Acc, R['data']]>
            : T extends `${string}${infer Rest}`
                ? $TryParseAll<Rest, P, Acc>
                : ParserResult<Acc, T>
        : never

abstract class Mapper {
    data: unknown
    abstract map: (data: never) => unknown
}

interface MapResult extends ParserHKT {
    fn: (
        input: Cast<this[_], [Parser, ...Mapper[]]>
    ) => typeof input extends [infer P extends Parser, ...infer M extends Mapper[]]
        ? MapResultImpl<P, M>
        : never
}

interface MapResultImpl<P extends Parser, M extends Mapper[]> extends ParserHKT {
    fn: (
        input: Cast<this[_], ParserInput<P>>
    ) => Parse<P, typeof input> extends infer R extends ParserResult<unknown, string>
        ? false extends R['success']
            ? R
            : ParserResult<$MapResult<M, R['data']>, R['rest']>
        : never
}

type $MapResult<M extends Mapper[], Data> = M extends [
        infer Head extends Mapper,
        ...infer Rest extends Mapper[],
    ]
    ? $MapResult<Rest, ReturnType<(Head & { data: Data })['map']>>
    : Data

type Lazy<P = any> = () => P

type Eval<P extends Parser> = P extends Lazy
    ? ReturnType<P> extends infer T extends ParserHKT
        ? T
        : never
    : P extends ParserHKT
        ? P
        : never

type Cast<T, U> = T extends U ? T : U

type Parse<P extends Parser, Data extends ParserInput<P>> = Eval<P> & {
    [_]: Data
} extends infer T extends ParserHKT
    ? ReturnType<T['fn']> extends infer R
        ? R
        : never
    : never

type Parser = ParserHKT | Lazy

interface UnwrapMaybe extends Mapper {
    map: (data: this['data']) => typeof data extends unknown[] ? $UnwrapMaybe<typeof data> : never
}

type $UnwrapMaybe<T extends unknown[], Acc extends unknown[] = []> = T extends [
        infer Head,
        ...infer Rest,
    ]
    ? $UnwrapMaybe<
        Rest,
        Head extends MaybeResult
            ? Head extends { success: true; data: infer Data }
                ? [...Acc, Data]
                : Acc
            : [...Acc, Head]
    >
    : Acc

interface Flatten extends Mapper {
    map: (data: this['data']) => typeof data extends unknown[] ? $Flatten<typeof data> : never
}

type $Flatten<Array extends readonly any[]> = Array extends [infer Head, ...infer Rest]
    ? Head extends readonly any[]
        ? [...$Flatten<Head>, ...$Flatten<Rest>]
        : [Head, ...$Flatten<Rest>]
    : []

interface Join extends Mapper {
    map: (data: this['data']) => typeof data extends string[] ? $Join<typeof data> : never
}

type $Join<T extends string[], Acc extends string = ''> = T extends [
        infer Head extends string,
        ...infer Rest extends string[],
    ]
    ? $Join<Rest, `${Acc}${Head}`>
    : Acc

// Тесты
type t0_actual = Parse<JSONParser, '"hello"'>["data"];
type t0_expected = "hello";
type t0 = Expect<Equal<t0_actual, t0_expected>>;

type t1_actual = Parse<JSONParser, '"hello\nworld"'>["data"];
type t1_expected = "hello\nworld";
type t1 = Expect<Equal<t1_actual, t1_expected>>;

type t2_actual = Parse<JSONParser, '{"hello": "world"}'>["data"];
type t2_expected = { hello: "world" };
type t2 = Expect<Equal<t2_actual, t2_expected>>;

type t3_actual = Parse<JSONParser, '[1, "hello", null, 4, "world"]'>["data"];
type t3_expected = [1, "hello", null, 4, "world"];
type t3 = Expect<Equal<t3_actual, t3_expected>>;

type t4_actual = Parse<JSONParser, '{ "a": { "b": "c" } }'>["data"];
type t4_expected = { a: { b: "c" } };
type t4 = Expect<Equal<t4_actual, t4_expected>>;

type t5_actual = Parse<
    JSONParser,
    '[{ "foo": "bar" }, { "foo": "baz" }, { "foo": 123 }]'
>["data"];
type t5_expected = [{ foo: "bar" }, { foo: "baz" }, { foo: 123 }];
type t5 = Expect<Equal<t5_actual, t5_expected>>;

type t6_actual = Parse<JSONParser, "[1, 2, 3">["success"];
type t6_expected = false;
type t6 = Expect<Equal<t6_actual, t6_expected>>;

type t7_actual = Parse<JSONParser, "{ foo: 123 }">["success"];
type t7_expected = false;
type t7 = Expect<Equal<t7_actual, t7_expected>>;

type t8_actual = Parse<JSONParser, "{">["success"];
type t8_expected = false;
type t8 = Expect<Equal<t8_actual, t8_expected>>;

type t9_actual = Parse<JSONParser, "[1, 2, 3,]">["success"];
type t9_expected = false;
type t9 = Expect<Equal<t9_actual, t9_expected>>;

type t10_actual = Parse<JSONParser, "\\,">["success"];
type t10_expected = false;
type t10 = Expect<Equal<t10_actual, t10_expected>>;

// base parsers
type Whitespace = " " | "\t" | "\n";

type Whitespace0 = Parse<Many0, Parse<Just, Whitespace>>;

type Digit = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9";

type Digits0 = Parse<Many0, Parse<Just, Digit>>;
type Digits1 = Parse<Many1, Parse<Just, Digit>>;

type Between<L extends Parser, R extends Parser, P extends Parser> = Parse<
    Left,
    [Parse<Right, [L, P]>, R]
>;

type Str<
    T extends string,
    Acc extends Parser[] = [],
> = T extends `${infer Head}${infer Rest}`
    ? Str<Rest, [...Acc, Parse<Just, Head>]>
    : Parse<MapResult, [Parse<Seq, Acc>, Join]>;

type Padded<P extends Parser> = Parse<Left, [P, Whitespace0]>;

type Sym<T extends string> = Padded<Str<T>>;

type JSONParser = Between<Whitespace0, EOF, JSONValueParser>;

type JSONValueParser = () => Padded<
    Parse<
        Choice,
        [
            JSONNullParser,
            JSONBooleanParser,
            JSONStringParser,
            JSONNumberParser,
            JSONArrayParser,
            JSONObjectParser,
        ]
    >
>;

// `null`
type JSONNullParser = Parse<MapResult, [Str<"null">, ToLiteral<null>]>;

// boolean
type JSONBooleanParser = Parse<
    Choice,
    [
        Parse<MapResult, [Str<"true">, ToLiteral<true>]>,
        Parse<MapResult, [Str<"false">, ToLiteral<false>]>,
    ]
>;

// string
type JSONStringParser = Parse<
    MapResult,
    [
        Between<
            Parse<Just, '"'>,
            Parse<Just, '"'>,
            Parse<
                Many0,
                Parse<
                    Choice,
                    [
                        Parse<NoneOf, '"' | "\\">,
                        Parse<
                            Right,
                            [
                                Parse<Just, "\\">,
                                Parse<
                                    Choice,
                                    [
                                        Parse<Just, "\\">,
                                        Parse<Just, "/">,
                                        Parse<Just, '"'>,
                                        Parse<MapResult, [Parse<Just, "b">, ToLiteral<"\u0008">]>,
                                        Parse<MapResult, [Parse<Just, "f">, ToLiteral<"\u000c">]>,
                                        Parse<MapResult, [Parse<Just, "n">, ToLiteral<"\n">]>,
                                        Parse<MapResult, [Parse<Just, "r">, ToLiteral<"\r">]>,
                                        Parse<MapResult, [Parse<Just, "t">, ToLiteral<"\t">]>,
                                    ]
                                >,
                            ]
                        >,
                    ]
                >
            >
        >,
        Join,
    ]
>;

// number
type JSONNumberParser = Parse<
    MapResult,
    [
        Parse<
            Seq,
            [
                Parse<Maybe, Parse<Just, "-">>,
                Parse<
                    Choice,
                    [
                        Parse<Just, "0">,
                        Parse<Pair, [Parse<Just, Exclude<Digit, "0">>, Digits0]>,
                    ]
                >,
                Parse<Maybe, Parse<Pair, [Parse<Just, ".">, Digits1]>>,
            ]
        >,
        UnwrapMaybe,
        Flatten,
        Join,
        StringToNumber,
    ]
>;

// array
type JSONArrayParser = Between<
    Sym<"[">,
    Sym<"]">,
    Parse<SepBy0, [JSONValueParser, Sym<",">]>
>;

// object
type JSONObjectParser = Between<
    Sym<"{">,
    Sym<"}">,
    Parse<
        MapResult,
        [
            Parse<
                SepBy0,
                [
                    Parse<
                        MapResult,
                        [
                            Parse<
                                Pair,
                                [
                                    Parse<Left, [Padded<JSONStringParser>, Sym<":">]>,
                                    JSONValueParser,
                                ]
                            >,
                            MakeObject,
                        ]
                    >,
                    Sym<",">,
                ]
            >,
            IntersectAll,
        ]
    >
>;

// mappers
interface ToLiteral<T> extends Mapper {
    map: () => T;
}

interface Flatten extends Mapper {
    map: (
        data: this["data"],
    ) => typeof data extends unknown[] ? DoFlatten<typeof data> : never;
}

type DoFlatten<Array extends readonly unknown[]> = Array extends [
        infer Head,
        ...infer Rest,
    ]
    ? Head extends readonly unknown[]
        ? [...DoFlatten<Head>, ...DoFlatten<Rest>]
        : [Head, ...DoFlatten<Rest>]
    : [];

interface UnwrapMaybe extends Mapper {
    map: (
        data: this["data"],
    ) => typeof data extends unknown[] ? DoUnwrapMaybe<typeof data> : never;
}

type DoUnwrapMaybe<
    T extends unknown[],
    Acc extends unknown[] = [],
> = T extends [infer Head, ...infer Rest]
    ? DoUnwrapMaybe<
        Rest,
        Head extends MaybeResult
            ? Head extends { success: true; data: infer Data }
                ? [...Acc, Data]
                : Acc
            : [...Acc, Head]
    >
    : Acc;

interface StringToNumber extends Mapper {
    map: (
        data: this["data"],
    ) => typeof data extends string ? DoStringToNumber<typeof data> : never;
}

type DoStringToNumber<T extends string> = T extends `${infer N extends number}`
    ? N
    : never;

interface MakeObject extends Mapper {
    map: (
        data: this["data"],
    ) => typeof data extends [PropertyKey, unknown]
        ? { [Key in (typeof data)[0]]: (typeof data)[1] }
        : never;
}

interface IntersectAll extends Mapper {
    map: (
        data: this["data"],
    ) => typeof data extends object[] ? DoIntersectAll<typeof data> : never;
}

type DoIntersectAll<T extends object[], Acc extends object = {}> = T extends [
        infer Head,
        ...infer Rest extends object[],
    ]
    ? DoIntersectAll<Rest, Acc & Head>
    : Omit<Acc, never>;

export interface Join extends Mapper {
    map: (
        data: this["data"],
    ) => typeof data extends string[] ? DoJoin<typeof data> : never;
}

type DoJoin<T extends string[], Acc extends string = ""> = T extends [
        infer Head extends string,
        ...infer Rest extends string[],
    ]
    ? DoJoin<Rest, `${Acc}${Head}`>
    : Acc;

type DigitParser = Parse<Just, Digit>;
type IntParser = Parse<
    MapResult,
    [Parse<Many1, DigitParser>, Join, StringToNumber]
>;

type t11_actual = Parse<IntParser, "123.4ff">;
type t11_expected = {
    data: 123;
    rest: ".4ff";
    success: true;
};
type t11 = Expect<Equal<t11_actual, t11_expected>>;
```