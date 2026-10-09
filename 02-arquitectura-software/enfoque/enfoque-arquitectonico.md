# Enfoque Arquitectónico: Clean Architecture

## 1. Definición del Enfoque

| Elemento | Descripción Aplicada al Marketplace |
| :--- | :--- |
| **Patrón / Enfoque Arquitectónico** | Clean Architecture (Arquitectura Limpia)[cite: 4, 8]. |
| **Objetivo** | Separar responsabilidades y controlar que todas las dependencias internas apunten siempre hacia el dominio (hacia adentro)[cite: 4, 8]. |
| **¿Qué problema resuelve?** | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas (bases de datos, APIs REST y servicios de pago/mensajería)[cite: 4, 8]. |
| **Capas Definidas** | Presentación, Aplicación, Dominio e Infraestructura[cite: 4, 8]. |
| **Beneficios Clave** | • Facilita el mantenimiento y la ejecución de pruebas unitarias en TypeScript puro[cite: 8].<br>• Permite intercambiar tecnologías (p. ej., de memoria a API REST) sin afectar el núcleo del negocio[cite: 4, 8].<br>• Garantiza la independencia de frameworks en la capa de Dominio[cite: 4, 8]. |

---

## 2. Diagrama de Enfoque Arquitectónico (Clean Architecture)

```mermaid
flowchart TD

subgraph MARKETPLACE ["«aplicación» Marketplace Web [Angular 18 - TypeScript] src/app/"]
    
    subgraph FRAMEWORKS ["ADAPTADORES Y FRAMEWORKS — dependen de Angular, HttpClient, RxJS"]

        subgraph PRES ["PRESENTACIÓN<br/>src/app/presentacion/"]
            CatComp["«componente»<br/><b>CatalogComponent</b><br/>lista y filtra productos"]
            EstCarr["«servicio de estado»<br/><b>EstadoCarrito</b><br/>signals · sin reglas"]
            CarComp["«componente»<br/><b>CarritoComponent</b><br/>resumen y confirmar compra"]
            AppComp["«componente»<br/><b>AppComponent</b><br/>shell de la aplicación"]
        end

        subgraph APLICACION ["APLICACIÓN — casos de uso - src/app/aplicacion/"]
            
            subgraph CASOS_USO ["Casos de Uso"]
                UC1["«caso de uso»<br/><b>ConsultarCatalogoCasoUso</b><br/>ejecutar()"]
                UC2["«caso de uso»<br/><b>AgregarAlCarritoCasoUso</b><br/>ejecutar()"]
                UC3["«caso de uso»<br/><b>RegistrarCompraCasoUso</b><br/>ejecutar()"]
            end

            subgraph DOMINIO ["DOMINIO — núcleo - src/app/dominio/"]
                
                subgraph MODELOS ["Modelos (entidades y reglas)"]
                    EntProd["«entidad»<br/><b>Producto</b><br/>stock, categoría, precio"]
                    EntCarr["«entidad»<br/><b>Carrito</b><br/>inmutable · subtotal, total"]
                    EntPed["«entidad»<br/><b>Pedido</b><br/>estados · cancelación"]
                    RegPrecios["«reglas»<br/><b>precios.ts</b><br/>comisión 10% · IGV 18%"]
                end

                subgraph CONTRATOS ["Contratos (puertos)"]
                    IRepoProd["«interface»<br/><b>RepositorioProductos</b>"]
                    IRepoPed["«interface»<br/><b>RepositorioPedidos</b>"]
                    IProcPagos["«interface»<br/><b>ProcesadorPagos</b>"]
                    INotif["«interface»<br/><b>NotificadorCliente</b>"]
                end
            end
        end

        subgraph INFRA ["INFRAESTRUCTURA<br/>src/app/infraestructura/"]
            AdaptProd["«adaptador»<br/><b>RepositorioProductosMemoria</b><br/><b>RepositorioProductosHttp</b>"]
            AdaptPed["«adaptador»<br/><b>RepositorioPedidosMemoria</b>"]
            AdaptPagos["«adaptador»<br/><b>ProcesadorPagosSimulado</b><br/><b>ProcesadorPagosNiubiz</b>"]
            AdaptNotif["«adaptador»<br/><b>NotificadorConsola</b><br/><b>NotificadorWhatsApp</b>"]
            Tokens["«Angular DI»<br/><b>tokens.ts</b><br/>InjectionToken por contrato"]
        end

        AppConfig["«raíz de composición»<br/><b>app.config.ts</b><br/>único archivo que elige qué adaptador cumple cada contrato (useFactory + InjectionToken) y lo inyecta en los casos de uso"]

    end
end

subgraph CLIENTE ["Cliente"]
    Usuario["👤 Usuario (Cliente)"]
    Nav["💻 Navegador"]
end

subgraph EXTERNO ["«sistema externo»<br/>Marketplace API REST<br/>Backend Node.js - monolito modular"]
    API_ENDPOINTS["/api/productos<br/>/api/pedidos<br/>/api/authorization<br/>/api/mensajes<br/><br/><i>Se integra con Niubiz y WhatsApp;<br/>las credenciales viven solo aquí.</i>"]
end

Usuario --> Nav
Nav --> PRES

PRES -- "invoca" --> CASOS_USO
EstCarr -.-> EntCarr

AdaptProd -. "implementa" .-> IRepoProd
AdaptPed -. "implementa" .-> IRepoPed
AdaptPagos -. "implementa" .-> IProcPagos
AdaptNotif -. "implementa" .-> INotif

AppConfig -. "registra" .-> Tokens

INFRA -- "HTTP / JSON" --> EXTERNO

classDef presStyle fill:#EBF8FF,stroke:#3182CE,color:#1A365D,stroke-width:1px;
classDef appStyle fill:#E6F4EA,stroke:#34A853,color:#137333,stroke-width:1px;
classDef domStyle fill:#FEF7E0,stroke:#FBBC04,color:#B06000,stroke-width:1px;
classDef infraStyle fill:#F3E8FD,stroke:#A142F4,color:#681DA8,stroke-width:1px;
classDef extStyle fill:#F1F3F4,stroke:#5F6368,color:#202124,stroke-width:1px;

class PRES,CatComp,EstCarr,CarComp,AppComp presStyle;
class APLICACION,CASOS_USO,UC1,UC2,UC3 appStyle;
class DOMINIO,MODELOS,CONTRATOS,EntProd,EntCarr,EntPed,RegPrecios,IRepoProd,IRepoPed,IProcPagos,INotif domStyle;
class INFRA,AdaptProd,AdaptPed,AdaptPagos,AdaptNotif,Tokens infraStyle;
class EXTERNO,API_ENDPOINTS extStyle;