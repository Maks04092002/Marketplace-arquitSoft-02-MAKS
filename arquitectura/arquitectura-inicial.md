# Arquitectura Inicial del Sistema

## Diagrama de Arquitectura en Capas

```mermaid
flowchart TD
%% =========================
%% ACTORES
%% =========================
subgraph ACTORES ["ACTORES"]
Cliente ["Cliente"]
Seller ["Seller"]
Admin ["Administrador"]
end

%% =========================
%% PRESENTACIÓN
%% =========================
subgraph PRESENTACION ["PRESENTACIÓN"]
Web ["Aplicación Web → API REST"]
end

%% =========================
%% LÓGICA DE NEGOCIO
%% =========================
subgraph NEGOCIO ["LÓGICA DE NEGOCIO"]
Usuarios ["Usuarios"]
Sellers ["Sellers"]
Catalogo ["Catálogo"]
Carrito ["Carrito"]
Pedidos ["Pedidos"]
end

%% =========================
%% DATOS
%% =========================
subgraph DATOS ["DATOS"]
BD ["Base de datos"]
end

%% =========================
%% SISTEMAS EXTERNOS
%% =========================
subgraph EXTERNOS ["SISTEMAS EXTERNOS"]
Pago ["Pasarela de pago"]
ERP ["ERP"]
Envio ["Servicio de envío"]
end

%% =========================
%% FLUJO Y RELACIONES
%% =========================
ACTORES --> PRESENTACION
PRESENTACION --> NEGOCIO
NEGOCIO --> DATOS
DATOS -->|"integraciones"| EXTERNOS

%% ALINEACIÓN VISUAL HORIZONTAL
Cliente ~~~ Seller
Seller ~~~ Admin
Usuarios ~~~ Sellers
Sellers ~~~ Catalogo
Catalogo ~~~ Carrito
Carrito ~~~ Pedidos
Pago ~~~ ERP
ERP ~~~ Envio

%% ESTILOS VISUALES
style ACTORES fill:#222,stroke:#fff,stroke-width:2px,color:#fff
style PRESENTACION fill:#222,stroke:#fff,stroke-width:2px,color:#fff
style NEGOCIO fill:#222,stroke:#fff,stroke-width:2px,color:#fff
style DATOS fill:#222,stroke:#fff,stroke-width:2px,color:#fff
style EXTERNOS fill:#222,stroke:#fff,stroke-width:2px,color:#fff
style Cliente fill:#222,stroke:#fff,color:#fff
style Seller fill:#222,stroke:#fff,color:#fff
style Admin fill:#222,stroke:#fff,color:#fff
style Web fill:#222,stroke:#fff,color:#fff
style Usuarios fill:#222,stroke:#fff,color:#fff
style Sellers fill:#222,stroke:#fff,color:#fff
style Catalogo fill:#222,stroke:#fff,color:#fff
style Carrito fill:#222,stroke:#fff,color:#fff
style Pedidos fill:#222,stroke:#fff,color:#fff
style BD fill:#222,stroke:#fff,color:#fff
style Pago fill:#222,stroke:#fff,color:#fff
style ERP fill:#222,stroke:#fff,color:#fff
style Envio fill:#222,stroke:#fff,color:#fff
```

## Descripción de la Arquitectura

La solución propuesta para el **Marketplace de productos para mascotas** se estructura en un modelo tradicional de **tres capas** suplementado por un bloque de **sistemas externos**:

1. **Capa de Presentación:**
   - Interfaz gráfica con la que interactúan los actores (Cliente, Seller, Administrador)[cite: 2].
   - Expone y consume servicios mediante una **API REST** desacoplada[cite: 2].

2. **Capa de Lógica de Negocio:**
   - Contiene las reglas del dominio distribuidas en los módulos principales: **Usuarios**, **Sellers**, **Catálogo**, **Carrito** y **Pedidos**[cite: 2].

3. **Capa de Datos:**
   - Gestiona el almacenamiento persistente mediante la **Base de Datos** principal[cite: 2].

4. **Sistemas Externos:**
   - Integración del módulo de Pedidos/Datos con la **Pasarela de Pago** (procesamiento de transacciones), **ERP** (inventario/facturación) y **Servicio de Envío** (logística de entrega)[cite: 2].