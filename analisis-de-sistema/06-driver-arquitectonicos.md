# Drivers Arquitectónicos

| ID | Driver Arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
| :--- | :--- | :--- | :--- |
| **DA01** | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales. | AC03 – Escalabilidad | Puede influir en la estrategia de escalamiento y despliegue del servidor. |
| **DA02** | El sistema debe mantener tiempos de respuesta adecuados durante una alta concurrencia. | AC01 – Rendimiento | Influye en la comunicación entre componentes, procesamiento y optimización de base de datos. |
| **DA03** | El sistema debe proteger los datos de usuarios y operaciones de compra. | AC04 – Seguridad | Influye en la autenticación, autorización y encriptación de transferencias de datos. |
| **DA04** | El sistema debe integrarse con una pasarela de pago externa mediante una API. | RC04 – Pasarela de pago | Condiciona la forma de comunicación e integración asíncrona o sincrónica con servicios externos. |
| **DA05** | El sistema debe utilizar una API REST para la comunicación entre frontend y backend. | RC03 – API REST | Limita las alternativas de comunicación, imponiendo una separación desacoplada entre cliente y servidor. |