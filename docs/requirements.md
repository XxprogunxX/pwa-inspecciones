# Requisitos del producto — completar en Semana 1

## 1. Problema y contexto

En instalaciones educativas y centros de investigación con múltiples laboratorios (cómputo, electrónica, redes y física), el personal técnico realiza rondas periódicas de inspección preventiva y correctiva para evaluar el estado físico de equipos, cableado estructurado, iluminación y condiciones de seguridad.

Actualmente, estos registros suelen realizarse en papel o mediante formularios web convencionales que dependen de una conexión ininterrumpida a Internet. Sin embargo, muchos de estos laboratorios están ubicados en sótanos, naves industriales o estructuras con apantallamiento electromagnético donde la conectividad celular y Wi-Fi es nula o sumamente intermitente.

**Alcance de la Semana 1:**
- Establecer un starter funcional, reproducible y limpio en Next.js (React/TypeScript).
- Presentar la interfaz base con visualización de datos sintéticos de inspecciones recientes.
- Configurar el pipeline de verificación automatizada e integración continua.

**Fuera del alcance en Semana 1:**
- Quedan explícitamente fuera de esta entrega la implementación de Service Worker, archivo Web App Manifest, capacidades de almacenamiento offline (Cache API / IndexedDB), sincronización en segundo plano (Background Sync), notificaciones push y módulos de autenticación de usuarios. Estas capacidades se incorporarán de manera incremental en semanas posteriores.

## 2. Usuarios y escenarios

### Usuarios
- **Técnico/a de Laboratorio (Inspector/a de Campo):** Personal encargado de recorrer las instalaciones, verificar el estado de los puestos de trabajo y registrar observaciones o incidencias.
- **Coordinador/a de Mantenimiento:** Supervisor que revisa el consolidado de inspecciones y programa las reparaciones pertinentes.

### Escenarios observables
- **Escenario 1 (Operación en área con conectividad estable):**
  La Técnica A accede a la aplicación desde su dispositivo en la oficina de coordinación antes de iniciar su turno. La aplicación carga de inmediato la pantalla principal de "Inspecciones de laboratorio", mostrando el resumen de las inspecciones previas, su estado ("Sin incidencias" / "Requiere atención") y el número de hallazgos para planificar su recorrido.
- **Escenario 2 (Operación en entorno con conectividad intermitente o nula):**
  El Técnico B se traslada al sótano donde se ubica el Laboratorio de Electrónica. Al descender, el dispositivo pierde cobertura móvil y la señal Wi-Fi oscila constantemente. El usuario necesita consultar la última inspección registrada de ese laboratorio para verificar si una falla en los bancos de trabajo ya había sido observada. La aplicación debe mantener una presentación estable sin errores críticos de bloqueo por falta de red.

## 3. Requisitos funcionales

- **RF-01 (Visualización de lista de inspecciones):**
  El sistema debe desplegar un listado de las inspecciones registradas, presentando para cada elemento: nombre del laboratorio, fecha de registro, estado visual, responsable asignado y número de hallazgos.
  *Criterio de aceptación:* Al navegar a la ruta raíz `/`, la interfaz debe renderizar las tarjetas correspondientes a las inspecciones existentes en el dataset sintético.

- **RF-02 (Indicadores de estado de inspección):**
  El sistema debe diferenciar claramente el estado operativo de cada inspección mediante badges visuales distintivos (por ejemplo, "Sin incidencias" y "Requiere atención").
  *Criterio de aceptación:* Cada tarjeta debe mostrar una etiqueta visual cuyo estilo semántico refleje el valor del campo `status` del registro.

- **RF-03 (Encabezado y contexto informativo):**
  El sistema debe mostrar en el encabezado la condición actual de la aplicación, indicando explícitamente que los datos son sintéticos y el estado del starter ("ejecutable · PWA aún no implementada").
  *Criterio de aceptación:* La página principal debe contener las etiquetas de contexto del proyecto especificadas para la Semana 1.

## 4. Requisitos no funcionales

- **RNF-01 (Reproducibilidad de instalación y compilación):**
  El proyecto debe poder instalarse y construirse de forma determinista y sin errores utilizando `npm ci` y `npm run build` en entornos compatibles con Node.js 18+.
- **RNF-02 (Rendimiento de carga inicial):**
  El First Contentful Paint (FCP) de la vista principal debe ser inferior a 1.5 segundos en conexiones de red simuladas estándar (Fast 3G / 4G).
- **RNF-03 (Accesibilidad y Semántica):**
  La interfaz debe utilizar elementos HTML5 semánticos (`<header>`, `<main>`, `<article>`) y mantener una relación de contraste mínima de 4.5:1 para texto normal, alineándose con las directrices WCAG 2.1 nivel AA.
- **RNF-04 (Seguridad y Privacidad):**
  El código fuente, configuración y repositorio no deben contener variables secretas, credenciales, contraseñas ni claves de API expuestas.
- **RNF-05 (Diseño Adaptativo / Mobile-First):**
  El diseño debe adaptarse fluidamente a pantallas de dispositivos móviles (viewport a partir de 360px de ancho) y escritorios.
- **RNF-06 (Arquitectura preparada para Offline):**
  El desacoplamiento de componentes y la capa de datos deben permitir la integración futura de Service Workers y almacenamiento local sin requerir reestructuración de la interfaz.

## 5. Datos sintéticos y límites

- **Datos utilizados:**
  Para el desarrollo y pruebas se utiliza exclusivamente información sintética generada de forma determinista en `src/lib/data/inspections.ts`. Incluye identificadores ficticios (`insp-001`, etc.), nombres de laboratorios simulados ("Laboratorio de Redes", "Laboratorio de Electrónica", "Laboratorio de Software"), responsables ficticios ("Técnica A", "Técnico B") y fechas de ejemplo.
- **Límites e información prohibida:**
  Está estrictamente prohibido el uso, almacenamiento o transmisión de datos reales de personal institucional, números de nómina, direcciones IP reales de equipos de red, contraseñas, información confidencial de auditorías reales o credenciales de servicios en la nube.

## 6. Criterios de aceptación de la Semana 1

1. **Instalación y construcción limpia:**
   *Verificación:* Ejecutar `npm ci && npm run build`. Ambos comandos deben finalizar con código de salida 0 sin advertencias críticas.
2. **Pruebas automatizadas del starter:**
   *Verificación:* Ejecutar `npm test`. Debe ejecutar `tests/starter.spec.mjs` y reportar `starter.spec.mjs: PASS`.
3. **Verificación de artefactos requeridos:**
   *Verificación:* Ejecutar `npm run verify` (o `make verify`). Debe validar la existencia de los 10 artefactos obligatorios, finalizar con `Starter verificable: PASS` y generar el archivo `reports/verification.json`.
4. **Validación de seguridad y repositorio:**
   *Verificación:* Ejecutar `bash public-tests/check.sh`. Debe confirmar que no existan secretos ni llaves expuestas y concluir con `PUBLIC_OK`.
