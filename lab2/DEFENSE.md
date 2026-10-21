# DEFENSE — Завдання 2: Вимоги та Use Cases

### Намір і критерії

Вимоги описують перевірку коментарів сервісом модерації, рішення модератора і видимість обговорення через його нульовий коментар. 
Критерії: кожна вимога в EARS має критерій приймання, усі ID покриті сценаріями, діаграма і матриця узгоджені між собою.

### Топ-3 розбіжності (модель ↔ spec) 

1. **Немає стану коментаря.** spec.md має три стани (опублікований, обмежений, видалений) і `reason`, а в `COMMENT` їх немає. [Коміт](https://github.com/tevarindol/forum/commit/0bda519).
2. **Немає тексту обговорення.** REQ-04 зберігає текст як нульовий коментар, а в `DISCUSSION` немає поля для тексту. [Коміт](https://github.com/tevarindol/forum/commit/0bda519).
3. **Вигадані вимоги в EARS.** Перша версія `lab2/ears.md` не відповідала `lab2/spec.md`. [Спроба 1](https://github.com/tevarindol/forum/commit/890ddea), [Спроба 2](https://github.com/tevarindol/forum/commit/a5ea78f) та [Спроба 3](https://github.com/tevarindol/forum/commit/d819c74).

### Ключове рішення

[0003 Автомтаична модерація](../docs/decisions/0003-automatic-comment-moderation.md).
[0004 Нульовий коментар](../docs/decisions/0004-zero-comment-for-discussion-text.md).

### Перевірка — узгодженість (як перевірив)

Звірював кожен раз після ШІ промпту spec.md з тим що ШІ генерувала (ears.md, usecases.puml, scenarions.md). Згенеровану usecases.svg перевіряв візуально.

Узгодженість з попередньою моделлю не можу гарантувати, spec.md порушує 2 сутності з model.mmd.

**Здача:** (а) посилання на GitHub PR; (б) цей DEFENSE.
