# Evidencia individual — completar antes de entregar

- Nombre: Hernández Méndez Javier
- Repositorio y commit evaluado: https://github.com/XxprogunxX/pwa-inspecciones @ docs
- Mi contribución concreta: Configuración del repositorio starter base en Next.js con TypeScript, especificación técnica de requisitos del sistema y escenarios con conectividad intermitente en docs/requirements.md, y elaboración del registro de decisión arquitectónica ADR-001 en docs/decision-record.md comparando PWA frente a alternativas nativas y web tradicional.
- Decisión técnica que puedo explicar: Justificación arquitectónica de la adopción de Progressive Web App (PWA) frente a Web tradicional y Apps Nativas, analizando trade-offs de costo de desarrollo en 14 semanas, distribución sin tiendas, reproducibilidad en CI/CD y estrategia incremental para soporte offline.
- Comando o prueba que ejecuté y resultado: Ejecución de `npm run verify` (PASS con reporte en `reports/verification.json`), `npm test` (PASS en `tests/starter.spec.mjs`) y `bash public-tests/check.sh` (resultado: PUBLIC_OK).
- Limitación o riesgo que encontré: En esta Semana 1 la aplicación no implementa todavía Service Worker ni almacenamiento local (IndexedDB), por lo que ante una pérdida de red no se pueden sincronizar nuevos datos hasta las siguientes semanas.
- Uso de IA (herramienta, propósito, fragmentos influenciados y validación humana): Antigravity AI utilizada como asistente para estructuración y redacción técnica de los documentos `docs/requirements.md` y `docs/decision-record.md`, con validación humana en la definición de alcances, revisión de coherencia en los requisitos y comprobación manual de la ejecución del starter y suites de prueba.
