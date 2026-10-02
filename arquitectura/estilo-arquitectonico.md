# Estilo Arquitectónico del Sistema

## 1. Selección del Estilo Arquitectónico

Para el sistema Marketplace de productos para mascotas, se ha seleccionado el estilo **Monolito Modular en Capas**[cite: 3, 8].

* **Monolito:** Es una única unidad de despliegue principal ejecutada sobre Node.js y Express[cite: 3, 8].
* **Modular:** El código está internamente dividido en módulos independientes por dominio de negocio (*Usuarios, Sellers, Catálogo, Carrito, Pedidos*)[cite: 7, 8].
* **En Capas:** Cada módulo organiza sus responsabilidades de forma horizontal (*Presentación, Lógica de Negocio y Datos*) para controlar el acoplamiento.

---

## 2. Justificación de la Decisión

1. **Simplicidad de Despliegue y Operación:** Permite centrar los esfuerzos en la calidad de la estructura interna del código sin la complejidad operativa de infraestructura distribuida[cite: 3].
2. **Respuesta a los Drivers Arquitectónicos:**
   * **DA01 (Escalabilidad):** Permite escalar horizontalmente mediante balanceo de carga de la instancia monolítica[cite: 6, 8].
   * **DA05 (API REST):** Separa limpiamente la interfaz de cliente web (Angular) del backend mediante HTTP/JSON[cite: 6, 8].
   * **DA06 (Mantenibilidad):** La modularidad evita que cambios en un módulo (p. ej. Carrito) rompan otros módulos (p. ej. Usuarios)[cite: 6, 8].

---

## 📐 3. Diagrama de Arquitectura Global

```mermaid
flowchart TD
    subgraph ACTORES ["👥 ACTORES"]
        Cliente["👤 Cliente"]
        Seller["🏪 Seller"]
        Admin["👨‍💼 Administrador"]
    end

    subgraph FRONTEND ["💻 CAPA CLIENTE"]
        Web["🌐 Cliente Web (Angular / HTML-CSS-JS)"]
    end

    ACTORES --> Web

    subgraph MONOLITO ["📦 MONOLITO MARKETPLACE BACKEND (Node.js 20 LTS + Express)"]
        
        subgraph MW ["🛡️ Middlewares Express Universales"]
            CORS["CORS"]
            AUTH["Auth (JWT)"]
            VAL["Validación de Entrada"]
            ERR["Manejo de Errores / Logger"]
        end

        subgraph CAPA_PRES ["1. CAPA DE PRESENTACIÓN (Rutas / Controllers)"]
            M_User_C["Módulo Usuarios<br/>(usuarios.routes / controller)"]
            M_Sell_C["Módulo Sellers<br/>(sellers.routes / controller)"]
            M_Cat_C["Módulo Catálogo<br/>(catalogo.routes / controller)"]
            M_Car_C["Módulo Carrito<br/>(carrito.routes / controller)"]
            M_Ped_C["Módulo Pedidos<br/>(pedidos.routes / controller)"]
        end

        subgraph CAPA_LOG ["2. CAPA DE LÓGICA DE NEGOCIO (Services)"]
            M_User_S["usuarios.service.js"]
            M_Sell_S["sellers.service.js"]
            M_Cat_S["catalogo.service.js"]
            M_Car_S["carrito.service.js"]
            M_Ped_S["pedidos.service.js"]
        end

        subgraph CAPA_DATOS ["3. CAPA DE DATOS (Repositories & ORM)"]
            M_User_R["usuarios.repository.js"]
            M_Sell_R["sellers.repository.js"]
            M_Cat_R["catalogo.repository.js"]
            M_Car_R["carrito.repository.js"]
            M_Ped_R["pedidos.repository.js"]
            ORM["Acceso a Datos Compartido<br/>(Sequelize ORM / Pool)"]
        end
    end

    subgraph DB ["🗄️️ PERSISTENCIA"]
        PostgreSQL[("PostgreSQL<br/>marketplace_db")]
    end

    subgraph EXTERNOS ["🌐 SISTEMAS EXTERNOS"]
        Pasarela["💳 Pasarela de Pagos (API REST)"]
        Envios["🚚 Servicio de Envíos (API REST)"]
    end

    Web -- "HTTPS / REST (JSON)" --> MW
    MW --> CAPA_PRES

    M_User_C --> M_User_S
    M_Sell_C --> M_Sell_S
    M_Cat_C --> M_Cat_S
    M_Car_C --> M_Car_S
    M_Ped_C --> M_Ped_S

    M_User_S --> M_User_R
    M_Sell_S --> M_Sell_R
    M_Cat_S --> M_Cat_R
    M_Car_S --> M_Car_R
    M_Ped_S --> M_Ped_R

    M_Ped_S -- "HTTPS / REST" --> Pasarela
    M_Ped_S -- "HTTPS / REST" --> Envios

    M_User_R --> ORM
    M_Sell_R --> ORM
    M_Cat_R --> ORM
    M_Car_R --> ORM
    M_Ped_R --> ORM

    ORM -- "SQL / TCP 5432" --> PostgreSQL