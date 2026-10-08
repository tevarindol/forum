# Специфікація моделі даних: Форум з програмування та операційних систем

Форум, де користувачі обговорюють програмування та операційні системи: створюють обговорення в спільнотах, пишуть коментарі, позначають обговорення тегами й ставлять реакції на коментарі. Модель описує лише предметну область (ER), а не фізичну схему БД.

## 1. Критерії прийняття (Acceptance Criteria)
- Відсутні дублювання даних і транзитивні залежності.
- Усі сутності мають первинний ключ типу `UUID`; усі зовнішні ключі теж `UUID`.
- Модель не містить персональних даних (email, ім'я, телефон); користувач ідентифікується лише `username`, пароль зберігається тільки як `password_hash` (див. ADR 0001).

## 2. Сутності та атрибути
- **USER:** `id` (UUID, PK), `username` (String), `password_hash` (String), `role` (Integer), `created_at` (DateTime).
- **COMMUNITY:** `id` (UUID, PK), `name` (String), `description` (String).
- **DISCUSSION:** `id` (UUID, PK), `author_id` (UUID, FK -> USER), `community_id` (UUID, FK -> COMMUNITY), `title` (String), `created_at` (DateTime).
- **COMMENT:** `id` (UUID, PK), `discussion_id` (UUID, FK -> DISCUSSION), `author_id` (UUID, FK -> USER), `body` (String), `created_at` (DateTime).
- **TAG:** `id` (UUID, PK), `name` (String).
- **REACTION:** `id` (UUID, PK), `user_id` (UUID, FK -> USER), `comment_id` (UUID, FK -> COMMENT), `type` (Integer), `created_at` (DateTime).

## 3. Зв'язки
- `COMMUNITY ||--o{ DISCUSSION` (1 : 0..N) — обговорення належить рівно одній спільноті.
- `USER ||--o{ DISCUSSION` (1 : 0..N) — користувач створює обговорення (автор).
- `DISCUSSION ||--o{ COMMENT` (1 : 0..N) — обговорення містить коментарі.
- `USER ||--o{ COMMENT` (1 : 0..N) — користувач пише коментарі (автор).
- `DISCUSSION }o--o{ TAG` (0..N : 0..N) — теги позначають обговорення.
- `USER ||--o{ REACTION` (1 : 0..N) — користувач ставить реакції.
- `COMMENT ||--o{ REACTION` (1 : 0..N) — коментар отримує реакції.