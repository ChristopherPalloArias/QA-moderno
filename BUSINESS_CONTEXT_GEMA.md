# Plantilla de Contexto de Negocio — Gema A (Diagnóstico INVEST)

> **Propósito:** Este documento sirve como entrada de contexto para la Gema A de Diagnóstico INVEST de Historias de Usuario. Debe ser proporcionado junto con la historia de usuario a analizar.

---

## 1. Descripción del Proyecto

- **Nombre del Proyecto:** Budget Management App (Aplicación de Gestión de Presupuestos)
- **Objetivo del Proyecto:** Proporcionar a los usuarios una herramienta web para gestionar sus finanzas personales, permitiendo el registro, categorización y seguimiento de ingresos y gastos, con el fin de facilitar el control presupuestario y la toma de decisiones financieras informadas.

## 2. Flujos Críticos del Negocio

- **Principales Flujos de Trabajo:**
  - Registro e inicio de sesión de usuarios mediante autenticación JWT.
  - Gestión de categorías personalizadas (CRUD) para clasificar transacciones financieras (ej: Alimentación, Transporte, Salario, Freelance).
  - Registro de transacciones financieras (ingresos y gastos) indicando monto, descripción, fecha, tipo (ingreso/gasto) y categoría asociada.
  - Visualización de resumen financiero: balance general, total de ingresos, total de gastos y saldo disponible.
  - Filtrado y búsqueda de transacciones por fecha, categoría o tipo.

- **Módulos o Funcionalidades Críticas:**
  - Módulo de autenticación y autorización (registro, login, JWT)
  - Módulo de gestión de categorías (CRUD completo)
  - Módulo de gestión de transacciones/movimientos (CRUD completo)
  - Módulo de resumen/dashboard financiero
  - Módulo de perfil de usuario

## 3. Reglas de Negocio y Restricciones

- **Reglas de Negocio Relevantes:**
  - Cada usuario solo puede ver y gestionar sus propias categorías y transacciones (aislamiento de datos por usuario).
  - Las transacciones deben estar asociadas obligatoriamente a una categoría existente del usuario.
  - Los montos de las transacciones deben ser valores numéricos positivos mayores a cero.
  - No se permite eliminar una categoría que tenga transacciones asociadas.
  - El tipo de transacción solo puede ser "ingreso" o "gasto".
  - Los campos obligatorios para una transacción son: monto, descripción, fecha, tipo y categoría.
  - Las contraseñas deben almacenarse de forma encriptada (hash con bcrypt).
  - Los tokens JWT tienen un tiempo de expiración definido.

- **Regulaciones o Normativas:**
  - Protección de datos personales del usuario (contraseñas hasheadas, datos no compartidos entre usuarios).
  - Validaciones de entrada para prevenir inyección SQL y ataques XSS.
  - Manejo seguro de sesiones mediante tokens JWT.

## 4. Perfiles de Usuario y Roles

- **Perfiles o Roles de Usuario en el Sistema:**
  - Usuario registrado: Persona que se ha registrado en la plataforma y puede acceder a todas las funcionalidades de gestión financiera personal.

- **Permisos y Limitaciones de Cada Perfil:**
  - El usuario registrado puede: registrarse, iniciar sesión, crear/editar/eliminar sus categorías, crear/editar/eliminar sus transacciones, visualizar su resumen financiero y gestionar su perfil.
  - El usuario NO puede: acceder a datos de otros usuarios, realizar operaciones sin estar autenticado, ni crear transacciones sin categoría asociada.

## 5. Condiciones del Entorno Técnico

- **Plataformas Soportadas:** Aplicación web responsive accesible desde navegadores modernos (Chrome, Firefox, Safari, Edge) en dispositivos desktop y móviles.

- **Tecnologías o Integraciones Clave:**
  - Backend: Node.js con Express.js
  - Base de Datos: PostgreSQL con Sequelize ORM
  - Autenticación: JSON Web Tokens (JWT) con bcrypt para hash de contraseñas
  - Frontend: Según implementación del cliente (React u otra tecnología SPA)
  - Validaciones: Express-validator para validación de datos de entrada
  - Arquitectura: API RESTful con endpoints organizados por recurso
  - Despliegue: Servidor Node.js (hosting tipo cPanel, VPS o cloud)

## 6. Casos Especiales o Excepciones

- Si un usuario intenta acceder a un recurso sin token JWT válido, el sistema debe retornar un error 401 (No autorizado).
- Si un usuario intenta acceder a un recurso de otro usuario, el sistema debe retornar un error 403 (Prohibido).
- Si se intenta registrar un usuario con un email ya existente, el sistema debe retornar un error indicando duplicidad.
- Si se intenta crear una transacción con una categoría inexistente o que no pertenece al usuario, el sistema debe rechazar la operación.
- El sistema debe manejar correctamente valores decimales en los montos (ej: 150.50).
- Las fechas deben manejarse en formato consistente (ISO 8601 recomendado).

---

> **Uso:** Copiar todo el contenido desde la sección 1 hasta la sección 6 y pegarlo como {PARAM_CONTEXTO} al momento de interactuar con la Gema A para diagnosticar una historia de usuario.
