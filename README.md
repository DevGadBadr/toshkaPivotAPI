# toshkaPivotAPI
sequenceDiagram
    participant Valmont as Valmont Server
    participant Toshka as Toshka Server

    Valmont->>Toshka: POST /generateToken (username + password)
    Toshka-->>Valmont: Returns token

    Valmont->>Toshka: POST /updateTags (username + password + token + tags)
    Toshka-->>Valmont: Success message
