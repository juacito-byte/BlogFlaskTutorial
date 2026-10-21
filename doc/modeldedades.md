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

| End-Point | Method | Descripció |
| ----------- | ----------- | ----------- |
| /hello | GET |         |
| /register | GET |         |
| /register | POST |         |
| /login | GET |         |
| /login | POST |         |
| /logout | GET |         |
| / | GET |         |
| /create | GET |         |
| /create | POST |         |
| /<int:id>/update | GET |         |
| /<int:id>/update | POST |         |
| /<int:id>/delete | POST |         |