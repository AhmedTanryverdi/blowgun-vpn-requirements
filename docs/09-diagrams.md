# 9. Diagrams

Диаграммы ниже — черновые модели, предназначенные для обсуждения.

## 9.1 Контекст системы

```mermaid
flowchart LR
    U[Пользователь] --> APP[Мобильное приложение\nReact Native]
    APP <--> API[Backend API]
    APP <--> OS[Системный VPN API]
    OS <--> Xray[Xray tunnel]
    Xray <--> SERVER[VPN-сервер]
    API <--> DB[(База данных)]
    API <--> PAY[Платёжный провайдер / магазин приложений]
    API <--> MAIL[Сервис email]
```

## 9.2 Вход по email

```mermaid
sequenceDiagram
    actor User as Пользователь
    participant App as Mobile app
    participant API as Backend API
    participant Mail as Email service
    User->>App: Вводит email
    App->>API: POST /auth/start
    API->>API: Проверяет формат и rate limit
    API->>Mail: Отправляет OTP
    API-->>App: Подтверждение отправки
    User->>App: Вводит OTP
    App->>API: POST /auth/verify
    API->>API: Проверяет подтверждение
    API->>API: Создаёт или находит пользователя
    API-->>App: Сессия / access token
```

## 9.3 Подключение к VPN

```mermaid
stateDiagram-v2
    [*] --> Disconnected
    Disconnected --> RequestingConfig: Пользователь нажал Connect
    RequestingConfig --> Connecting: Конфигурация получена
    RequestingConfig --> Error: Ошибка API
    Connecting --> Connected: Туннель подтверждён
    Connecting --> Error: Ошибка native / системного разрешения
    Error --> RequestingConfig: Повторить
    Connected --> Disconnecting: Пользователь нажал Disconnect
    Disconnecting --> Disconnected: Туннель остановлен
    Disconnecting --> Error: Ошибка отключения
```

## 9.4 Примечания

- Схемы не фиксируют окончательный способ выдачи конфигурации и хранения ключей.
- Точный жизненный цикл туннеля зависит от платформы и должен быть проверен прототипом.
- Платёжный провайдер и требования магазинов приложений не выбраны.
