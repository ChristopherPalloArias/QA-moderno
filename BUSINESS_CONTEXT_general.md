# Plantilla de Contexto de Negocio — General (Gema A + Gema B)

> **Propósito:** Este documento es la plantilla de contexto de negocio completa y unificada que sirve como entrada para ambas Gemas: la Gema A (Diagnóstico INVEST de Historias de Usuario) y la Gema B (Generación de Casos de Prueba en Gherkin). Contiene tanto la información de negocio como las especificaciones tecnicas necesarias para testing.

---

## 1. Descripción del Proyecto

- **Nombre del Proyecto:** Budget Management App (Aplicacion de Gestion de Presupuestos)
- **Objetivo del Proyecto:** Proporcionar a los usuarios una herramienta web para gestionar sus finanzas personales, permitiendo el registro, categorización y seguimiento de ingresos y gastos, con el fin de facilitar el control presupuestario y la toma de decisiones financieras informadas.

## 2. Flujos Críticos del Negocio

- **Principales Flujos de Trabajo:**
  - Registro e inicio de sesión de usuarios mediante autenticación JWT.
  - Gestión de categorías personalizadas (CRUD) para clasificar transacciones financieras (ej: Alimentación, Transporte, Salario, Freelance).
  - Registro de transacciones financieras (ingresos y gastos) indicando monto, descripcion, fecha, tipo (ingreso/gasto) y categoria asociada.
  - Visualización de resumen financiero: balance general, total de ingresos, total de gastos y saldo disponible.
  - Filtrado y búsqueda de transacciones por fecha, categoría o tipo.

- **Modulos o Funcionalidades Criticas:**
  - Modulo de autenticacion y autorizacion (registro, login, JWT)
  - Modulo de gestion de categorias (CRUD completo)
  - Modulo de gestion de transacciones/movimientos (CRUD completo)
  - Modulo de resumen/dashboard financiero
  - Modulo de perfil de usuario

## 3. Reglas de Negocio y Restricciones

- **Reglas de Negocio Relevantes:**
  - Cada usuario solo puede ver y gestionar sus propias categorias y transacciones (aislamiento de datos por usuario).
  - Las transacciones deben estar asociadas obligatoriamente a una categoria existente del usuario.
  - Los montos de las transacciones deben ser valores numericos positivos mayores a cero.
  - No se permite eliminar una categoria que tenga transacciones asociadas.
  - El tipo de transaccion solo puede ser "ingreso" o "gasto".
  - Los campos obligatorios para una transaccion son: monto, descripcion, fecha, tipo y categoria.
  - Las contrasenas deben almacenarse de forma encriptada (hash con bcrypt).
  - Los tokens JWT tienen un tiempo de expiracion definido.

- **Regulaciones o Normativas:**
  - Proteccion de datos personales del usuario (contrasenas hasheadas, datos no compartidos entre usuarios).
  - Validaciones de entrada para prevenir inyeccion SQL y ataques XSS.
  - Manejo seguro de sesiones mediante tokens JWT.

- **Especificaciones Tecnicas para Testing (especialmente relevante para Gema B):**
  - Campo email: formato valido (usuario@dominio.com), maximo 255 caracteres, debe ser unico en el sistema.
  - Campo contrasena: minimo 6 caracteres (segun configuracion), se almacena hasheada con bcrypt.
  - Campo nombre de categoria: tipo texto, obligatorio, no debe estar vacio ni contener solo espacios.
  - Campo monto (transaccion): tipo numerico decimal, debe ser > 0, acepta hasta 2 decimales (ej: 150.50).
  - Campo descripcion (transaccion): tipo texto, obligatorio, longitud maxima segun configuracion de base de datos (VARCHAR 255 tipico).
  - Campo fecha (transaccion): formato ISO 8601 (YYYY-MM-DD), obligatorio.
  - Campo tipo (transaccion): solo acepta dos valores: "ingreso" o "gasto".
  - Campo categoria (transaccion): referencia a ID de categoria existente del usuario autenticado.

- **Endpoints principales de la API REST (especialmente relevante para Gema B):**
  - POST /api/auth/register — Registro de usuario
  - POST /api/auth/login — Inicio de sesion (retorna JWT)
  - GET/POST/PUT/DELETE /api/categories — CRUD de categorias
  - GET/POST/PUT/DELETE /api/transactions — CRUD de transacciones
  - GET /api/transactions/summary — Resumen financiero

## 4. Perfiles de Usuario y Roles

- **Perfiles o Roles de Usuario en el Sistema:**
  - Usuario registrado: Persona que se ha registrado en la plataforma y puede acceder a todas las funcionalidades de gestion financiera personal.

- **Permisos y Limitaciones de Cada Perfil:**
  - El usuario registrado puede: registrarse, iniciar sesion, crear/editar/eliminar sus categorias, crear/editar/eliminar sus transacciones, visualizar su resumen financiero y gestionar su perfil.
  - El usuario NO puede: acceder a datos de otros usuarios, realizar operaciones sin estar autenticado, ni crear transacciones sin categoria asociada.

## 5. Condiciones del Entorno Tecnico

- **Plataformas Soportadas:** Aplicacion web responsive accesible desde navegadores modernos (Chrome, Firefox, Safari, Edge) en dispositivos desktop y moviles.

- **Tecnologias o Integraciones Clave:**
  - Backend: Node.js con Express.js
  - Base de Datos: PostgreSQL con Sequelize ORM
  - Autenticacion: JSON Web Tokens (JWT) con bcrypt para hash de contrasenas
  - Frontend: Segun implementacion del cliente (React u otra tecnologia SPA)
  - Validaciones: Express-validator para validacion de datos de entrada
  - Arquitectura: API RESTful con endpoints organizados por recurso
  - Despliegue: Servidor Node.js (hosting tipo cPanel, VPS o cloud)

## 6. Casos Especiales o Excepciones

- Si un usuario intenta acceder a un recurso sin token JWT valido, el sistema debe retornar un error 401 (No autorizado).
- Si un usuario intenta acceder a un recurso de otro usuario, el sistema debe retornar un error 403 (Prohibido).
- Si se intenta registrar un usuario con un email ya existente, el sistema debe retornar un error indicando duplicidad.
- Si se intenta crear una transaccion con una categoria inexistente o que no pertenece al usuario, el sistema debe rechazar la operacion.
- El sistema debe manejar correctamente valores decimales en los montos (ej: 150.50).
- Las fechas deben manejarse en formato consistente (ISO 8601 recomendado).
- Intentar registrar una transaccion con monto 0, negativo o no numerico debe ser rechazado.
- Intentar registrar una transaccion con tipo diferente a "ingreso" o "gasto" debe ser rechazado.
- Intentar operaciones CRUD sin header Authorization: Bearer <token> debe retornar 401.
- Strings vacios o solo espacios en campos obligatorios deben ser rechazados.

---

> **Uso para Gema A (Diagnostico INVEST):** Copiar secciones 1-6. La Gema A se enfocara naturalmente en el analisis de negocio (claridad, INVEST, coherencia, refinamiento).
>
> **Uso para Gema B (Casos de Prueba):** Copiar secciones 1-6 completas. La Gema B aprovechara las especificaciones tecnicas, limites de campos y endpoints para generar casos mas completos.
>
> **Nota:** Este documento general se puede usar tal cual para ambas Gemas sin modificaciones. Cada Gema usara la informacion relevante a su funcion.
