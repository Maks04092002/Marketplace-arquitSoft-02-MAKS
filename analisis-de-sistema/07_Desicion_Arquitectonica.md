# Decisiones Arquitectónicas (ADR)

Ahora que ya tenemos identificados los drivers arquitectónicos, podemos definir las decisiones arquitectónicas (ADR) que permitirán responder a las necesidades y requisitos del sistema[cite: 19].

**ADR (Architecture Decision Record)** significa Registro de Decisión Arquitectónica y documenta las decisiones importantes que tomamos durante el diseño de la arquitectura del software, junto con su justificación[cite: 19].

| ID | Decisión arquitectónica | Driver relacionado | Justificación | Resultado |
| :--- | :--- | :--- | :--- | :--- |
| **ADR-001** | Monolito modular | DA01 - Escalabilidad;<br>DA06 - Mantenibilidad[cite: 19] | Organizar las funcionalidades en módulos independientes dentro de una misma aplicación desplegable[cite: 19]. | Módulos de Catálogo, Carrito, Pedidos, Pagos y Usuarios[cite: 19]. |
| **ADR-002** | Clean Architecture | DA06 - Mantenibilidad[cite: 19] | Separar las reglas del negocio de los detalles tecnológicos[cite: 19]. | Dominio, Aplicación, Infraestructura y Presentación[cite: 19]. |
| **ADR-003** | Estrategia de caché | DA02 - Rendimiento[cite: 19] | Reducir consultas repetitivas a la fuente de datos[cite: 19]. | Caché para información de consulta frecuente[cite: 19]. |
| **ADR-004** | Integración de pagos mediante interfaces y adaptadores | DA04 - Integración con pagos[cite: 19] | Desacoplar los casos de uso del proveedor de pagos[cite: 19]. | Contrato de pagos y adaptador para la pasarela externa[cite: 19]. |