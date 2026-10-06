# Форум з програмування та операційних систем

ER-модель даних форуму: користувачі створюють обговорення в спільнотах, пишуть коментарі, позначають обговорення тегами й ставлять реакції на коментарі. Специфікація — `spec.md`, ER-діаграма Mermaid — `model.mmd`.

```mermaid
erDiagram
    USER {
        UUID id PK
        String username
        String password_hash
        Integer role
        DateTime created_at
    }
    COMMUNITY {
        UUID id PK
        String name
        String description
    }
    DISCUSSION {
        UUID id PK
        UUID author_id FK
        UUID community_id FK
        String title
        DateTime created_at
    }
    COMMENT {
        UUID id PK
        UUID discussion_id FK
        UUID author_id FK
        String body
        DateTime created_at
    }
    TAG {
        UUID id PK
        String name
    }
    REACTION {
        UUID id PK
        UUID user_id FK
        UUID comment_id FK
        Integer type
        DateTime created_at
    }

    COMMUNITY ||--o{ DISCUSSION : "належить"
    USER ||--o{ DISCUSSION : "автор"
    DISCUSSION ||--o{ COMMENT : "містить"
    USER ||--o{ COMMENT : "автор"
    DISCUSSION }o--o{ TAG : "теги"
    USER ||--o{ REACTION : "ставить"
    COMMENT ||--o{ REACTION : "отримує"
```
