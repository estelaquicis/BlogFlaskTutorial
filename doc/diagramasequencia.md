````mermaid
sequenceDiagram
    autonumber
    actor Usuari
    participant Navegador
    participant Auth as auth.py (Blueprint)
    participant DB as db.py (SQLite)
    participant Session as Sessió (Cookie)

    Usuari->>Navegador: Accedeix a /auth/login
    Navegador->>Auth: GET /auth/login
    activate Auth
    Auth-->>Navegador: Retorna login.html
    deactivate Auth
    Navegador-->>Usuari: Mostra el formulari

    Usuari->>Navegador: Introdueix username i password
    Navegador->>Auth: POST /auth/login
    activate Auth

    Auth->>DB: Consulta l'usuari pel username
    activate DB
    DB-->>Auth: Retorna user o None
    deactivate DB

    alt L'usuari no existeix
        Auth-->>Navegador: Error d'usuari incorrecte
    else Contrasenya incorrecta
        Auth->>Auth: check_password_hash()
        Auth-->>Navegador: Error de contrasenya
    else Autenticació correcta
        Auth->>Auth: Comprova el hash
        Auth->>Session: session.clear()
        Auth->>Session: Guarda user_id
        Auth-->>Navegador: Redirigeix a /
    end

    deactivate Auth
````