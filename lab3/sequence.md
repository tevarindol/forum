# Взаємодія UC_Comment: створення коментаря

Кожна діаграма показує окремий сценарій із `lab3/spec.md`. Активаційні смуги показані лише для учасників, які виконують дії у відповідному сценарії.

## Гість отримує відмову (REQ-02b)

```mermaid
sequenceDiagram
    actor U as Користувач (гість)
    participant C as Контролер коментарів

    U->>C: createComment(discussionId, body)
    activate C
    C->>C: getCurrentUser()
    C-->>C: userId порожній
    break Гість не автентифікований (REQ-02b)
        C-->>U: відмова в доступі, коментар не створено
    end
    deactivate C
```

## Коментар опубліковано (REQ-07c)

```mermaid
sequenceDiagram
    actor U as Користувач
    participant C as Контролер коментарів
    participant MS as Сервіс модерації
    participant DB as Сховище COMMENT

    U->>C: createComment(discussionId, body)
    activate C
    C->>C: getCurrentUser()
    C-->>C: userId
    C->>MS: checkContent(body)
    activate MS
    MS-->>C: score ≤ 0.8 протягом 2 с
    deactivate MS
    Note over C: state = опублікований, reason порожній
    Note over C,DB: authorId = userId з getCurrentUser()
    C->>DB: insertComment(discussionId, authorId, body, опублікований, порожньо)
    activate DB
    DB-->>C: commentId
    deactivate DB
    C-->>U: commentId, state = опублікований
    deactivate C
```

## Коментар обмежено за оцінкою (REQ-07a)

```mermaid
sequenceDiagram
    actor U as Користувач
    participant C as Контролер коментарів
    participant MS as Сервіс модерації
    participant DB as Сховище COMMENT

    U->>C: createComment(discussionId, body)
    activate C
    C->>C: getCurrentUser()
    C-->>C: userId
    C->>MS: checkContent(body)
    activate MS
    MS-->>C: score > 0.8 протягом 2 с
    deactivate MS
    Note over C: state = обмежений, reason = 1
    Note over C,DB: authorId = userId з getCurrentUser()
    C->>DB: insertComment(discussionId, authorId, body, обмежений, 1)
    activate DB
    DB-->>C: commentId
    deactivate DB
    C-->>U: commentId, state = обмежений
    deactivate C
```

## Коментар обмежено через таймаут (REQ-07b)

```mermaid
sequenceDiagram
    actor U as Користувач
    participant C as Контролер коментарів
    participant MS as Сервіс модерації
    participant DB as Сховище COMMENT

    U->>C: createComment(discussionId, body)
    activate C
    C->>C: getCurrentUser()
    C-->>C: userId
    C->>MS: checkContent(body)
    activate MS
    alt Відповідь не надійшла протягом 2 с (REQ-07b)
        Note over C,MS: Таймаут 2 с, оцінки немає
        Note over C: state = обмежений, reason = 2
        Note over C,DB: authorId = userId з getCurrentUser()
        C->>DB: insertComment(discussionId, authorId, body, обмежений, 2)
        activate DB
        DB-->>C: commentId
        deactivate DB
        C-->>U: commentId, state = обмежений
    else Відповідь надійшла протягом 2 с
        MS-->>C: score
        Note over C: Гілка обробки оцінки описана в окремих діаграмах REQ-07a і REQ-07c
    end
    deactivate MS
    deactivate C
```
