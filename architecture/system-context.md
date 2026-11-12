flowchart TB
    User["Пользователь"]

    subgraph Blowgun["blowgun.vpn"]
        App["Приложение"]
        Backend["Backend API"]
    end

    VPN["VPN-серверы<br/>AmneziaWG / XRay"]
    Payment["Платёжный провайдер"]

    User <-->|Использование приложения| App
    App <-->|HTTPS / REST API| Backend
    Backend <-->|Управление серверами<br/>Получение конфигураций| VPN
    Backend <-->|Обработка платежей| Payment
    App <-->|VPN-туннель| VPN

    style Blowgun fill:#e8f1ff,stroke:#2563eb,stroke-width:2px