# Arquitectura Inicial del Sistema

## Diagrama de Arquitectura

```mermaid
flowchart TD
    %% --- ACTORES ---
    subgraph ACTORES ["🎭 ACTORES"]
        direction LR
        Cliente["👤 Cliente"]
        Seller["🏪 Seller"]
        Admin["⚙️ Administrador"]
    end

    %% --- CAPA DE PRESENTACIÓN ---
    subgraph PRESENTACION ["💻 CAPA DE PRESENTACIÓN"]
        Web["🌐 Aplicación Web & API REST"]
    end

    %% --- CAPA DE LÓGICA DE NEGOCIO ---
    subgraph NEGOCIO ["🧠 CAPA DE LÓGICA DE NEGOCIO"]
        direction LR
        Usuarios["👥 Usuarios"]
        Sellers["🏬 Sellers"]
        Catalogo["📦 Catálogo"]
        Carrito["🛒 Carrito"]
        Pedidos["📋 Pedidos"]
    end

    %% --- CAPA DE DATOS ---
    subgraph DATOS ["🗄️ CAPA DE DATOS"]
        BD[("🛢️ Base de Datos Principal")]
    end

    %% --- SISTEMAS EXTERNOS ---
    subgraph EXTERNOS ["🌐 SISTEMAS EXTERNOS"]
        direction LR
        Pago["💳 Pasarela de Pago"]
        ERP["📊 Sistema ERP"]
        Envio["🚚 Servicio de Envío"]
    end

    %% --- FLUJO PRINCIPAL ---
    ACTORES --> PRESENTACION
    PRESENTACION --> NEGOCIO
    NEGOCIO --> DATOS
    DATOS -. Integraciones .-> EXTERNOS

    %% --- ESTILOS DE COMPONENTES ---
    classDef actorStyle fill:#2D3748,stroke:#A0AEC0,color:#FFFFFF,stroke-width:2px;
    classDef presStyle fill:#1A365D,stroke:#3182CE,color:#FFFFFF,stroke-width:2px;
    classDef busStyle fill:#1C4532,stroke:#38A169,color:#FFFFFF,stroke-width:2px;
    classDef dataStyle fill:#744210,stroke:#D69E2E,color:#FFFFFF,stroke-width:2px;
    classDef extStyle fill:#4A5568,stroke:#CBD5E0,color:#FFFFFF,stroke-width:2px;

    class Cliente,Seller,Admin actorStyle;
    class Web presStyle;
    class Usuarios,Sellers,Catalogo,Carrito,Pedidos busStyle;
    class BD dataStyle;
    class Pago,ERP,Envio extStyle;
```

## Descripción
La arquitectura inicial se organiza en tres capas principales:
* **Presentación:** permite la interacción de los usuarios con el sistema mediante la aplicación web y la API REST.
* **Lógica de negocio:** contiene los principales módulos responsables de las funcionalidades del sistema: usuarios, sellers, catálogo, carrito y pedidos.
* **Datos:** permite almacenar y consultar la información mediante una base de datos.

Además, el módulo de **Pedidos** se integra con sistemas externos como la **pasarela de pago**, el **ERP** y el **servicio de envío**.