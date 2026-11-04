# Diagrama Components

Descriure tots els components de l'aplicacio

Components: db, _ init_ , auth, blog
## db
Descripcio: funcions d'acces a BBDD
Metodes
- get_db: conecta la aplicacion a la base de datos configurada
- close_db: si la peticion esta conectada a la base de datos que cierre esta conecion
- init_db: borra los datos existentes y crea nuevas tablas
- init_db_command: borra los datos existentes y crea nuevas tablas
- init_app: registra las funciones de la base de datos con flask

## blog
- index: muestra todos los post el mas reciente primero
- get_post: obtiene un post y la id de su autor verifica si la id existe y si el usuario actual es el autor
- create: crea un nuevo post para el usuario actual
- update: actualiza el post si el usuario actual es el autor
- delete: se asegura de que el post existe y que el usuario que se a loggeado es el author del post

## auth
- login_required: mira el decorador que redirecciona a los usuarios anonimos a la pagina de login
- load_logged_in_user: si el id del usuario esta guardado en la sesion carga el objeto del usuario de la base de datos a g.user
- register: Registra un nuevo usuario valida que el usuario no esta pillado y ponle hashes al pasword por seguridad
- login: que haga log un usuario ya registrado poniendo su id de usuario en la sesion
- logout: borrar el usuario actual incluyendo su id guardado

## _init_
- create_app: crea y configura una instancia de la aplicacion de flask

```mermaid
classDiagram
  class Init {
    +create_app()
  }
  class db {
    +get_db()
    +close_db()
    +init_db()
    +init_db_command()
    +init_app()
  }
  class auth {
    +login_required()
    +load_logged_in_user()
    +register()
    +login()
    +logout()
  }
  class blog {
    +index()
    +get_post()
    +create()
    +update()
    +delete()
  }
  Init  -->  db 
  Init  --> auth 
  Init --> blog 
```

```mermaid
sequenceDiagram
    autonumber
    actor Usuari as Usuari / Terminal
    participant CLI as CLI de Flask
    participant App as Aplicació Flask<br/>(create_app)
    participant DBModule as Mòdul de Base de Dades<br/>(db.py)
    participant SQLite as SQLite3 / Fitxer<br/>(flaskr.sqlite)

    Note over Usuari, SQLite: Execució de la comanda d'inicialització de la base de dades

    Usuari->>CLI: Executa: $ flask --app flaskr init-db
    
    activate CLI
    CLI->>App: Carrega i crida la factoria create_app()
    activate App
    
    Note over App, DBModule: Durant la creació de l'aplicació s'registra el mòdul DB
    App->>DBModule: init_app(app)
    activate DBModule
    DBModule->>App: Registra la comanda CLI 'init-db'<br/>(via app.cli.command)
    deactivate DBModule
    
    App-->>CLI: Retorna instància de l'aplicació configurada
    deactivate App

    CLI->>CLI: Cerca i localitza la comanda 'init-db' dins del context
    CLI->>App: Crea un Context d'Aplicació (Application Context)
    activate App

    CLI->>DBModule: Executa la funció associada: init_db_command()
    activate DBModule

    DBModule->>DBModule: Crida a init_db()
    
    DBModule->>App: Accedeix a current_app per llegir la ruta de la BD<br/>(app.config['DATABASE'])
    
    DBModule->>DBModule: Crida a get_db() per establir connexió
    DBModule->>SQLite: sqlite3.connect(database_path)
    activate SQLite
    SQLite-->>DBModule: Retorna l'objecte connexió (amb sqlite3.Row)
    deactivate SQLite

    DBModule->>App: open_resource('schema.sql')
    App-->>DBModule: Obre i llegeix el fitxer SQL d'esquema

    DBModule->>SQLite: db.executescript(schema_sql)
    activate SQLite
    Note over SQLite: Crea les taules 'user' i 'post'<br/>a la carpeta 'instance/'
    SQLite-->>DBModule: Execució completada (amb commit implícit)
    deactivate SQLite

    DBModule->>CLI: click.echo('Initialized the database.')
    deactivate DBModule

    CLI-->>Usuari: Mostra per pantalla: "Initialized the database."
    deactivate CLI
```