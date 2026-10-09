# PASO 3: Componentes Arquitectónicos del Marketplace GoPet

## 1. Visión General del Sistema
El backend del **Marketplace GoPet** está diseñado como un **Monolito Modular Organizado en Capas** ejecutado sobre **Node.js con Express**[cite: 14, 25, 26]. Este enfoque permite mantener todas las funcionalidades dentro de un mismo despliegue, pero aislando la lógica de cada dominio de negocio en módulos independientes con interfaces claras, facilitando la mantenibilidad, escalabilidad y testabilidad[cite: 16, 26].

---

## 2. Diagrama de Componentes (Modelo C4 - Nivel 3)

> **Ubicación de la imagen en el proyecto:** `docs/img/diagrama-componentes.png`[cite: 25, 26]

![Diagrama de Componentes Marketplace Backend](../img/diagrama-componentes.png)[cite: 25, 26]

### Representación en Mermaid (Renderizable en GitHub)

```mermaid
graph TB
    %% 1. ACTORES DEL SISTEMA
    subgraph Actores ["👥 Actores del Sistema"]
        direction LR
        C["📱 Cliente<br/>(Compra productos)"]
        S["🏪 Seller<br/>(Gestiona catálogo)"]
        A["🛡️ Administrador<br/>(Control global)"]
    end

    %% 2. CAPA DE PRESENTACIÓN
    subgraph Frontend ["🌐 Capa de Presentación"]
        WEB["💻 Aplicación Web<br/>(Next.js / React)"]
    end

    %% 3. CONTENEDOR PRINCIPAL BACKEND
    subgraph Backend ["📦 API Backend (Node.js / Express)"]
        API["🚀 Controlador REST / Enrutador<br/>(Express / Auth JWT)"]

        subgraph Modulos ["🧩 Módulos de Negocio"]
            USR["👤 Usuarios"]
            SLR["🏬 Sellers"]
            CAT["📦 Catálogo"]
            CAR["🛒 Carrito"]
            PED["📋 Pedidos"]
            PAG["💳 Pagos"]
            ENV["🚚 Envíos"]
            NOT["🔔 Notificaciones"]
        end
    end

    %% 4. CAPA DE ALMACENAMIENTO Y CACHÉ
    subgraph Almacenamiento ["💾 Persistencia y Caché"]
        direction LR
        CACHE[("⚡ Caché<br/>(Redis)")]
        DB[("🗄️ Base de Datos<br/>(PostgreSQL)")]
    end

    %% 5. SERVICIOS EXTERNOS
    subgraph Externos ["☁️ Sistemas Externos"]
        direction TB
        PAS["💳 Pasarela de Pagos<br/>(Culqi / Niubiz)"]
        ERP["🏢 ERP Empresarial"]
        LOG["🚛 Servicio de Envío"]
    end

    %% --- FLUJO Y CONEXIONES ---
    C & S & A -->|HTTPS / REST| WEB
    WEB -->|Peticiones HTTP/JSON| API

    %% Enrutamiento de la API a Módulos
    API --> USR
    API --> SLR
    API --> CAT
    API --> CAR
    API --> PED
    API --> PAG

    %% Interacciones de Caché
    CAT <-->|Consultas rápidas| CACHE
    CAR <-->|Estados temporales| CACHE

    %% Coordinación interna de Pedidos
    PED --> PAG
    PED --> ENV
    PED --> NOT

    %% Acceso a Base de Datos
    USR & SLR & CAT & CAR & PED & PAG & ENV -->|Lectura / Escritura| DB

    %% Integraciones Externas
    PAG -->|HTTPS / REST| PAS
    PED -->|HTTPS / REST| ERP
    ENV -->|HTTPS / REST| LOG

    %% --- ESTILOS VISUALES ---
    style Actores fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Frontend fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style Backend fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    style Almacenamiento fill:#fff3e0,stroke:#e65100,stroke-width:2px
    style Externos fill:#eceff1,stroke:#37474f,stroke-width:2px