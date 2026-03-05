# 📋 Refinamiento de Historias de Usuario — Comparativa

> **Proyecto:** Budget Management App (SofkianOS MVP)  
> **Fecha:** 2026-03-03  
> **Propósito:** Documentar las diferencias entre las HU originales y las versiones refinadas por la gema, identificando cambios en alcance, criterios de aceptación y reglas de negocio.

---

## 🗑️ US-017 — Eliminar un Reporte Financiero

### 📄 HU Original

**Título:** Eliminar un Reporte Financiero de un Período

> Como **Usuario Registrado**,  
> quiero **eliminar un reporte financiero de un período mensual específico**,  
> para **mantener mi historial de reportes limpio y libre de información que ya no es relevante o que fue generada por error.**

<details>
<summary>📝 Criterios de aceptación (Gherkin) — 5 escenarios</summary>

```gherkin
Funcionalidad: Eliminación de Reporte Financiero

  Escenario: Eliminación exitosa de un reporte
    Dado que el usuario está autenticado y se encuentra en la página de Reportes
    Y existe un reporte para el período "2025-03"
    Cuando el usuario selecciona la opción "Eliminar" para el reporte de "2025-03"
    Entonces el sistema muestra un diálogo de confirmación con el mensaje:
      "¿Estás seguro de que deseas eliminar el reporte del período 2025-03? Esta acción no se puede deshacer."
    Cuando el usuario confirma la eliminación haciendo clic en "Confirmar"
    Entonces el reporte es eliminado del sistema
    Y la tabla de historial de reportes se actualiza y ya no muestra el reporte de "2025-03"
    Y se muestra una notificación de éxito: "Reporte eliminado correctamente"

  Escenario: El usuario cancela la eliminación
    Dado que el usuario está autenticado y se encuentra en la página de Reportes
    Y existe un reporte para el período "2025-05"
    Cuando el usuario selecciona "Eliminar" para el reporte de "2025-05"
    Y el sistema muestra el diálogo de confirmación
    Cuando el usuario hace clic en "Cancelar"
    Entonces el diálogo se cierra
    Y el reporte de "2025-05" permanece intacto en el sistema
    Y la tabla de historial no sufre cambios

  Escenario: Intento de eliminar un reporte del período en curso con transacciones activas
    Dado que el usuario está autenticado
    Y el período actual es "2026-02"
    Y el reporte del período "2026-02" tiene transacciones registradas
    Cuando el usuario intenta eliminar el reporte de "2026-02"
    Entonces el sistema muestra un mensaje de advertencia:
      "No es posible eliminar el reporte del período en curso mientras existan transacciones activas asociadas."
    Y la opción de confirmar la eliminación está deshabilitada

  Escenario: Intento de eliminar un reporte que no existe
    Dado que el usuario está autenticado
    Cuando el sistema intenta procesar una solicitud de eliminación para un reporte inexistente
    Entonces el sistema muestra un mensaje de error:
      "El reporte que intentas eliminar no existe o ya fue eliminado."
    Y la tabla de historial permanece sin cambios

  Escenario: La eliminación falla por un error del sistema
    Dado que el usuario confirma la eliminación de un reporte
    Y ocurre un error interno durante el proceso
    Entonces el sistema muestra un mensaje de error:
      "No fue posible eliminar el reporte. Por favor, inténtalo de nuevo más tarde."
    Y el reporte permanece en el sistema sin cambios
```

</details>

---

### ✨ HU Refinada por la Gema

**Título:** Eliminar Registro de Reporte Mensual

> Como **Usuario Registrado**,  
> quiero **eliminar el registro de un reporte financiero guardado de un mes específico**,  
> para **limpiar mi historial de cierres mensuales y poder regenerarlo manualmente si los datos de mis transacciones han cambiado.**

<details>
<summary>📝 Criterios de aceptación (Gherkin) — 3 escenarios</summary>

```gherkin
Funcionalidad: Gestión de Persistencia de Reportes

  Escenario: Eliminación exitosa de un reporte guardado
    Dado que el usuario está autenticado y tiene un reporte guardado para "2025-03"
    Cuando el usuario selecciona "Eliminar" en el historial de reportes
    Entonces el sistema muestra una advertencia:
      "Si eliminas este reporte, deberás generarlo manualmente de nuevo para ver los totales de este período."
    Y el usuario confirma la acción
    Entonces el registro del reporte se elimina de la base de datos
    Y la vista se actualiza notificando: "Reporte eliminado correctamente."

  Escenario: Restricción por transacciones activas (Soft-delete)
    Dado que el reporte de "2026-02" fue calculado con transacciones que aún están "Activas" en la DB
    Cuando el usuario intenta eliminar el reporte
    Entonces el sistema permite la eliminación pero emite un aviso sobre la persistencia de los datos origen (transacciones).

  Escenario: Intentar eliminar un reporte ya inexistente
    Dado que un reporte fue eliminado previamente o no existe
    Cuando el sistema recibe una petición de borrado para ese ID
    Entonces retorna un error 404: "El reporte no existe."
```

</details>

---

### 🔍 Diferencias Detectadas

| # | Aspecto | Impacto |
|:-:|:--------|:-------:|
| 1 | Cambio de enfoque conceptual | 🟡 Medio |
| 2 | Propósito reformulado | 🟡 Medio |
| 3 | Regla de negocio: transacciones activas | 🔴 Alto |
| 4 | Tono del mensaje de confirmación | 🟢 Bajo |
| 5 | Código de error explícito | 🟡 Medio |

**1. Cambio de enfoque conceptual**
- **Original:** habla de "eliminar un reporte financiero".
- **Refinada:** lo redefine como "eliminar el **registro** de un reporte guardado", enfatizando que es un dato persistido (cierre mensual) y no el cálculo en sí.

**2. Propósito (valor) reformulado**
- **Original:** mantener historial limpio y libre de info irrelevante o errónea.
- **Refinada:** limpiar historial de cierres mensuales **y poder regenerarlo manualmente**. Introduce el concepto de regeneración.

> [!CAUTION]
> **3. Cambio crítico en regla de negocio (transacciones activas)**
> - **Original:** **bloquea** la eliminación del período en curso si hay transacciones activas (botón deshabilitado).
> - **Refinada:** **permite** la eliminación pero advierte sobre la persistencia de los datos origen (enfoque **soft-delete**). Cambio significativo de comportamiento.

**4. Tono del mensaje de confirmación**
- **Original:** _"¿Estás seguro...? Esta acción no se puede deshacer."_
- **Refinada:** _"Si eliminas este reporte, deberás generarlo manualmente de nuevo..."_ (más informativo, orientado a consecuencia).

**5. Código de error explícito**
- La refinada introduce código HTTP `404` en el escenario de reporte inexistente; la original solo describe un mensaje de error sin código.

---
---

## 🔄 US-019 — Recalcular un Reporte Financiero

### 📄 HU Original

**Título:** Recalcular un Reporte Financiero

> Como **Usuario Registrado**,  
> quiero **solicitar la recalculación de un reporte financiero de un período específico**,  
> para **asegurarme de que los totales del reporte reflejen con exactitud todas mis transacciones registradas en ese período.**

<details>
<summary>📝 Criterios de aceptación (Gherkin) — 5 escenarios</summary>

```gherkin
Funcionalidad: Recalculación de Reporte Financiero

  Escenario: Recalculación exitosa de un reporte
    Dado que el usuario está autenticado y se encuentra en la página de Reportes
    Y existe un reporte para el período "2025-11"
    Cuando el usuario selecciona la opción "Actualizar / Recalcular" sobre el reporte de "2025-11"
    Entonces el sistema procesa la solicitud recalculando totalIncome, totalExpense y balance
    basándose en todas las transacciones registradas para ese período
    Y el reporte de "2025-11" muestra los valores actualizados en la tabla
    Y se muestra la notificación: "Reporte del período 2025-11 actualizado correctamente."

  Escenario: El sistema indica estado de procesamiento durante la recalculación
    Dado que el usuario solicita la recalculación de un reporte
    Cuando el sistema está procesando la solicitud
    Entonces el botón "Actualizar / Recalcular" se deshabilita y muestra el estado "Procesando..."
    Y la fila del reporte en la tabla muestra un indicador de carga
    Al completarse, los datos actualizados son mostrados automáticamente

  Escenario: No existen transacciones para el período a recalcular
    Dado que el usuario solicita recalcular el reporte del período "2024-09"
    Y no existen transacciones registradas para ese período
    Entonces el sistema muestra el mensaje:
      "No se encontraron transacciones para el período seleccionado. El reporte no puede ser recalculado."
    Y el reporte permanece sin cambios

  Escenario: La recalculación falla por un error del sistema
    Dado que el usuario solicita la recalculación de un reporte
    Y ocurre un error interno durante el procesamiento
    Entonces el sistema muestra el mensaje:
      "No fue posible actualizar el reporte en este momento. Por favor, inténtalo de nuevo más tarde."
    Y los datos del reporte permanecen sin cambios

  Escenario: Recalcular un reporte que no existe
    Dado que el usuario intenta recalcular un reporte que fue eliminado previamente
    Cuando el sistema procesa la solicitud
    Entonces se muestra el mensaje:
      "El reporte que intentas actualizar no existe."
    Y no se realiza ningún cambio
```

</details>

---

### ✨ HU Refinada por la Gema

**Título:** Recalcular Reporte Financiero (Versión Corregida)

> Como **Usuario Registrado**,  
> quiero **solicitar la recalculación de un reporte financiero de un período específico (mes/año)**,  
> para **sincronizar los totales del reporte y el Balance General con las modificaciones manuales realizadas en mis transacciones y asegurar la exactitud de los datos en mi divisa local.**

<details>
<summary>📝 Criterios de aceptación (Gherkin) — 3 escenarios</summary>

```gherkin
Funcionalidad: Recalculación de Reporte Financiero e Integridad de Datos

  Escenario: Recalculación exitosa con actualización de Balance General
    Dado que el usuario está autenticado y tiene configurada una divisa (ej: "USD")
    Y existe un reporte para el período "2025-11" que requiere sincronización
    Cuando el usuario selecciona "Actualizar / Recalcular"
    Entonces el sistema recalcula totalIncome, totalExpense y balance usando las transacciones actuales
    Y el sistema actualiza automáticamente el Balance General global del Dashboard
    Y los montos se muestran con el símbolo de divisa correspondiente y 2 decimales

  Escenario: Sincronización cuando no existen transacciones (Reset a Cero)
    Dado que el usuario solicita recalcular el reporte del período "2024-09"
    Y se han eliminado todas las transacciones de ese período previamente
    Cuando el sistema procesa la solicitud
    Entonces el reporte debe actualizar sus valores a 0.00 (en lugar de mantener datos obsoletos)
    Y el Balance General debe reflejar esta deducción de forma inmediata

  Escenario: Control de concurrencia en la interfaz (UI)
    Dado que el usuario hace clic en el botón "Actualizar / Recalcular"
    Cuando el proceso se dispara (ejecución rápida en backend)
    Entonces el botón debe bloquearse instantáneamente y cambiar su estado a "Procesando..."
    Para evitar múltiples peticiones en el breve lapso de ejecución
```

</details>

---

### 🔍 Diferencias Detectadas

| # | Aspecto | Impacto |
|:-:|:--------|:-------:|
| 1 | Ampliación del propósito | 🟡 Medio |
| 2 | Comportamiento sin transacciones | 🔴 Alto |
| 3 | Nuevo: Balance General (Dashboard) | 🔴 Alto |
| 4 | Soporte de divisa y formato numérico | 🟡 Medio |
| 5 | Configuración de divisa del usuario | 🟡 Medio |
| 6 | Control de concurrencia (UI) | 🟢 Bajo |
| 7 | Regla de negocio eliminada | 🟡 Medio |

**1. Ampliación del propósito**
- **Original:** asegurar que los totales reflejen las transacciones.
- **Refinada:** **sincronizar el Balance General** del Dashboard además del reporte individual, y añade **soporte de divisa local**. Mayor alcance de impacto.

> [!CAUTION]
> **2. Cambio crítico: comportamiento sin transacciones**
> - **Original:** cuando no hay transacciones, **el reporte permanece sin cambios** y muestra mensaje informativo.
> - **Refinada:** **resetea los valores a 0.00** y actualiza el Balance General inmediatamente. Cambio funcional significativo — evita datos obsoletos.

> [!IMPORTANT]
> **3. Nuevo concepto: Balance General (Dashboard)**
> - La refinada introduce la obligación de actualizar automáticamente el **Balance General global del Dashboard** tras la recalculación. Esto no existía en la original.

**4. Soporte de divisa y formato numérico**
- La refinada exige que los montos se muestren con **símbolo de divisa** correspondiente y **2 decimales**. La original no menciona formato de presentación.

**5. Configuración de divisa del usuario**
- La refinada presupone que el usuario tiene configurada una divisa (ej: `"USD"`). La original no menciona este concepto.

**6. Control de concurrencia (UI) más específico**
- **Original:** botón deshabilitado con "Procesando..." + indicador de carga en fila.
- **Refinada:** botón se **bloquea instantáneamente** para evitar múltiples peticiones. El foco cambia de UX visual a **prevención de race conditions**.

> [!WARNING]
> **7. Regla de negocio eliminada**
> - La original incluye: _"Si el resultado es idéntico al anterior, el sistema notifica que no hubo cambios."_ La refinada no contempla este caso.

---
---

## 📥 US-021 — Descargar Reporte de un Período como PDF

### 📄 HU Original

**Título:** Descargar Reporte de un Período como PDF

> Como **Usuario Registrado**,  
> quiero **descargar el reporte financiero de un período específico en formato PDF**,  
> para **conservar un registro imprimible y compartible de mi actividad financiera mensual.**

<details>
<summary>📝 Criterios de aceptación (Gherkin) — 5 escenarios</summary>

```gherkin
Funcionalidad: Descarga de Reporte en PDF

  Escenario: Descarga exitosa del PDF de un reporte de período
    Dado que el usuario está autenticado y se encuentra en la página de Reportes
    Y existe un reporte para el período "2025-10"
    Cuando el usuario selecciona la opción "Descargar PDF" para el reporte de "2025-10"
    Entonces el sistema genera un documento PDF con los datos del reporte del período "2025-10"
    Y el documento incluye: período, total de ingresos, total de gastos y balance neto
    Y el nombre del archivo descargado es: "reporte-2025-10.pdf"
    Y el archivo se descarga automáticamente en el dispositivo del usuario

  Escenario: El sistema muestra un estado de carga durante la generación del PDF
    Dado que el usuario solicita la descarga del PDF
    Cuando el sistema está generando el documento
    Entonces el botón "Descargar PDF" se deshabilita y muestra "Generando PDF..."
    Y al completarse, la descarga se inicia automáticamente
    Y el botón vuelve a su estado habitual

  Escenario: El reporte no tiene datos suficientes para generar el PDF
    Dado que el usuario intenta descargar el PDF de un reporte que fue eliminado o no existe
    Cuando el sistema procesa la solicitud
    Entonces se muestra el mensaje:
      "No fue posible generar el PDF. El reporte seleccionado no existe."
    Y no se descarga ningún archivo

  Escenario: La generación del PDF falla por error del sistema
    Dado que el usuario solicita la descarga del PDF
    Y ocurre un error interno durante la generación del documento
    Entonces el sistema muestra el mensaje:
      "No fue posible generar el PDF en este momento. Por favor, inténtalo de nuevo más tarde."
    Y no se descarga ningún archivo

  Escenario: Descarga bloqueada para usuario no autenticado
    Dado que el usuario no está autenticado
    Cuando intenta acceder a la funcionalidad de descarga de PDF
    Entonces el sistema lo redirige a la página de inicio de sesión
    Y no se genera ni descarga ningún archivo
```

</details>

---

### ✨ HU Refinada por la Gema

**Título:** Descargar Reporte de un Período como PDF (Refinada)

> Como **Usuario Registrado**,  
> quiero **descargar el reporte financiero de un período específico en formato PDF**,  
> para **conservar un registro imprimible y compartible de mi actividad financiera.**

<details>
<summary>📝 Criterios de aceptación (Gherkin) — 4 escenarios</summary>

```gherkin
Funcionalidad: Generación y descarga de reporte financiero resumido

  Escenario: Descarga exitosa de reporte con múltiples páginas
    Dado que el usuario está autenticado con JWT válido
    Y tiene múltiples transacciones registradas en el período "2025-10"
    Cuando selecciona "Descargar PDF" para dicho período
    Entonces el sistema genera un documento que incluye:
      | Campo          | Detalle                                    |
      | Período        | Formato YYYY-MM                            |
      | Total Ingresos | Sumatoria de transacciones tipo "ingreso"  |
      | Total Gastos   | Sumatoria de transacciones tipo "gasto"    |
      | Balance Neto   | Ingresos menos Gastos                      |
    Y si la información excede una página, el PDF debe generar automáticamente
      páginas adicionales conservando el encabezado.
    Y el archivo se descarga con el nombre "reporte-2025-10.pdf".

  Escenario: Validación de montos con decimales
    Dado que el usuario tiene gastos con céntimos (ej. 150.50)
    Cuando se genera el PDF
    Entonces los montos deben mostrarse con dos decimales y alineación financiera.

  Escenario: Intento de descarga de datos de otro usuario
    Dado que un usuario intenta forzar la descarga de un reporte mediante ID de otro usuario
    Cuando el sistema valida la propiedad del recurso
    Entonces el sistema debe retornar un error 403 (Prohibido)
    Y no se genera ningún documento.

  Escenario: Control de errores en generación
    Dado que el servidor de reportes experimenta un fallo de conexión
    Cuando el usuario solicita el PDF
    Entonces el botón muestra "Error al generar" y se habilita nuevamente tras 3 segundos.
```

</details>

---

### 🔍 Diferencias Detectadas

| # | Aspecto | Impacto |
|:-:|:--------|:-------:|
| 1 | Descripción ligeramente más genérica | 🟢 Bajo |
| 2 | Reducción y reemplazo de escenarios (5 → 4) | 🟡 Medio |
| 3 | Soporte de múltiples páginas en PDF | 🟡 Medio |
| 4 | Seguridad de autorización (403) | 🔴 Alto |
| 5 | Formato de montos con alineación financiera | 🟡 Medio |
| 6 | Cambio en comportamiento de error | 🟡 Medio |
| 7 | Autenticación explícita (JWT) | 🟡 Medio |
| 8 | Contenido del PDF simplificado | 🟡 Medio |
| 9 | Validación INVEST y metadatos eliminados | 🟢 Bajo |

**1. Descripción casi idéntica, ligero cambio**
- **Original:** "actividad financiera **mensual**".
- **Refinada:** "actividad financiera" (sin "mensual"). Más genérica.

**2. Reducción y reemplazo de escenarios (5 → 4)**
- ❌ Se elimina el escenario de **estado de carga** ("Generando PDF...").
- ❌ Se elimina el escenario de **usuario no autenticado** (redirección a login).
- ✅ Se **añade** escenario de **validación de montos con decimales** (alineación financiera).
- ✅ Se **añade** escenario de **autorización cruzada entre usuarios** (error 403).

**3. Nuevo: soporte de múltiples páginas en PDF**
- La refinada exige que si la información excede una página, el PDF genere **páginas adicionales conservando el encabezado**. La original no contempla paginación.

> [!IMPORTANT]
> **4. Nuevo: seguridad de autorización (403)**
> - La refinada incluye validación explícita de **propiedad del recurso**: si un usuario intenta descargar un reporte de otro usuario, el sistema retorna `403 Forbidden`. La original solo cubría el caso de no autenticado (redirección a login).

**5. Nuevo: formato de montos con alineación financiera**
- La refinada exige explícitamente montos con **2 decimales y alineación financiera** en el PDF. La original no especificaba formato numérico.

**6. Cambio en comportamiento de error**
- **Original:** muestra mensaje de error genérico sin mención de recuperación.
- **Refinada:** el botón muestra **"Error al generar"** y se **habilita de nuevo tras 3 segundos**. Introduce tiempo de recuperación automático en la UI.

**7. Autenticación**
- La refinada menciona explícitamente **JWT válido** como mecanismo de autenticación. La original solo dice "autenticado" de forma genérica.

**8. Contenido del PDF simplificado**
- **Original:** lista 6 campos (nombre usuario, período, ingresos, gastos, balance, fecha/hora generación).
- **Refinada:** lista 4 campos en tabla Gherkin (período, ingresos, gastos, balance). Se pierden **nombre del usuario** y **fecha/hora de generación**.

**9. Validación INVEST y metadatos eliminados.**