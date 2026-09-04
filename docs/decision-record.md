# ADR-001 — Estrategia de aplicación

## Estado

Aceptada — 2026-09-03

## Contexto y restricciones

El proyecto consiste en desarrollar una aplicación para la gestión e inspección de laboratorios técnicos y universitarios. Los operadores deben realizar levantamiento de información en campo (sótanos, almacenes y laboratorios de cómputo/electrónica), entornos donde la conectividad de red celular y Wi-Fi suele ser intermitente, inestable o inexistente.

Las restricciones clave del proyecto son:
1. **Duración del curso:** Plazo estricto de 14 semanas académicas para diseño, desarrollo, pruebas e iteraciones progresivas.
2. **Dispositivos heterogéneos:** Los inspectores utilizan teléfonos móviles personales y corporativos de diversas marcas y sistemas operativos (Android e iOS) sin garantía de soporte homogéneo de hardware.
3. **Despliegue reproducible:** El sistema debe compilarse, probarse y desplegarse mediante pipelines automatizados (CI/CD) sin depender de entornos de compilación propietarios pesados (como Xcode o Android Studio en servidores locales).
4. **Manejo de datos sintéticos:** El sistema debe operar con datasets sintéticos sin dependencias de infraestructura backend privada compleja en etapas tempranas.

## Alternativas consideradas

### Alternativa 1: Progressive Web App (PWA)
- **Instalación:** Instalable directamente desde el navegador web sin intermediación de tiendas comerciales.
- **Capacidad offline:** Soportada mediante Service Workers, Cache Storage API e IndexedDB para almacenamiento local y sincronización diferida.
- **Distribución:** Actualización instantánea en cada despliegue web vía URLs públicas/privadas; sin ciclos de aprobación de tiendas de aplicaciones.
- **Costo de desarrollo y mantenimiento:** Un único código base basado en estándares web abiertos (HTML, CSS, JavaScript/TypeScript con Next.js/React).
- **Acceso a hardware:** Suficiente para captura de evidencias (cámara, almacenamiento local, estado de red), aunque con limitaciones en APIs avanzadas de bajo nivel.
- **Riesgos:** Diferencias menores en la implementación del ciclo de vida de Service Workers y cuotas de almacenamiento entre Safari (WebKit/iOS) y Chrome (Blink/Android).

### Alternativa 2: Web Tradicional (Single Page Application / Server Rendered)
- **Instalación:** No requiere instalación; acceso exclusivo vía navegador web.
- **Capacidad offline:** Nula. Cualquier corte o intermitencia de red bloquea las peticiones HTTP y detiene la operativa del inspector.
- **Distribución:** Inmediata y centralizada.
- **Costo de desarrollo:** Bajo, similar al starter actual.
- **Acceso a hardware:** Limitado a capacidades básicas del navegador; no permite ejecución en segundo plano.
- **Riesgos:** Inviabilidad operativa en sótanos y zonas con conectividad degradada, incumpliendo el escenario principal del producto.

### Alternativa 3: Aplicación Nativa (Kotlin/Android y Swift/iOS)
- **Instalación:** Descarga e instalación a través de Google Play Store y Apple App Store.
- **Capacidad offline:** Excelente; control total sobre bases de datos locales (SQLite/Room/CoreData) y persistencia en el dispositivo.
- **Distribución:** Lenta; requiere empaquetado específico, firma de binarios (.apk/.aab, .ipa) y tiempos de revisión de tiendas de aplicaciones.
- **Costo de desarrollo y mantenimiento:** Muy alto; requiere mantener dos bases de código separadas y contar con herramientas especializadas (macOS para iOS).
- **Riesgos:** Incompatible con el límite de tiempo de 14 semanas y requerimiento de CI ligera y universal para el curso.

### Alternativa 4: Aplicación Multiplataforma Híbrida (Flutter / React Native)
- **Instalación:** Requiere empaquetado binario e instalación mediante tiendas o side-loading.
- **Capacidad offline:** Muy buena mediante plugins de almacenamiento local y gestión de estado.
- **Distribución:** Compleja en entornos de evaluación rápida; la automatización en CI requiere runners pesados con SDK de Android y CocoaPods/Xcode.
- **Costo de desarrollo:** Moderado; un solo lenguaje/framework, pero con complejidad adicional en puentes nativos y resolución de dependencias de build.
- **Riesgos:** Alto riesgo de cuellos de botella en la configuración del pipeline de evaluación y entrega continua para los estudiantes.

### Matriz comparativa

| Criterio | PWA | Web Tradicional | App Nativa | Multiplataforma |
| :--- | :---: | :---: | :---: | :---: |
| **Soporte Offline** | Alto (Semana 2+) | Nulo | Completo | Completo |
| **Tiempo de desarrollo (14 sem)** | Óptimo | Rápido | Inviable | Ajustado |
| **Distribución sin fricción** | Inmediata | Inmediata | Lenta (Tiendas) | Lenta (Tiendas) |
| **Costo de mantenimiento** | Bajo (1 código) | Bajo (1 código) | Alto (2 códigos) | Medio |
| **Pipeline CI/CD ligero** | Excelente | Excelente | Complejo | Complejo |

## Decisión

Se selecciona **Progressive Web App (PWA)** basada en **Next.js / React / TypeScript**.

**Justificación:**
1. Es la única alternativa que combina desarrollo ágil con un solo código base estándar, despliegue continuo reproducible en GitHub Actions y capacidad de operar fuera de línea mediante Service Workers e IndexedDB.
2. Permite a los usuarios instalar el aplicativo en sus dispositivos móviles sin necesidad de cuentas de desarrollador de Apple o Google.
3. Permite un enfoque incremental: en la Semana 1 se valida la base ejecutable y el contrato de datos sintéticos, habilitando la incorporación progresiva de capacidades offline en las semanas subsecuentes.

**Qué no resuelve todavía (Semana 1):**
Esta decisión estratégica NO implementa en la Semana 1 el Service Worker, el Web App Manifest, el almacenamiento en IndexedDB, la sincronización en segundo plano ni la autenticación. En la entrega actual, la aplicación opera como una base web ejecutable en preparación para la capa PWA.

## Consecuencias y riesgos

### Consecuencias positivas
- Ciclo de retroalimentación inmediato en el desarrollo y en la integración continua.
- Máxima portabilidad entre sistemas operativos de escritorio y móviles.
- Cumplimiento de estándares abiertos auditables mediante Lighthouse y scripts de prueba en Node.js.

### Costos y riesgos técnicos
- **Riesgo:** Restricciones de almacenamiento y desalojo de caché en iOS/WebKit si el dispositivo tiene poco espacio o pasa periodos prolongados sin uso.
  *Mitigación:* Diseñar una estrategia de persistencia conservadora enfocada en datos esenciales estructurados (JSON) y validar el estado de sincronización.
- **Riesgo:** Complejidad en la invalidación de caché al desplegar nuevas versiones del código.
  *Mitigación:* Adoptar estrategias de caching controladas (`Stale-While-Revalidate` para datos y `Network-First` o hashes de versión para assets estáticos) a partir de la implementación del Service Worker.

## Validación

La validez de esta decisión arquitectónica se verificará mediante:
1. **Semana 1:** Ejecución exitosa de la suite de verificación básica (`make verify` / `npm run verify` y `public-tests/check.sh`).
2. **Semanas 2 a 4:** Incorporación progresiva y auditoría de Service Worker con Lighthouse PWA Check (puntuación > 90 en categoría PWA).
3. **Semanas posteriores:** Pruebas automatizadas de corte de red (Network Emulation Offline en pruebas end-to-end) confirmando que la aplicación carga y permite registrar inspecciones sin conexión activa.
## Contribución de Hernández Camacho Carlos Eduardo

Se realizó el análisis comparativo de las alternativas de arquitectura
consideradas para el proyecto, evaluando sus ventajas, costos, riesgos y
viabilidad dentro del periodo académico de 14 semanas.

La decisión adoptada fue utilizar una Progressive Web App (PWA) basada en
Next.js, React y TypeScript, considerando especialmente la necesidad de
trabajar con conectividad intermitente y mantener un pipeline de desarrollo
y validación reproducible.

También se identificaron como riesgos técnicos las diferencias de
almacenamiento y comportamiento del caché entre navegadores, especialmente
en dispositivos iOS/WebKit, por lo que se propone una estrategia de
persistencia conservadora para las siguientes semanas.