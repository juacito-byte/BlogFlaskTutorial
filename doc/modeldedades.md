# modeldedades

# Model entitat relacio E/r

```mermaid
erDiagram
    User ||--o{ Post : id
    User {
        string username
        string password
    }
    Post {
        date created
        string title
        string body
    }
```

| Color | Código |
| ----------- | ----------- |
| Rojo | #FF0000 |
| Azul | #0000FF |