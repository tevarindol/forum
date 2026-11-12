# DEFENSE - Завдання 3: Взаємодія (Sequence)

**1. Що я просив у AI.** На основі `lab3/spec.md` побудувати окрему Mermaid sequence-діаграму для кожного сценарію створення коментаря, дотримавшись меж UC, сигнатур і критеріїв активації. Промпт: [`ai/0019-lab3-sequence-diagrams.md`](ai/0019-lab3-sequence-diagrams.md).

**2. Топ-3 розбіжності, які я знайшов і виправив** (`spec.md` <-> вихід AI):

| # | Що було не так (критерій `spec.md`) | Мій слід (промпт / фідбек / spec)                                    | Виправлення (коміт)                                                                                                                                                                                                  |
|---|---|----------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 | Одна діаграма змішувала незалежні сценарії; відновлення й видалення були поза межами UC_Comment. | [`ai/0017`](ai/0017-lab3-sequence-diagram.md), [`spec.md`](spec.md)  | [spec](https://github.com/tevarindol/forum/commit/63e5675fd17b49a3a931fd7a2e8e02dbeb1ee0cc), [діаграми](https://github.com/tevarindol/forum/commit/0b4fe0d126f8371354643e3443328d216d4dc4bc)                         |
| 2 | Не було activation bars, зокрема активації Контролера навколо `alt`. | [`ai/0019`](ai/0019-lab3-sequence-diagrams.md), [`spec.md`](spec.md) | [діаграми](https://github.com/tevarindol/forum/commit/0b4fe0d126f8371354643e3443328d216d4dc4bc)                                                                                                                      |
| 3 | Виклик передачі автора був неоднозначний: `spec.md` задавав `authorId`, а діаграма передавала `userId`. | [`ai/0017`](ai/0017-lab3-sequence-diagram.md), [`spec.md`](spec.md)  | [сигнатура уточнена](https://github.com/tevarindol/forum/commit/7e7f72686cb824d90b96b946cb5135c2092924d9), [діаграма узгоджена](https://github.com/tevarindol/forum/commit/0b4fe0d126f8371354643e3443328d216d4dc4bc) |

**3. Ключове рішення:** створення та зміна стану коментаря мають окремі функції [`ADR 0005`](../docs/decisions/0005-two-functions-for-comment-writes.md). У цій роботі використовується `insertComment`; `saveComment` поза межами.

**4. Перевірка:** зіставив чотири діаграми в [`sequence.md`](sequence.md) з REQ-02b/REQ-07a/b/c та відповідними Gherkin-сценаріями в `lab2/scenarios.md`. Перевірив поріг `0.8`, таймаут 2 с, `reason`, порядок `checkContent` -> `insertComment`, активації учасників і використання `DISCUSSION.id`, `USER.id`, `COMMENT.discussion_id`, `COMMENT.author_id`, `COMMENT.body`. `state` і `reason` відсутні в ER; цю розбіжність зафіксував у [`spec.md`](spec.md), не змінюючи ER-модель лабораторної 1.
