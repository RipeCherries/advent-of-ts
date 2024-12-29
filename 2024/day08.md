# Ночь с миссис Клаус

## 🇺🇸 Описание

> [🎅 Santa's noticed that 💋 Crystal Claus (his wife) is a little "on-edge" lately. 
> He doesn't know why, but he wants to give her some much needed attention. 
> 🎅 Santa doesn't know about 💋 Crystal's affair with 🪩 Jamie Glitterglum]
>
> [🎅 Santa, drawing 💋 Crystal in for a kiss] 💋 Crystal, my sweet, I thought we'd draw a 
> bath and take the night to ourselves.
>
> [💋 Crystal] Oh, I had plans dear.
>
> [🎅 Santa] Surely you can reschedule?
>
> [💋 Crystal] Um. I guess I could.
> 
> [🎅 Santa] Is that a new perfume you're wearing? Where have I smelled that before? It's heavenly.
>
> [💋 Crystal, quickly and nervously] You know what! No problem! Let's get to it!
>
> [🎅 Santa] Marvelous, now I just need to freshen up the environment. 
> I think I'll turn on the mood lights, prepare some chocolate strawberries, and set the bath to 327.59 degrees. 
> Not too cold, not too hot. And compliant with ISO 3103 standards for liquid enjoyment.
>
> Only 🎅 Santa, only you could turn romance into a NASA launch checklist.
> Still, let's help 🎅 Santa set the environment up for a night with Mrs. Claus.

## 🇷🇺 Описание

> [🎅 Санта заметил, что 💋 Кристалл Клаус (его жена) в последнее время немного "на нервах". 
> Он не знает почему, но хочет уделить ей немного внимания. 🎅 Санта не подозревает о 
> романе 💋 Кристалл с 🪩 Джейми Глиттерглумом.]
> 
> [🎅 Санта, притягивая 💋 Кристалл для поцелуя] 💋 Кристалл, моя дорогая, я подумал, 
> что мы могли бы принять ванну и провести вечер только вдвоём.
> 
> [💋 Кристалл] Ох, у меня были планы, дорогой.
> 
> [🎅 Санта] Разве их нельзя перенести?
> 
> [💋 Кристалл] Эм. Думаю, могу.
> 
> [🎅 Санта] Это новый парфюм? Где-то я уже чувствовал этот запах. Просто божественно.
> 
> [💋 Кристалл, быстро и нервно] Знаешь что! Без проблем! Давай займёмся этим!
> 
> [🎅 Санта] Прекрасно. Теперь мне нужно только подготовить атмосферу. Я включу приглушённый свет, 
> подготовлю клубнику в шоколаде и настрою ванну на 327,59 градусов. Не слишком холодно, не слишком горячо. 
> И в полном соответствии со стандартами ISO 3103 для наслаждения жидкостью.

Только 🎅 Санта мог превратить романтику в чек-лист для запуска NASA. Всё же, давайте поможем 🎅 Санте создать 
идеальную атмосферу для вечера с миссис Клаус.

## 💻 Решение

Для решения задачи используем механизм `Module Augmentation`. `process.env` является окружением `NodeJS`,
поэтому обращаемся к нему как к модулю и изменяем поля в интерфейсе `ProcessEnv`.

```typescript
import type { Expect, Equal } from 'type-testing';

// Решение
declare module NodeJS {
    interface ProcessEnv {
        MOOD_LIGHTS: 'true',
        BATH_TEMPERATURE: '327.59',
        STRAWBERRIES: 'chocolate'
    }
}

// Тесты
const {MOOD_LIGHTS} = process.env;
type t0_actual = typeof MOOD_LIGHTS;
type t0_expected = 'true';
type t0 = Expect<Equal<t0_actual, t0_expected>>;

const {BATH_TEMPERATURE} = process.env;
type t1_actual = typeof BATH_TEMPERATURE;
type t1_expected = '327.59';
type t1 = Expect<Equal<t1_actual, t1_expected>>;

const {STRAWBERRIES} = process.env;
type t2_actual = typeof STRAWBERRIES;
type t2_expected = 'chocolate';
type t2 = Expect<Equal<t2_actual, t2_expected>>;
```