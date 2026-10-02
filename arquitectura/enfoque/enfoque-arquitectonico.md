# Enfoque Arquitectónico: Clean Architecture

## 1. Definición del Enfoque

| Elemento | Descripción Aplicada al Marketplace |
| :--- | :--- |
| **Patrón / Enfoque Arquitectónico** | Clean Architecture (Arquitectura Limpia)[cite: 8]. |
| **Objetivo** | Separar responsabilidades y controlar que todas las dependencias internas apunten siempre hacia el dominio (hacia adentro)[cite: 4, 8]. |
| **¿Qué problema resuelve?** | Evita el acoplamiento entre la interfaz de usuario en Angular, las reglas del negocio y las tecnologías externas (bases de datos, APIs y servicios de pago)[cite: 4, 8]. |
| **Capas Definidas** | Presentación, Aplicación, Dominio e Infraestructura[cite: 4, 8]. |
| **Beneficios Clave** | • Facilita el mantenimiento y la ejecución de pruebas unitarias[cite: 8].<br>• Permite cambiar implementaciones técnicas sin modificar las reglas del negocio[cite: 4, 8].<br>• Mejora la organización y la separación de responsabilidades en el código[cite: 8]. |

---

## 2. Organización de Responsabilidades por Capa

1. **Capa de Dominio (Domain - Núcleo):**
   * Contiene las entidades principales del Marketplace (`Producto`, `Carrito`, `Pedido`) y los contratos/interfaces de repositorios y servicios (`RepositorioProducto`, `ProcesadorPagos`, `NotificadorCliente`). Es completamente independiente de frameworks y librerías externas[cite: 4].
2. **Capa de Aplicación (Application Layer):**
   * Contiene los casos de uso del sistema que orquestan el comportamiento del negocio (`ConsultarCatalogoUseCase`, `AgregarAlCarritoUseCase`, `ProcesarCompraUseCase`).
3. **Capa de Presentación (Presentation):**
   * Contiene los componentes visuales y la interfaz del usuario en Angular (`CatalogoComponent`, `CarritoComponent`, `AppComponent`).
4. **Capa de Infraestructura (Infrastructure):**
   * Implementa las interfaces/contratos del dominio mediante conectores concretos (`HttpProductoRepositoryImpl`, `HttpPedidoRepositoryImpl`, `ProcesadorPagosSimulado`, `NotificadorEmailModal`, `app.config.ts`).

---

## 📐 3. Diagrama de Clean Architecture

```mermaid
flowchart TD

subgraph EXTERNO ["🌐 SERVICIOS EXTERNOS"]
    API["Backend Marketplace API REST<br/>(Node.js / Express / PostgreSQL)"]
end

subgraph APP ["📱 APLICACIÓN MARKETPLACE WEB (Angular 18 / TypeScript)"]

    subgraph INFRA ["🟣 INFRAESTRUCTURA (Adaptadores e Implementaciones)"]
        HttpProd["HttpProductoRepositoryImpl"]
        HttpPed["HttpPedidoRepositoryImpl"]
        PagosSim["ProcesadorPagosSimulado"]
        NotifEmail["NotificadorEmailModal"]
        AppCfg["app.config.ts"]
    end

    subgraph PRES ["🔵 PRESENTACIÓN (UI / Componentes)"]
        CatComp["CatalogoComponent"]
        CarComp["CarritoComponent"]
        AppComp["AppComponent"]
    end

    subgraph APP_LAYER ["🟢 APLICACIÓN (Casos de Uso)"]
        UC1["ConsultarCatalogoUseCase"]
        UC2["AgregarAlCarritoUseCase"]
        UC3["ProcesarCompraUseCase"]
    end

    subgraph DOMAIN ["🟡 DOMINIO (Entidades y Contratos / Núcleo)"]
        subgraph ENTIDADES ["Entidades de Negocio"]
            Prod["Producto"]
            Carr["Carrito"]
            Ped["Pedido"]
        end
        subgraph CONTRATOS ["Interfaces / Puertos"]
            IRepoProd["RepositorioProducto"]
            IRepoPed["RepositorioPedidos"]
            IProcPagos["ProcesadorPagos"]
            INotif["NotificadorCliente"]
        end
    end

end

PRES --> APP_LAYER
INFRA --> APP_LAYER

APP_LAYER --> DOMAIN

INFRA -- "Implementa" --> CONTRATOS
HttpProd -- "HTTPS / REST" --> API
HttpPed -- "HTTPS / REST" --> API
PagosSim -- "HTTPS / REST" --> API

classDef domainStyle fill:#FEFCBF,stroke:#D69E2E,color:#744210,stroke-width:2px;
classDef appStyle fill:#C6F6D5,stroke:#38A169,color:#1C4532,stroke-width:2px;
classDef presStyle fill:#EBF8FF,stroke:#3182CE,color:#1A365D,stroke-width:2px;
classDef infraStyle fill:#E9D8FD,stroke:#805AD5,color:#44337A,stroke-width:2px;
classDef extStyle fill:#EDF2F7,stroke:#718096,color:#2D3748,stroke-width:2px;

class DOMAIN,ENTIDADES,CONTRATOS,Prod,Carr,Ped,IRepoProd,IRepoPed,IProcPagos,INotif domainStyle;
class APP_LAYER,UC1,UC2,UC3 appStyle;
class PRES,CatComp,CarComp,AppComp presStyle;
class INFRA,HttpProd,HttpPed,PagosSim,NotifEmail,AppCfg infraStyle;
class API extStyle;