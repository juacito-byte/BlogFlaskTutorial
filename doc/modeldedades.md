# modeldedades

# Model entitat relacio E/r

erDiagram
    User ||--o{ ORDER : id
    User {
        string username
        string password
    }
    ORDER {
        date created
        string title
        string body
    }