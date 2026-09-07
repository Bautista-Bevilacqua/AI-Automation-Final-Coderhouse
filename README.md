# Ecosistema Autónomo de Triage y Respuesta de Leads B2B con IA y HITL

Proyecto final integrador de arquitectura de automatización con inteligencia artificial. El sistema resuelve el ciclo completo de ingesta, validación de integridad, clasificación semántica, redacción de propuestas personalizadas y validación humana antes de la salida multicanal.

---

## 🔗 Enlaces del Proyecto

* **Base de Datos (Airtable en modo lectura):** [PEGA_AQUÍ_TU_ENLACE_COMPARTIDO_DE_AIRTABLE]
* **Video Demo (3 min):** [PEGA_AQUÍ_EL_ENLACE_DE_LOOM_O_YOUTUBE]
* **Diagrama de Arquitectura (PDF):** [Ver PDF](./architecture/diagram.pdf)
* **Workflow n8n (JSON):** [Descargar JSON](./workflow/workflow_ecosistema_leads.json)

---

## 🛠️ Stack Tecnológico

1. **Orquestador Principal:** n8n (Node execution engine).
2. **Base de Datos / Memoria:** Airtable (Esquema relacional con control de estados y logs de error).
3. **Motor de IA:** OpenAI (`gpt-4o-mini`) con structured JSON outputs y prompts dinámicos.
4. **Canal de Salida & Validación:** Gmail API (notificación interna HITL y despacho final al cliente).

---

## 📐 Arquitectura y Lógica del Flujo

1. **Trigger Inteligente:** Monitoreo optimizado sobre la tabla `Leads` de Airtable mediante el campo `Created` para evitar ejecuciones redundantes.
2. **Gestión de Errores y Resiliencia (Camino Infeliz):**
   * Validación condicional previa con nodo `If` para verificar integridad de datos obligatorios (`Email`, `Mensaje`).
   * Desvío automático a rama de contingencia que registra `Estado: Error` y detalle en `Log_Error` en la base de datos sin romper la ejecución.
   * Directiva `Continue On Fail` activada en el nodo de OpenAI ante caídas de API o indisponibilidad de servicio.
3. **Procesamiento IA:** 
   * Evaluación de urgencia comercial (`VIP`, `Calificado`, `Descartado`).
   * Redacción adaptativa de propuesta comercial en formato JSON estructurado.
4. **Human-in-the-Loop (HITL):**
   * Pausa de ejecución mediante nodo nativo `Wait` (`On Webhook Call`).
   * Envío de correo al operador humano con el resumen y la URL dinámica de aprobación (`resumeUrl`).
   * Cero posibilidad de "efecto metralleta" o despachos descontrolados sin supervisión.
5. **Salida Multicanal:** Despacho del correo formal al lead y actualización final del registro en Airtable a `Aprobado`.

---

## 📸 Evidencias de Ejecución

### 1. Base de Datos en Airtable
![Airtable DB](./evidences/01_airtable_database.png)

### 2. Flujo Completo en n8n
![n8n Flow](./evidences/02_n8n_flow_overview.png)

### 3. Prueba de Camino Infeliz (Error Handling)
![Error Handling](./evidences/03_test_error_handling.png)

### 4. Notificación y Aprobación Humana (HITL)
![HITL Approval](./evidences/04_hitl_approval_email.png)

### 5. Salida Final al Cliente
![Final Output](./evidences/05_final_customer_email.png)
